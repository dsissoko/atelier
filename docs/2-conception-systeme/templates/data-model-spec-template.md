# DATA model — <ENTITY-XXX> <Nom court et explicite de l'entité>

## Contexte / objectif
- Entité métier :
- Cas d’usage principaux :
- Relations avec d’autres entités :

## Classe / schéma proposé
- Champs (nom, type, contraintes : notNull, unique, longueur, formats).
- Valeurs par défaut.
- Index / clés uniques.
- Relations (one-to-many, many-to-many, contraintes de cascade).
- Règles métier côté backend (validation Sequelize, hooks).

## Exemples d’objets
- Exemples JSON réalistes couvrant les cas standards + bord.
- Variantes (champs optionnels, erreurs attendues).

## Données de référence
- Listes fermées / tables de ref (codes, labels, statuts).
- Comment sont-elles créées/seedées (migration, script, config) ?
- Règles de maintien (ajout/suppression/activation).

## Impacts base / migrations
- Migration à créer (nouvelle table, colonne, index).
- Données existantes : reprise / migration / backfill ?
- Stratégie de rollback.

## Risques / points de vigilance
- Performance (index, volumes).
- Sécurité (données sensibles, permissions).
- Cohérence des relations et des validations.
