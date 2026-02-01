---
name: nuxt-modules
description: Create Nuxt modules for published npm packages, local project modules, or runtime extensions. Provides defineNuxtModule patterns, Kit utilities, hooks, E2E testing, and release automation. Use when building, testing, or publishing Nuxt modules.
license: MIT
compatibility: Nuxt 4+, Node.js 18+
metadata:
  category: infrastructure
  time: 5h
  source: nuxt-modules-skill
  triggers:
    - "create Nuxt module"
    - "build Nuxt module"
    - "publish Nuxt module"
    - "test Nuxt module"
    - "set up module CI/CD"
    - "extend Nuxt functionality"
---

# Nuxt Module Development

Create Nuxt modules that extend framework functionality. This skill provides a reusable implementation guide so AI agents generate production-quality Nuxt modules.

**Related skills:** `nuxt` (basics), `vue` (runtime patterns)

## Quick Start

When the user asks to create a Nuxt module, follow this workflow:

1. **Initialize module** – Use `nuxi init -t module` to scaffold module structure
2. **Define module** – Create `src/module.ts` with `defineNuxtModule`
3. **Add runtime code** – Place components, composables, plugins in `src/runtime/`
4. **Set up playground** – Configure playground for development testing
5. **Write tests** – Create E2E tests in `test/fixtures/`
6. **Publish module** – Set up CI/CD and publish to npm

## When to Use This Skill

- **Published npm modules** – Creating `@nuxtjs/` or `nuxt-` prefixed packages
- **Local project modules** – Building project-specific extensions in `modules/` directory
- **Runtime extensions** – Adding components, composables, plugins
- **Server extensions** – Creating API routes, middleware, server utilities
- **Releasing/publishing** – Publishing modules to npm with proper versioning
- **CI/CD workflows** – Setting up automated testing and publishing workflows

## Core Concepts

### 1. Module Types

| Type      | Location         | Use Case                         |
| --------- | ---------------- | -------------------------------- |
| Published | npm package      | `@nuxtjs/`, `nuxt-` distribution |
| Local     | `modules/` dir   | Project-specific extensions      |
| Inline    | `nuxt.config.ts` | Simple one-off hooks             |

### 2. Module Anatomy

- **Entry point:** `src/module.ts` with `defineNuxtModule`
- **Runtime code:** `src/runtime/` injected into user's app
- **Playground:** Development testing environment
- **Tests:** E2E tests in `test/fixtures/`

### 3. Kit Utilities

Nuxt Kit provides utilities for module development:
- `addComponentsDir()` – Auto-import components
- `addImports()` – Auto-import composables/utils
- `addPlugin()` – Register plugins
- `addServerHandler()` – Add API routes
- `addHooks()` – Register lifecycle hooks

## Project Structure

```
my-module/
├── src/
│   ├── module.ts           # Entry point
│   └── runtime/            # Injected into user's app
│       ├── components/
│       ├── composables/
│       ├── plugins/
│       └── server/
├── playground/             # Dev testing
│   └── nuxt.config.ts
├── test/
│   └── fixtures/           # E2E tests
└── package.json
```

## Implementation Guide

### Initialize Module

```bash
npx nuxi init -t module my-module
cd my-module && npm install
```

### Define Module

```ts
// src/module.ts
import { defineNuxtModule, addComponentsDir, addImports } from '@nuxt/kit'

export default defineNuxtModule({
  meta: {
    name: 'my-module',
    configKey: 'myModule'
  },
  defaults: {
    enabled: true
  },
  setup(options, nuxt) {
    // Add components
    addComponentsDir({
      path: resolve('./runtime/components'),
      pathPrefix: false
    })

    // Add composables
    addImports({
      name: 'useMyComposable',
      from: resolve('./runtime/composables/useMyComposable')
    })
  }
})
```

### Runtime Code

```ts
// src/runtime/composables/useMyComposable.ts
export const useMyComposable = () => {
  return { message: 'Hello from module!' }
}
```

### Development

```bash
npm run dev        # Start playground
npm run dev:build  # Build in watch mode
npm run test       # Run tests
```

## References

| File | Purpose |
|------|---------|
| `references/development.md` | Module anatomy, defineNuxtModule, Kit utilities, hooks |
| `references/testing-and-publishing.md` | E2E testing, best practices, releasing, publishing |
| `references/ci-workflows.md` | Copy-paste CI/CD workflow templates |

## Usage Pattern

**Load based on context:**

- Building module features? → `references/development.md`
- Testing or publishing? → `references/testing-and-publishing.md`
- CI workflow templates? → `references/ci-workflows.md`

## Resources

- [Module Guide](https://nuxt.com/docs/guide/going-further/modules)
- [Nuxt Kit](https://nuxt.com/docs/api/kit)
- [Module Starter](https://github.com/nuxt/starter/tree/module)

---

*This skill aligns with the [Drift Skills](https://github.com/dadbodgeoff/drift/wiki/Skills) format: reusable implementation guides so AI agents produce production-grade Nuxt module code.*
