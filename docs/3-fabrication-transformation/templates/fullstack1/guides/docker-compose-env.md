# Docker Compose et variables d’env

Objectif : ne plus confondre les sources d’ENV. Comprendre qui lit quoi, quand, et éviter les warnings `POSTGRES_*`.

## 1) Deux phases, deux sources

- **Substitution YAML (phase 1)**  
  Compose remplace chaque `${VAR}` dans `docker-compose.yml`. Sources lues : `.env` (auto), exports shell, fichier passé en `--env-file`.  
  👉 Si `${VAR}` est vide ici, warning immédiat.

- **Injection conteneur (phase 2)**  
  Après la substitution, juste avant de lancer chaque service, Compose lit les fichiers listés dans `env_file:` et ajoute ces variables dans l’environnement **interne** du conteneur.  
  👉 `env_file:` n’influe pas sur la substitution ; il ne fait pas disparaître un warning de la phase 1.

## 2) Piège classique : `--env-file` ≠ `env_file:`

Dans notre YAML, `postgres` déclare :
```yaml
environment:
  POSTGRES_USER: ${POSTGRES_USER}
  POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
  POSTGRES_DB: ${POSTGRES_DB}
env_file:
  - ${ENV_FILE:-.env.dev}
```

- Phase 1 : `${POSTGRES_*}` doit être déjà renseigné (shell, `.env`, ou `--env-file`).  
- Phase 2 : le `env_file:` (avec `${ENV_FILE}`) est lu seulement pour l’environnement **interne** du conteneur.

Exemple piégeux : `ENV_FILE=.env.prod docker compose --profile localdb up --force-recreate`  
→ `.env.prod` n’est pas lu pour la substitution, `${POSTGRES_*}` reste vide, warnings. Le `env_file:` ne rattrape rien.

## 3) Commandes recommandées (choisir une stratégie)

- **Stratégie A : passer le fichier à la commande (simple)**  
  ```
  docker compose --env-file .env.prod --profile localdb up -d --force-recreate
  ```
  (ou `.env.dev` selon le cas). Pas besoin de `ENV_FILE`, les `POSTGRES_*` sont disponibles en phase 1.

- **Stratégie B : exporter les variables (CI ou shell)**  
  ```
  export POSTGRES_USER=... POSTGRES_PASSWORD=... POSTGRES_DB=...
  docker compose --profile localdb up -d --force-recreate
  ```
  Pas de `--env-file` si tout est exporté.

- **Option de confort (si tu veux des defaults dans le YAML)**  
  ```
  POSTGRES_USER: ${POSTGRES_USER:-postgres}
  POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-postgres}
  POSTGRES_DB: ${POSTGRES_DB:-postgres}
  ```
  Pas de warning même sans export. À n’utiliser que si ces defaults sont acceptables.

## 4) Cas build

Pendant `docker compose build`, seule la phase 1 existe : Compose fait la substitution avec ce qui est disponible (export, `.env`, `--env-file`). Les `env_file:` des services ne jouent aucun rôle pendant le build.

Commande type (avec variables du build) :
```
docker compose --env-file .env.prod build frontend backend --no-cache
```

## 5) Cas run

- Phase 1 : substitution (exports, `.env`, `--env-file`).  
- Phase 2 : `env_file:` ajoute des variables dans le conteneur.  
- Au runtime : environnement = phase 1 + `env_file:`.

## 6) Distinguer les `.env` (racine vs backend/.env vs frontend/.env)

- **Racine (`.env.dev`, `.env.prod`)** : pour Compose (substitution + `environment:`/build args). Toujours passer le fichier via `--env-file ...`.
- **Backend (`backend/.env`)** : lu uniquement si `NODE_ENV !== "production"` (dev local hors Docker). En prod/Docker, `dotenv` ne charge rien → passer les vars via l’environnement/`--env-file`.
- **Frontend Vite (`frontend/.env*`)** : en dev local (`npm run dev`), Vite lit ces fichiers mais seulement les `VITE_*`. En build Docker, les valeurs viennent des `ARG`/`ENV` du `frontend/Dockerfile` (ex. `VITE_ENV`, `VITE_API_BASE_URL_ITEMS`, `VITE_ENABLE_MSW`, etc.). Si tu ne fournis rien, les defaults du Dockerfile s’appliquent (ex. `VITE_ENV=prod`).
