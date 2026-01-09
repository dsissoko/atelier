# Frontend-first CRUD Strategy — Feathers, Sequelize, MSW

## Objectif du document

L’objectif de ce document est de présenter **un pattern efficace et pragmatique de mapping ORM** dans une application fullstack.

Pour aller **droit au but** et éviter toute sur‑complexité inutile :
- le pattern **DTO-less API** est volontairement choisi ;
- le **modèle métier frontend définit directement la shape de l’API** ;
- le backend (Feathers + Sequelize) s’aligne sur ce contrat.

Ce choix est **pédagogique et stratégique** :  
il permet de comprendre rapidement les flux, les responsabilités et les points de friction réels.
« DTO-less API » signifie qu’il n’existe pas de couche DTO dédiée : le modèle métier frontend fait directement office de contrat API. Dit autrement: dans le pattern DTO-less, le mapping ORM est volontairement réduit à une projection 1:1 tant que la shape API et la persistence restent alignées.

➡️ **Important** :  
ce pattern est **totalement compatible** avec une architecture utilisant des DTO dédiés.  
La conclusion présente les ajouts nécessaires pour basculer vers un **pattern DTO** sans remettre en cause la stratégie globale. Ce pattern est également tout à fait suffisant pour un projet complexe.

## Stack

- **Frontend** : application web TypeScript
- **Mock API** : MSW (Mock Service Worker)
- **Backend** : FeathersJS
- **Persistence** : Sequelize + base SQL (PostgreSQL, MySQL, etc.)

---

## Problématique

L'objectif est de construire un **modèle métier CRUD** qui soit :

- simple à comprendre
- facile à maintenir
- extensible sans refactor violent
- aligné frontend / backend
- facilement **générable par une IA** une fois le modèle stabilisé

La contrainte clé :  
éviter l’over‑engineering tout en gardant une architecture propre.

La stratégie retenue est **frontend‑first** :
- le modèle métier est conçu côté frontend
- testé avec MSW
- puis reproduit côté backend via Feathers + Sequelize

---

## Exemple métier

Deux entités suffisent pour couvrir les cas réels :

### User
- id
- email
- role
- createdAt
- updatedAt

### Item
- id
- label
- status
- ownerId (User)
- createdAt
- updatedAt

Relation :
- User 1‑N Item

---

## Arborescence — Frontend (pertinent uniquement)

```text
frontend/
  src/
    model/
      user.ts
      item.ts
    mocks/
      handlers/
        users.ts
        items.ts
      browser.ts
      handlers.ts
```
---

## Arborescence — Backend (projection cible)

```text
backend/
  src/
    persistence/
      sequelize/
        models/
          user.model.ts
          item.model.ts
        migrations/
          xxxx-create-users.ts
          xxxx-create-items.ts
    services/
      users/
        users.service.ts
        users.hooks.ts
      items/
        items.service.ts
        items.hooks.ts
```

---

## Code — Frontend

### model/user.ts

```ts
export type UserRole = "user" | "admin";

export interface User {
  id: string;
  email: string;
  role: UserRole;
  createdAt: string;
  updatedAt: string;
}
```

### model/item.ts

```ts
export type ItemStatus = "draft" | "active" | "archived";

export interface Item {
  id: string;
  label: string;
  status: ItemStatus;
  ownerId: string;
  createdAt: string;
  updatedAt: string;
}
```

---

### mocks/handlers/users.ts

```ts
import { http, HttpResponse } from "msw";
import { User } from "../../model/user";

type Page<T> = {
  total: number;
  limit: number;
  skip: number;
  data: T[];
};

let users: User[] = [];

export const usersHandlers = [
  http.get("/users", ({ request }) => {
    const url = new URL(request.url);
    const limit = Number(url.searchParams.get("$limit") ?? 10);
    const skip = Number(url.searchParams.get("$skip") ?? 0);

    return HttpResponse.json({
      total: users.length,
      limit,
      skip,
      data: users.slice(skip, skip + limit)
    } as Page<User>);
  }),

  http.post("/users", async ({ request }) => {
    const body = await request.json() as Partial<User>;
    const now = new Date().toISOString();

    const user: User = {
      id: crypto.randomUUID(),
      email: body.email!,
      role: body.role ?? "user",
      createdAt: now,
      updatedAt: now
    };

    users.unshift(user);
    return HttpResponse.json(user, { status: 201 });
  })
];
```

---

### mocks/handlers/items.ts

```ts
import { http, HttpResponse } from "msw";
import { Item } from "../../model/item";

type Page<T> = {
  total: number;
  limit: number;
  skip: number;
  data: T[];
};

let items: Item[] = [];

export const itemsHandlers = [
  http.get("/items", ({ request }) => {
    const url = new URL(request.url);
    const limit = Number(url.searchParams.get("$limit") ?? 10);
    const skip = Number(url.searchParams.get("$skip") ?? 0);

    return HttpResponse.json({
      total: items.length,
      limit,
      skip,
      data: items.slice(skip, skip + limit)
    } as Page<Item>);
  }),

  http.post("/items", async ({ request }) => {
    const body = await request.json() as Partial<Item>;
    const now = new Date().toISOString();

    const item: Item = {
      id: crypto.randomUUID(),
      label: body.label!,
      status: body.status ?? "draft",
      ownerId: body.ownerId!,
      createdAt: now,
      updatedAt: now
    };

    items.unshift(item);
    return HttpResponse.json(item, { status: 201 });
  })
];
```

---

### mocks/handlers.ts

```ts
import { usersHandlers } from "./handlers/users";
import { itemsHandlers } from "./handlers/items";

export const handlers = [
  ...usersHandlers,
  ...itemsHandlers
];
```

### mocks/browser.ts

```ts
import { setupWorker } from "msw/browser";
import { handlers } from "./handlers";

export const worker = setupWorker(...handlers);
```

---

## Conclusion

Cette approche permet :

- de stabiliser le **modèle métier côté frontend**
- de tester l’intégralité du CRUD sans backend
- d’aligner naturellement MSW et Feathers
- de générer ensuite facilement les modèles Sequelize et services Feathers

Étapes suivantes :
- introduire une **validation métier** structurée avec Zod
- extraire le modèle vers une **lib shared** front/back
- automatiser la génération backend (ORM, services) à partir du modèle stabilisé
- basculer vers un pattern DTO

Le cœur de la valeur reste le **modèle métier**, pas la technologie.


## Annexes

### Évolution vers un pattern DTO

Le pattern **DTO-less API** est idéal pour :
- démarrer vite
- réduire le code inutile
- stabiliser le modèle métier
- faciliter la génération backend (humaine ou IA)

Lorsque la complexité augmente (sécurité, champs internes, divergences de shape),  
il devient pertinent d’introduire un **pattern DTO**.

### Ajouts nécessaires (sans remise en cause)

#### Frontend
- ajout d’un répertoire `dto/` si la shape API diverge du modèle UI

#### Backend
- ajout d’un répertoire `dto/` pour les contrats API
- ajout d’un répertoire `mappers/` pour les conversions ORM ↔ DTO

### Arborescence complétée — Pattern DTO

#### Frontend
```
frontend/
  src/
    model/
    dto/
    mocks/
```

#### Backend
```
backend/
  src/
    dto/
    mappers/
    persistence/
    services/
```