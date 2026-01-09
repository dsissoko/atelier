# Compute — <COMPUTE-XXX> <Nom court et explicite>

## Contexte / objectif
- Fonction métier visée :
- Déclencheur : (UI, event, cron, webhook…)
- Résultat attendu (observable) :
- Portée : backend / frontend / shared (lib)

## Règles métier (le cœur)
- Invariants :
  - …
- Validations :
  - …
- Décisions / calculs :
  - …
- Permissions :
  - …

## Workflow
1. Entrée (données minimales)
2. Étapes de traitement (ordre strict)
3. Sortie (résultat / event)
4. Effets de bord (si applicable)

## Données touchées
- Entités lues :
- Entités écrites :
- Concurrence / idempotence (si nécessaire)

## Observabilité
- Logs attendus :
- Erreurs métier :
  - 4xx :
  - 5xx :
- Retry (oui/non, conditions)

## Tests minimaux
- Happy path :
- Erreur métier principale :
- Cas limite :
