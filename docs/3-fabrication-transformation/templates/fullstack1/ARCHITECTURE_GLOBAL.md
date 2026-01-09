# Architecture (vue d'ensemble)

## Périmètre
- Frontend : React + Vite + TypeScript + Primer.
- Backend : Node/TypeScript + Feathers (Express) + Sequelize + PostgreSQL.
- Infra : Docker / docker-compose pour le dev local ; répertoire `infra/` pour IaC et déploiement (placeholder pour l'instant).

## Couche front
- App Shell : Header (ThemeToggle + A propos), Sidebar, Footer.
- Routing : React Router (AppRouter) + Breadcrumbs.
- UI : @primer/react, thèmes light/dark.
- Données : Axios + React Query ; validation Zod ; mocks MSW en dev.
- State : Zustand pour l’UI (préférences), React Query pour les données serveur.

## Couche back
- Serveur Feathers/Express, schémas Zod, Swagger/OpenAPI.
- Architecture en couches : API → Application → Domaine → Infrastructure (Sequelize/PostgreSQL).
- Logging : pino (pino-pretty en dev).

## Cross-cutting
- Gestion des environnements : variables `VITE_*` pour le front (build-time, publiques) ; variables backend dans `.env.dev`/compose.
- Build metadata : version (package.json), git SHA, build date, injectées automatiquement au build front.
- Docker : multi-stage (build Node → runtime nginx pour le front, image backend dédiée).