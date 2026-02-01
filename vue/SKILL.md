---
name: vue
description: Build Vue 3 components, composables, and utilities using Composition API patterns. Provides props/emits best practices, reactive destructuring guidance, VueUse integration, TypeScript patterns, and testing practices. Use when editing .vue files, creating components, writing composables, or testing Vue code.
license: MIT
compatibility: Vue 3.5+
metadata:
  category: frontend
  time: 3h
  source: vue-skill
  triggers:
    - "create Vue component"
    - "write Vue composable"
    - "test Vue component"
    - "refactor Vue code"
    - "improve Vue code quality"
---

# Vue 3 Development

Build Vue 3 components, composables, and utilities using Composition API patterns. This skill provides a reusable implementation guide so AI agents generate production-quality Vue code.

**Current stable:** Vue 3.5+ with enhanced reactivity performance (-56% memory, 10x faster array tracking), new SSR features, and improved developer experience.

## Quick Start

When the user asks for Vue code, follow this workflow:

1. **Identify context** – Component, composable, utility, or test?
2. **Load relevant reference** – Only load the file matching current work
3. **Follow patterns** – Use Composition API patterns, reactive destructuring, type safety
4. **Test if needed** – Write tests using Vitest + @vue/test-utils
5. **Iterate** – Apply patterns consistently across codebase

## When to Use This Skill

**Use this skill when:**

- **Writing `.vue` components** – Creating Vue single-file components
- **Creating composables** – Building reusable `use*` functions
- **Building client-side utilities** – Creating pure functions, formatters, validators
- **Testing Vue code** – Writing tests for components and composables
- **TypeScript patterns** – Using InjectionKey, generic components, strict templates

**Use `nuxt` skill instead for:**

- Server routes, API endpoints
- File-based routing, middleware
- Nuxt-specific patterns

**Related skills:**
- **Styled UI components:** use `nuxt-ui` skill
- **Headless accessible components:** use `reka-ui` skill
- **VueUse composables:** use `vueuse` skill

## Quick Reference

| Working on...            | Load file                  |
| ------------------------ | -------------------------- |
| `.vue` in `components/`  | references/components.md   |
| File in `composables/`   | references/composables.md  |
| File in `utils/`         | references/utils-client.md |
| `.spec.ts` or `.test.ts` | references/testing.md      |
| TypeScript patterns      | references/typescript.md   |
| Vue Router typing        | references/router.md       |

## Loading Files

**Load one file at a time based on file context:**

- Component work → [references/components.md](references/components.md)
- Composable work → [references/composables.md](references/composables.md)
- Utils work → [references/utils-client.md](references/utils-client.md)
- Testing → [references/testing.md](references/testing.md)
- TypeScript → [references/typescript.md](references/typescript.md)
- Vue Router → [references/router.md](references/router.md)

**DO NOT load all files at once** - wastes context on irrelevant patterns.

## Available Guidance

**[references/components.md](references/components.md)** - Props with reactive destructuring, emits patterns, defineModel for v-model, slots shorthand

**[references/composables.md](references/composables.md)** - Composition API structure, VueUse integration, lifecycle hooks, async patterns, reactivity gotchas

**[references/utils-client.md](references/utils-client.md)** - Pure functions, formatters, validators, transformers, when NOT to use utils

**[references/testing.md](references/testing.md)** - Vitest + @vue/test-utils, component testing, composable testing, router mocking

**[references/typescript.md](references/typescript.md)** - InjectionKey for provide/inject, vue-tsc strict templates, tsconfig settings, generic components

**[references/router.md](references/router.md)** - Route meta types, typed params with unplugin-vue-router, scroll behavior, navigation guards

## Core Concepts

### 1. Progressive Loading

Load only relevant reference files based on current work:

- Component work → `references/components.md`
- Composable work → `references/composables.md`
- Utils work → `references/utils-client.md`
- Testing → `references/testing.md`
- TypeScript → `references/typescript.md`
- Vue Router → `references/router.md`

**DO NOT load all files at once** - wastes context on irrelevant patterns.

### 2. Reactive Destructuring

Use `toRefs()` or `storeToRefs()` when destructuring reactive objects:

```ts
// ❌ Avoid - loses reactivity
const { count } = state

// ✅ Prefer - maintains reactivity
const { count } = toRefs(state)
```

### 3. Props with Reactive Destructuring

Destructure props safely:

```ts
const props = defineProps<{ count: number }>()
const { count } = toRefs(props)
```

### 4. defineModel for v-model

Use `defineModel()` for two-way binding:

```ts
const model = defineModel<string>({ default: '' })
```

### 5. Type Safety

- Use `InjectionKey<T>` for provide/inject
- Enable `vue-tsc` strict template checking
- Use generic components for type inference

## Project Structure

```
project/
├── components/          # Vue components
├── composables/        # Reusable composables
├── utils/              # Client-side utilities
└── __tests__/          # Test files
```

## Implementation Guide

### Component Example

```vue
<script setup lang="ts">
import { toRefs } from 'vue'

interface Props {
  title: string
  count?: number
}

const props = withDefaults(defineProps<Props>(), {
  count: 0
})

const emit = defineEmits<{
  update: [value: number]
}>()

const { title, count } = toRefs(props)

function increment() {
  emit('update', count.value + 1)
}
</script>

<template>
  <div>
    <h2>{{ title }}</h2>
    <button @click="increment">Count: {{ count }}</button>
  </div>
</template>
```

### Composable Example

```ts
// composables/useCounter.ts
export function useCounter(initial = 0) {
  const count = ref(initial)

  const increment = () => count.value++
  const decrement = () => count.value--
  const reset = () => count.value = initial

  return {
    count: readonly(count),
    increment,
    decrement,
    reset
  }
}
```

## References

| File | Purpose |
|------|---------|
| `references/components.md` | Props with reactive destructuring, emits patterns, defineModel for v-model, slots shorthand |
| `references/composables.md` | Composition API structure, VueUse integration, lifecycle hooks, async patterns, reactivity gotchas |
| `references/utils-client.md` | Pure functions, formatters, validators, transformers, when NOT to use utils |
| `references/testing.md` | Vitest + @vue/test-utils, component testing, composable testing, router mocking |
| `references/typescript.md` | InjectionKey for provide/inject, vue-tsc strict templates, tsconfig settings, generic components |
| `references/router.md` | Route meta types, typed params with unplugin-vue-router, scroll behavior, navigation guards |

## Quick Reference

| Working on...            | Load file                  |
| ------------------------ | -------------------------- |
| `.vue` in `components/`  | `references/components.md`   |
| File in `composables/`   | `references/composables.md`  |
| File in `utils/`         | `references/utils-client.md` |
| `.spec.ts` or `.test.ts` | `references/testing.md`      |
| TypeScript patterns      | `references/typescript.md`   |
| Vue Router typing        | `references/router.md`       |

## Token Efficiency

Main skill: ~250 tokens base. Each sub-file: ~500-1500 tokens. Only load files relevant to current task.

---

*This skill aligns with the [Drift Skills](https://github.com/dadbodgeoff/drift/wiki/Skills) format: reusable implementation guides so AI agents produce production-grade Vue component code.*
