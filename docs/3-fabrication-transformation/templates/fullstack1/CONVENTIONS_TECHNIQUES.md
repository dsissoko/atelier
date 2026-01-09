# Conventions techniques — Template fullstack1

## Objectif
Ce document centralise les conventions techniques détaillées du template **fullstack1**. La constitution ne doit en garder qu’une référence.

## 1) Stack et outillage

### Frontend
- React 18 + TypeScript + Vite.
- UI : @primer/react, @primer/primitives, @primer/octicons-react.
- Data : @tanstack/react-query, axios.
- State UI : zustand.
- Validation : zod.
- Forms : react-hook-form.
- Routing : react-router-dom.
- i18n : i18next, react-i18next.
- Tests : vitest, @testing-library/*, jsdom.
- Mocks API : msw.

### Backend
- Node.js + TypeScript.
- Feathers v5 (Express) pour les services REST.
- Sequelize v6 + PostgreSQL (pg, pg-hstore).
- Validation : zod (quand nécessaire), sinon validation ORM.
- Docs API : @loudsrl/feathers-swagger.
- Sécurité : helmet, cors.
- Logging : pino, pino-http.
- Upload : multer.
- Import : xlsx.
- Tooling : tsx (dev), sequelize-cli (migrations/seed).

### Infra / Dev
- Docker / docker-compose (stack locale, Postgres, pgAdmin).
- Variables via `.env.dev` / `.env.prod` à la racine.

## 2) Structure de dépôt (canonique)

### Racine
```
/ (repo)
  backend/
  frontend/
  docker-compose.yml
  .env.dev
  .env.prod
  docs/
  infra/
  scripts/
```

### Frontend
```
frontend/
  src/
    assets/
    components/
      ui/
    error/
    layout/
    mocks/
    models/
    pages/
    router/
    services/
    store/
    styles/
    theme/
    utils/
    validation/
```

### Backend
```
backend/
  src/
    app.ts
    index.ts
    adapters/
    config/
    docs/
    hooks/
    middleware/
    models/
    services/
    validation/
```

## 3) Conventions Frontend

### Pages et UI
- Les pages vivent dans `src/pages/<feature>`.
- Layout global assuré par `AppShell` ; les pages ne modifient pas le shell.
- Utiliser `PageLayout` + `Panel` + `Stack` pour les pages.
- `src/components/ui/` contient les composants réutilisables (EmptyState, SpinnerLoader, ConfirmButton, Panel, etc.).
- Les tokens (espacements, largeurs) sont centralisés dans `src/theme/`.

### Données / API
- Appels via `axios` (HTTP client centralisé dans `src/services/`).
- Caching et mutations via TanStack Query.
- Les modèles métiers sont définis dans `src/models/` et validés avec Zod dans `src/validation/`.

### Mocking
- MSW activé par `VITE_ENABLE_MSW === "true"`.
- Handlers dans `src/mocks/` alignés sur la forme API.

## 4) Conventions Backend

### Services
- Services Feathers sous `/api/v1/<feature>`.
- Déclaration des services dans `src/services/`.
- Modèles Sequelize dans `src/models/`.
- Hooks dans `src/hooks/`.

### Validation
- Validation métier via Zod si nécessaire.
- Validation ORM côté Sequelize pour les contraintes simples.

### Swagger / Docs
- Swagger UI : `/api/docs` (alias `/docs`).
- Spéc OpenAPI : `/swagger.json`.

### Logging
- pino-http avec `LOG_LEVEL`.
- Ne pas logger `/health` et `/favicon.ico`.

## 5) Environnements & variables

### Frontend (Vite)
- `VITE_API_BASE_URL_*` : base URL API (si vide, proxy local).
- `VITE_ENABLE_MSW` : activation MSW.
- `VITE_SSE_URL`, `VITE_WS_URL` : URLs realtime.
- `VITE_ENV`, `VITE_APP_VERSION`, `VITE_APP_GIT_SHA`, `VITE_APP_BUILD_DATE` : métadonnées.

### Backend
- `PORT` : port HTTP (3001 par défaut).
- `DATABASE_URL` : connexion Postgres.
- `LOG_LEVEL` : niveau pino.
- `NODE_ENV` : `development`/`production`.

## 6) Conventions API
- REST JSON, pagination Feathers (`$limit`, `$skip`).
- Versionnement via préfixe `/api/v1`.
- Healthcheck : `/health`.
- Infos build/env : `/api/info`.

## 7) Tests
- Front : vitest + testing-library + msw si besoin.
- Back : tests ciblés par service (à définir selon la feature).

## 8) Docker / Compose
- Utiliser `--env-file .env.dev` ou `.env.prod` pour la substitution Compose.
- Rebuild + `--force-recreate` quand les variables changent.

## 9) Infra / IaC
- Le répertoire `infra/` contient l’ensemble des scripts d’Infrastructure as Code.
- Toute modification d’infra doit être versionnée et documentée dans ce dossier.
