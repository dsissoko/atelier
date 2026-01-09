# BRIEF DESIGN — MVP FRONTEND (Template)

<!--
Ce document sert de brief produit pour cadrer un MVP frontend.
Chaque section doit être remplie avec le contexte du projet.
Conservez les titres, remplacez les commentaires par votre contenu.
-->

## 1. Vision du service

<!--
Décrivez le service en 5-10 lignes :
- Qui sont les utilisateurs cibles ?
- Quel problème principal résolvez-vous ?
- En quoi l'approche est-elle differente d'alternatives existantes ?
- Quelle experience emotionnelle ou mentale voulez-vous favoriser ?
Conseil : evitez le jargon technique ici.
-->

---

## 2. Role strategique du frontend

<!--
Expliquez la contribution unique du frontend :
- Quel est son role pour l'utilisateur (boussole, cockpit, assistant, etc.) ?
- Quels types d'informations il doit rendre immediates ?
- Quels usages doivent etre simples et evidents ?
- Quelles limites de charge cognitive devez-vous respecter ?
Conseil : insistez sur la priorisation de l'information.
-->

---

## 3. Portee fonctionnelle du MVP

<!--
Listez clairement ce qui est INCLUS et EXCLU dans le MVP.
But : eviter la sur-ambition et fixer les frontieres.

Inclure : ecrans, modules, parcours essentiels, supports techniques (ex: mock API).
Exclure : analyses avancees, workflows complexes, integr. profondes, etc.
-->

### Inclus
<!--
- [Ecran ou module] : description en 1 phrase
- [Ecran ou module]
-->

### Exclus
<!--
- [Fonction non couverte]
- [Fonction non couverte]
-->

---

# 4. Description des ecrans (UX enrichie et coherente avec le modele metier)

<!--
Pour chaque ecran : role, contenu cle, actions, limites.
Objectif : aligner UX + metier + priorites MVP.
-->

## 4.1 [Nom de l'ecran]
<!--
Role : decrire l'objectif principal de l'ecran.
Visuel propose : decrire la structure (cartes, tableaux, graphiques, etc.).
Actions utilisateur : lister les actions possibles.
-->

---

## 4.2 [Nom de l'ecran]
<!-- Meme structure que 4.1 -->

---

## 4.X [Nom de l'ecran majeur]
<!--
Detaillez si un ecran est central au produit :
- sous-sections (listes, detail, creation)
- etats (actif, suspendu, termine, etc.)
- regles metier visibles
-->

---

## 5. Backend (MVP)

<!--
Decrivez le role du backend dans le MVP :
- Que couvre-t-il ?
- Que ne couvre-t-il pas ?
- Pourquoi cette limite est-elle volontaire ?
-->

### 5.1 Stack et principes
<!--
Listez la stack technique et les principes d'implementation.
Exemple : framework API, ORM, auth, logging, docs.
Indiquez la priorite : simplicite, robustesse, compatibilite, etc.
-->

### 5.2 Services exposes
<!--
Listez les endpoints/ressources principales.
Pour chaque service : objectifs, operations supportees, regles de securite.
-->

### 5.3 Securite et integration frontend
<!--
- Mode d'authentification
- Regles d'acces
- Mode mocke vs backend reel
-->

### 5.4 Evolutivite
<!--
Expliquez ce qui pourra etre etendu plus tard.
-->

---

# 6. Elements techniques

## 6.1 Architecture frontend
<!--
Precisez l'architecture (monorepo, separation front/back, structure de dossiers).
-->

## 6.2 Objectifs techniques
<!--
Lister les objectifs techniques prioritaires (ex: mock API, perf, theming, deploy).
-->

## 6.3 API mockees
<!--
Lister les routes mockees et les entites cibles.
Indiquer le format attendu (compatibilite backend si necessaire).
-->

## 6.4 Stack du frontend
<!--
Coller un extrait JSON ou une liste des dependances cles.
-->

## 6.5 Stack du backend
<!--
Coller un extrait JSON ou une liste des dependances cles.
-->

---

# 7. Roadmap semver

<!--
Definir une progression de versions pour le MVP :
- chaque version = un ensemble coherent de features
- eviter les versions trop grosses
Exemple : 0.1.0, 0.2.0, etc.
-->

## 0.1.0
<!--
- [Feature 1]
- [Feature 2]
-->

## 0.2.0
<!--
- [Feature 1]
-->

<!-- Ajouter autant de versions que necessaire -->
