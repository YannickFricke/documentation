---
title: Plugin - Elysia.js
head:
  - - meta
    - property: 'og:title'
      content: Plugin - Elysia.js

  - - meta
    - name: 'description'
      content: A plugin is a way to decouple logic into smaller parts, defining reusable components across the server. Plugin can register by using `use`, registering a plugin will combine types between plugin and current instance, and the scope of hooks, and schema get merged too.

  - - meta
    - property: 'og:description'
      content: A plugin is a way to decouple logic into smaller parts, defining reusable components across the server. Plugin can register by using `use`, registering a plugin will combine types between plugin and current instance, and the scope of hooks, and schema get merged too.
---

# Plugin
A plugin is a way to decouple logic into smaller parts, defining reusable components across the server.

Defining a plugin is as simple as defining a new Elysia instance:
```typescript
import { Elysia } from 'elysia'

const plugin = new Elysia()
    .state('plugin-version', 1)
    .get('/from-plugin', () => 'Hi')

const app = new Elysia()
    .use(plugin)
    .get('/version', ({ store }) => store['plugin-version'])
    .listen(8080)
```

Plugin can register by using `use`.

Registering a plugin will combine types between plugin and current instance, and the scope of hooks, and schema get merged too.

You can also use a function callback to create a new plugin:
```ts
const plugin = (app: Elysia) => app
    .get('/from-plugin', () => 'Hi')

const app = new Elysia()
    .use(plugin)
    .listen(8080)
```

Functional callback will allows use to access main instance values like routes schema, store.

## Config
You can customize plugin by creating function to return callback which accepts Elysia.

```typescript
import { Elysia } from 'elysia'

const plugin = ({
    prefix = '/v1'
}) => (app: Elysia) => app
    .get(`/${prefix}/hi`, () => 'Hi')

const app = new Elysia()
    .use(plugin({
        prefix: '/v2'
    }))
    .listen(8080)
```

Config type will be inferred into `use`, generating auto completion and type strict as intend.

## Plugin deduplication
By default, Elysia will registered any plugin and handle type definitions which when using multiple times will results in a multiple duplication of setting value or routes.

This can be fixed by providing name and optional seeds to help Elysia identify instance duplication:
```ts
import { Elysia } from 'elysia'

const plugin = (config) => new Elysia({
    name: 'my-plugin',
    seed: config
})
    .get(`/${prefix}/hi`, () => 'Hi')

const app = new Elysia()
    .use(plugin({
        prefix: '/v2'
    }))
    .listen(8080)
```

## Official plugins
You can find pre-built plugins for Elysia at [plugins](/plugins/overview).
