# Agents

## Langue (obligatoire)
- Toutes les réponses de l’agent DOIVENT être en français.
- Exceptions autorisées : code, identifiants (API, classes, variables) et citations externes.
- Si le prompt est entièrement en anglais, l’agent DOIT refuser et demander une version française.

## Consignes MCP (serveurs actuellement installés)

### Règles générales
- Si une question touche un domaine couvert par un serveur MCP, **utiliser d’abord l’outil MCP correspondant** avant d’inférer ou d’écrire du code.
- Si une info est incertaine ou versionnée, **privilégier l’outil MCP** plutôt que la mémoire.
- Si un outil MCP ne donne rien (ex: ressource vide), **continuer avec la meilleure alternative** et signaler la limite.

### context7 (docs libs)
- Utiliser Context7 **dès qu’une lib/framework est explicitement mentionné** ou qu’un exemple dépend d’une API externe.
- Si la lib est connue, **fournir l’ID direct** (`/org/project`) pour éviter `resolve-library-id`.
- Cibler la version si elle est donnée; sinon demander la version.

### github (repo distant)
- Pour lire/écrire des fichiers d’un repo distant, **utiliser MCP GitHub**.
- Préférer `get_file_contents` pour inspection et `create_or_update_file`/`push_files` pour modifications.

### postgres (base de données)
- Pour toute question DB (schéma, tables, données, perf), **utiliser MCP Postgres d’abord**.
- Préférer `list_schemas`/`list_objects` pour découvrir, `execute_sql` pour requêtes, `explain_query` pour plans.

### snyk-security (sécurité)
- Pour scans SAST/OSS/IaC/containers, **utiliser MCP Snyk**.
- Ne pas lancer de scan coûteux sans confirmation explicite.

## Hors MCP
- Si la question dépend d’informations externes à jour, **utiliser web.run**.

## Active Technologies

## Recent Changes
