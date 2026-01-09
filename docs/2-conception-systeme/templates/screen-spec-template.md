# Template de spécification d’écran

## Contexte / objectif
- Fonction métier visée :
- Utilisateurs cibles / parcours concernés :
- Valeur attendue :

## Contenu de l’écran
- Titre / sous-titre / message introductif :
- Sections / panneaux (ordre, titres, description courte) :
- États à gérer : loading, empty, succès, erreurs (messages explicites), droits d’accès.
- Actions disponibles (boutons, liens) et navigation (routes, redirections).
- Données affichées : champs, formats, tri/filtre/pagination.
- Validation UX : formats, longueurs, contraintes métier visibles.

## API / données consommées
- Endpoints REST/WS/SSE utilisés (méthodes, params, payloads).
- BaseURL / proxy : `VITE_API_BASE_URL_<FEATURE>` ou appels relatifs.
- Contrats de données (DTO) attendus : lister les champs utilisés.

## Composants / layout
- Layout : `PageLayout`, `Panel/Stack`, tokens (espacements/hauteurs) si applicable.
- Composants métier ou génériques à utiliser ou créer.
- Accessibilité : focus, lecture écran, labels, contrastes.

## Critères d’acceptation
- [ ] …
- [ ] …
- [ ] …

## Impacts / risques
- Navigation ou routes à ajouter/modifier.
- Données sensibles ? performance ? montée en charge ?

## Validation
- Tests manuels clés (scénarios happy path + erreurs).
- Tests auto envisagés (unitaires, intégration, e2e).
