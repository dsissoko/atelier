# Docker Compose : commandes usuelles et cas pratiques

Objectif : avoir sous la main les commandes que nous utilisons (build, run, DB seule, nettoyage) et rappeler quand forcer la recréation pour éviter de conserver un ancien environnement.

## 1) Build

- Build dev/prod (front + back) en prenant les variables depuis un fichier :  
  `docker compose --env-file .env.dev build frontend backend --no-cache`  
  (prod : `.env.prod`).  
- Note : pendant le build, seules les variables disponibles pour la substitution comptent (`--env-file`, exports, `.env` auto). Les `env_file:` du YAML ne jouent pas ici.

## 2) Run (stack complète)

- Dev : `docker compose --env-file .env.dev --profile localdb up -d --force-recreate`  
  (lance backend + frontend + postgres + pgadmin).  
- Prod-like : `docker compose --env-file .env.prod --profile localdb up -d --force-recreate`.  
- Pourquoi `--force-recreate` ? Pour éviter de réutiliser un conteneur créé avec un ancien environnement (APP_ENV, NODE_ENV, VITE_ENV, etc.).

## 3) Run (DB seule)

- Dev : `docker compose --env-file .env.dev --profile localdb up -d --force-recreate postgres pgadmin`  
  puis migrations/seed côté `backend/`.  
- Prod-like : `docker compose --env-file .env.prod --profile localdb up -d --force-recreate postgres pgadmin`.

## 4) Rebuild + run en une seule passe

- `docker compose --env-file .env.dev up --build -d --force-recreate`  
  (idem avec `.env.prod`). Utile après changement de deps/code ou de variables de build.

## 5) Inspecter / vérifier l’environnement

- Voir l’env injecté dans un service :  
  `docker compose --env-file .env.dev exec backend env | grep -E 'APP_ENV|NODE_ENV|DATABASE_URL'`
- Voir la config interpolée :  
  `docker compose --env-file .env.dev config`

## 6) Nettoyage

- Arrêter et supprimer conteneurs (sans toucher aux volumes) :  
  `docker compose --env-file .env.dev down`
- Supprimer conteneurs + volumes nommés :  
  `docker compose --env-file .env.dev down -v`
- Supprimer images obsolètes (dangling) :  
  `docker image prune`  
  (ou `docker system prune` si tu veux nettoyer conteneurs/volumes réseaux non utilisés, attention à l’effet global).

## 7) Rappels clés

- Substitution vs injection : `--env-file` (ou exports/`.env`) est lu pour remplacer `${VAR}` dans le YAML. `env_file:` dans le YAML est lu plus tard, juste avant de démarrer le conteneur.  
- En prod/Docker, le backend ne lit pas `backend/.env` (dotenv désactivé si `NODE_ENV=production`). Vite en build lit les `ARG/ENV` du Dockerfile, pas `frontend/.env*`.  
- Si tu changes d’ENV ou de variables critiques (APP_ENV, NODE_ENV, VITE_ENV, POSTGRES_*), reconstruis et lance avec `--force-recreate` pour éliminer les états précédents.
