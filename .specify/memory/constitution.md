<!--
Rapport d'impact de synchronisation
- Version : 0.1.0 → 0.1.1
- Principes modifiés : III. Conventions de développement et de spécification (alignement avec la constitution racine) ; IV. Invariants techniques pilotés par template (clarification des références)
- Sections ajoutées : aucune
- Sections supprimées : aucune
- Templates à mettre à jour : ✅ .specify/templates/plan-template.md ; ✅ .specify/templates/spec-template.md ; ✅ .specify/templates/tasks-template.md
- Actions à suivre : aucune
-->

# Constitution (générique)

## Principes fondamentaux

### I. Expérience Utilisateur (UX) ciblée
L’application DOIT privilégier la rapidité, la lisibilité et l’efficacité pour un usage
intensif. L’interface DOIT rester responsive et accessible, sans compromis sur la vitesse
ou la clarté des actions clés.

### II. Langue et communication
- Toute documentation, spécification, ticket et prompt IA DOIT être rédigé en français.
- L’IA DOIT répondre exclusivement en français.
- Exceptions autorisées : code, identifiants (API, classes, variables) et citations externes.
- Une spécification ou un prompt entièrement en anglais DOIT être refusé.
- Les résumés, confirmations et messages de suivi DOIVENT aussi être en français.

### III. Conventions de développement et de spécification
Le document `docs/2-conception-systeme/spec-convention.md` est la source unique de vérité pour la rédaction des
specs Speckit (écrans, APIs, entités). En cas de conflit avec un autre document, ses règles
priment. Toute spec DOIT être traçable, testable et rédigée en français avec des récits utilisateurs
indépendants et des scénarios Étant donné / Quand / Alors.

### IV. Invariants techniques pilotés par template
- Les invariants techniques DOIVENT être définis par le template actif.
- Les specs Speckit DOIVENT proposer uniquement des architectures compatibles avec la stack du
  template actif.
- Les APIs et entités DOIVENT rester cohérentes avec les conventions techniques du template actif.

### V. Simplicité et justification des écarts
Tout écart aux principes ou toute complexité ajoutée DOIT être justifié dans le plan ou la
spec concernée, avec une alternative plus simple explicitement évaluée.

## Template actif
- Template sélectionné : **fullstack1**
- Annexe technique : `docs/3-fabrication-transformation/templates/fullstack1/constitution-annexe.md`

## Organisation des spécifications Speckit
- Dossiers de spec par story : `specs/<nom-story>/` regroupant la story, les écrans (screen-spec-*), les data model (data-model-spec-*), les traitements (compute-spec-*),
- Templates obligatoires dans `docs/2-conception-systeme/templates/`

## Handoff & reprise
- À la fin de chaque session, `docs/START-HERE.md` DOIT être mis à jour avec la fonctionnalité en cours
  (ou **AUCUNE** si rien n’est actif).

**Version** : 0.1.1 | **Ratifiée** : 2025-11-23 | **Dernière modification** : 2026-01-09
