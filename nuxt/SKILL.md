---
name: nuxt
description: Build production-grade Nuxt 4+ applications using server routes, file-based routing, middleware patterns, Nuxt-specific composables, and configuration. Covers h3 v1 helpers (validation, WebSocket, SSE) and nitropack v2 patterns. Use when building, scaffolding, or reviewing Nuxt applications.
license: MIT
compatibility: Nuxt 4+, Node.js 18+
metadata:
  category: frontend
  time: 4h
  source: nuxt-skill
  triggers:
    - "build a Nuxt app"
    - "create Nuxt server routes"
    - "set up Nuxt routing"
    - "add Nuxt middleware"
    - "review Nuxt code"
    - "configure Nuxt project"
---

# Nuxt 4+ Development

Build Nuxt 4+ applications with server routes, file-based routing, middleware, and composables. This skill provides a reusable implementation guide so AI agents generate production-quality Nuxt code.

## Quick Start

When the user asks for a Nuxt application, follow this workflow:

1. **Understand requirements** – Server routes, pages, middleware, plugins needed
2. **Initialize project structure** – `nuxt.config.ts`, directories (`server/`, `pages/`, `components/`)
3. **Set up routing** – File-based routing with layouts and route groups
4. **Add server endpoints** – API routes with h3 validation and helpers
5. **Implement middleware/plugins** – Route guards and app extensions
6. **Iterate on features** – Reuse patterns for remaining functionality

## When to Use This Skill

- **New Nuxt projects** – Scaffolding a Nuxt 4+ application from scratch
- **Server routes** – Creating API endpoints, server middleware, server utilities
- **File-based routing** – Setting up pages, layouts, route groups with typed router
- **Middleware & plugins** – Implementing route guards, navigation, app lifecycle hooks
- **Nuxt composables** – Using `useFetch`, `useRequestURL`, navigation helpers
- **Configuration** – Setting up `nuxt.config.ts`, modules, auto-imports, layers
- **Code review / refactor** – Reviewing or refactoring Nuxt code for best practices

**Related skills:**
- **Vue composables:** See `vue` skill (VueUse, Composition API patterns)
- **UI components:** use `nuxt-ui` skill
- **Database/storage:** use `nuxthub` skill
- **Content-driven sites:** use `nuxt-content` skill
- **Creating modules:** use `nuxt-modules` skill

## Core Concepts

### 1. Progressive Loading

Load only relevant reference files based on current work:

- Working in `server/` → read `references/server.md`
- Working in `pages/` or `layouts/` → read `references/routing.md`
- Using composables/data fetching → read `references/nuxt-composables.md`
- Using `<a>`, `<img>`, `<time>` elements → read `references/nuxt-components.md`
- Working in `middleware/` or `plugins/` → read `references/middleware-plugins.md`
- Editing `nuxt.config.ts` → read `references/nuxt-config.md`

**DO NOT read all files at once.** Load based on context to minimize token usage.

### 2. Nuxt 4 vs Older Versions

**You are working with Nuxt 4+.** Key differences:

| Old (Nuxt 2/3)    | New (Nuxt 4)                    |
| ----------------- | ------------------------------- |
| `<Nuxt />`        | `<NuxtPage />`                  |
| `context.params`  | `getRouterParam(event, 'name')` |
| `window.origin`   | `useRequestURL().origin`        |
| String routes     | Typed router with route names   |
| Separate layouts/ | Parent routes with `<slot>`     |

### 3. Server-First Architecture

- **Server routes:** `server/api/` for API endpoints, `server/middleware/` for server middleware
- **h3 helpers:** Validation (Zod), WebSocket, SSE, typed event handlers
- **Nitro patterns:** Build-time optimizations, multi-cloud deployment

### 4. File-Based Routing

- **Pages:** `pages/` directory maps to routes automatically
- **Layouts:** `layouts/` for shared page structure
- **Route groups:** `(group)/` for organization without affecting URLs
- **Typed router:** Type-safe navigation with route names

### 5. Auto-Imports & Modules

- **Auto-imports:** Components, composables, utils automatically available
- **Modules:** Extend functionality via `modules` array in `nuxt.config.ts`
- **Layers:** Share configuration and code across projects

## Project Structure

```
project/
├── server/
│   ├── api/              # API endpoints
│   ├── middleware/       # Server middleware
│   └── utils/            # Server utilities
├── pages/                # File-based routes
│   ├── (group)/         # Route groups
│   └── layouts/         # Page layouts
├── components/           # Auto-imported components
├── composables/          # Auto-imported composables
├── middleware/           # Route middleware
├── plugins/              # App plugins
└── nuxt.config.ts       # Nuxt configuration
```

## Implementation Guide

### Server Routes

Create API endpoints in `server/api/`:

```ts
// server/api/users/[id].ts
export default defineEventHandler(async (event) => {
  const id = getRouterParam(event, 'id')
  const query = getQuery(event)

  // Validation with Zod
  const schema = z.object({ id: z.string().uuid() })
  const { id: userId } = await getValidatedRouterParams(event, schema)

  return { user: await getUser(userId) }
})
```

### File-Based Routing

Pages automatically map to routes:

```vue
<!-- pages/users/[id].vue -->
<script setup>
const route = useRoute()
const userId = route.params.id
</script>

<template>
  <div>User {{ userId }}</div>
</template>
```

### Middleware

Route guards in `middleware/`:

```ts
// middleware/auth.ts
export default defineNuxtRouteMiddleware((to, from) => {
  const { loggedIn } = useUserSession()
  if (!loggedIn.value) {
    return navigateTo('/login')
  }
})
```

### Composables

Nuxt-specific composables:

```ts
// useFetch for data fetching
const { data, error, pending } = await useFetch('/api/users')

// useRequestURL for request info
const { origin, pathname } = useRequestURL()

// Navigation
await navigateTo('/dashboard')
```

## References

| File | Purpose |
|------|---------|
| `references/server.md` | API routes, server middleware, validation (Zod), WebSocket, SSE |
| `references/routing.md` | File-based routing, route groups, typed router, definePage |
| `references/middleware-plugins.md` | Route middleware, plugins, app lifecycle |
| `references/nuxt-composables.md` | Nuxt composables (useRequestURL, useFetch, navigation) |
| `references/nuxt-components.md` | NuxtLink, NuxtImg, NuxtTime (prefer over HTML elements) |
| `references/nuxt-config.md` | Configuration, modules, auto-imports, layers |
| `references/project-setup.md` | CI/ESLint setup, project scaffolding |

## Official Documentation

**When to fetch latest docs:**

- New Nuxt 4 features not covered here
- Module-specific configuration
- Breaking changes or deprecations
- Advanced use cases

**Official sources:**

- Nuxt: https://nuxt.com/docs
- h3 (server engine): https://v1.h3.dev/
- Nitro: https://nitro.build/

## Token Efficiency

Main skill: ~300 tokens. Each sub-file: ~800-1500 tokens. Only load files relevant to current task.

---

*This skill aligns with the [Drift Skills](https://github.com/dadbodgeoff/drift/wiki/Skills) format: reusable implementation guides so AI agents produce production-grade Nuxt application code.*
