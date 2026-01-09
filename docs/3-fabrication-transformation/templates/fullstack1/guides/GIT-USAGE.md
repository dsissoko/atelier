# Guide Git — Participation au dépôt [NOM_PROJET]

Deux rôles interviennent :
- **Développeur** : implémente une feature sur sa branche dédiée, pousse son travail et ouvre la PR.
- **Intégrateur** : relit les PR, demande des ajustements si besoin et merge sur `main`
  (ou décide du sort des branches).

> Les 2 rôles peuvent être joués par la même personne.

Versionnage : `main` est taggée après chaque merge de PR validé, en suivant SemVer (`x.y.z`).
Le point de départ du projet est le tag initial `v0.0.0` (créé avant la première PR).

Les branches de feature sont créées par le développeur. Dans tous les cas, **une seule
branche par feature** est utilisée jusqu’à la PR.

Après le merge, la branche de feature est conservée pour archive. Elle pourra être supprimée
ultérieurement lors d’opérations de nettoyage sporadiques.

## 1) Cloner et préparer le dépôt

```bash
git clone <URL_DU_REPO>
cd <DOSSIER_DU_REPO>
git remote -v             # vérifier l’URL
git branch --show-current # vérifier la branche active (souvent main)
git status                # état initial propre ?
```

Création de la branche feature :
- **Manuel** : depuis `main` à jour (`git pull`), créer et basculer sur la branche :
  ```bash
  git switch -c feature/<NOM_FEATURE>
  ```

Les itérations se font **uniquement** sur cette branche (pas de branches intermédiaires).

## 2) Développer sur la branche de feature

Travaille, teste, vérifie fréquemment :
```bash
git status   # fichiers modifiés ?
git diff     # relire avant commit
```

## 3) Commit et push

Quand la feature est prête (ou pour un incrément cohérent) :
```bash
git add .                        # ou fichier par fichier
git status                       # vérifier la staging area
git commit -m "Message clair"    # ex : "Add README for <NOM_PROJET>"
git push -u origin feature/<NOM_FEATURE>
```

## 4) Ouvrir la Pull Request

- Depuis l’UI GitHub/GitLab : comparer `feature/<NOM_FEATURE>` → `main`,
  décrire le changement et les tests faits.
- Avec GitHub CLI (optionnel) :
  ```bash
  gh pr create --fill
  ```

## 5) Rôle de l’intégrateur

- Relit la PR, demande d’éventuelles modifications en échangeant directement avec le
  développeur.
- Merge dans `main` quand c’est validé.
- Tag la branche main par exemple :

```bash
git tag -a v0.2.0 -m "v0.2.0"
git push origin v0.2.0
```

- Décide de la gestion/suppression des branches distantes (le développeur ne les supprime pas).

### Routine de version applicative (backend/front)
- Juste avant la release : bumper la version dans les `package.json` concernés
  (backend, frontend si applicable) avec `npm version`.
- Exemple release patch backend uniquement :
  ```bash
  cd backend
  npm version patch   # met à jour package.json et package-lock.json, crée un commit et un tag vX.Y.Z
  git push origin HEAD --tags
  ```
- Exemple release patch frontend uniquement :
  ```bash
  cd frontend
  npm version patch   # met à jour package.json et package-lock.json, crée un commit et un tag vX.Y.Z
  git push origin HEAD --tags
  ```
- Exemple release coordonnée front + back (monorepo) :
  ```bash
  cd backend && npm version minor && cd ..
  cd frontend && npm version minor && cd ..
  git add backend/package*.json frontend/package*.json
  git commit -m "chore: bump versions"
  git tag -a v0.3.0 -m "v0.3.0"
  git push origin HEAD --tags
  ```
- Toujours tagger sur le commit qui contient le bump de version. Le build/artefact doit
  partir du tag.
- Les versions affichees en runtime proviennent automatiquement des `package.json` :
  cote front (constante injectee par Vite), cote back (/api/info et page racine).
  Pas de saisie manuelle dans le code ou le .env.

### Environnements (backend)
- `NODE_ENV` est binaire pour Node/Express : `production` active les optimisations
  (logs JSON, mode prod). Toute autre valeur est consideree comme dev.
- `APP_ENV` sert a nommer l’environnement fonctionnel (dev/staging/preprod/prod) et est
  affiche dans `/` et `/api/info`. Laisser `NODE_ENV=production` en staging/preprod/prod
  pour garder le comportement prod, mais ajuster `APP_ENV` selon le contexte.

## Git tips

- **Upstream (tracking branch)** : lie une branche locale a sa branche distante pour eviter
  de preciser le remote/branche a chaque commande.
  - Exemple : a la creation/premier push d’une feature :
    ```bash
    git push -u origin feature/<NOM_FEATURE>
    ```
    ou si la branche existe deja cote remote :
    ```bash
    git branch --set-upstream-to=origin/feature/<NOM_FEATURE> feature/<NOM_FEATURE>
    ```
  - Interet : ensuite `git push` / `git pull` sans arguments utilisent automatiquement
    `origin/feature/<NOM_FEATURE>`, ce qui reduit les erreurs et la frappe repetitive.
