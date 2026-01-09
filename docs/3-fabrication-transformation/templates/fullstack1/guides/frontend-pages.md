# Guide rapide : structurer une page frontend

Objectif : aider à choisir les bons composants selon le type de contenu (layout vs rendu visuel) dans le template.

## Layout vs rendu visuel
- `Box` (Primer) : conteneur générique pour le layout (flex/grid/marges). Par défaut, pas de style visuel.
- `Panel` (custom) : `Box` pré-stylé (bordure, fond subtil, padding) pour encadrer un bloc de contenu ou une carte. À utiliser dès qu’on veut un encadré visible.

## Règles simples
- Page = assemblage de `Panel` pour les sections visibles ; `Box` pour organiser l’espace (grille, flex, spacing).
- Header/page title : utiliser `PageTitle` si besoin d’un titre + sous-titre + slot droit.
- Éléments UI génériques : `Panel`, `EmptyState`, `SpinnerLoader`, `ConfirmButton`, etc. (dans `src/components/ui/`).
- Layout global : `AppShell` gère header/sidebar/footer/breadcrumbs ; les pages ne touchent pas à ces éléments.

## Exemples
- Démo UI (`UIDemoPage`) : chaque composant est montré dans un `Panel`, empilés dans un `Stack`.
- Temps réel : blocs SSE/WS et note d’info enveloppés dans des `Panel`, empilés via `Stack`, grille interne en `Box`.
- Page simple (dashboard) : `Box` + `Heading`/`Text` suffisent si aucun encadré n’est requis.

## Quand créer un nouveau composant UI ?
- Quand un bloc visuel est réutilisé par plusieurs pages.
- Quand le style d’encadré se répète : factoriser dans `Panel` ou un dérivé (ex : `Card`, `AlertPanel`).
