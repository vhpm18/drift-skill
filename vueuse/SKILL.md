---
name: vueuse
description: Use VueUse composables for reactive utilities (state, browser APIs, sensors, network, animations). Provides comprehensive collection of essential Vue Composition utilities. Check VueUse before writing custom composables - most patterns already implemented. Use when building Vue composables, accessing browser APIs, or managing reactive state.
license: MIT
compatibility: Vue 3.5+, VueUse 14.x+
metadata:
  category: frontend
  time: 2h
  source: vueuse-skill
  triggers:
    - "use VueUse composable"
    - "find reactive utility"
    - "access browser API"
    - "manage state"
    - "check before writing composable"
---

# VueUse

Collection of essential Vue Composition utilities. Check VueUse before writing custom composables - most patterns already implemented. This skill provides a reusable reference so AI agents use existing VueUse composables instead of reinventing patterns.

**Current stable:** VueUse 14.x for Vue 3.5+

## Quick Start

When the user asks for a composable or reactive utility, follow this workflow:

1. **Check VueUse first** – Search `references/composables.md` for existing composable
2. **Load composable file** – Read specific composable documentation if found
3. **Use VueUse** – Prefer VueUse over custom implementation
4. **Handle SSR** – Check `isClient` or use SSR-safe composables
5. **Custom only if needed** – Only write custom composable if VueUse doesn't cover it

## When to Use This Skill

- **Finding composables** – Searching for existing VueUse composables
- **State management** – Using `useLocalStorage`, `useSessionStorage`, `useRefHistory`
- **Browser APIs** – Accessing clipboard, fullscreen, media queries
- **Sensors** – Mouse, keyboard, device orientation tracking
- **Network** – Fetch, WebSocket, EventSource utilities
- **Animations** – Transitions, intervals, timeouts
- **Component utilities** – `useVModel`, `useVirtualList`, `useTemplateRefsList`
- **Before writing custom** – Always check VueUse first to avoid reinventing patterns

## Installation

**Vue 3:**

```bash
pnpm add @vueuse/core
```

**Nuxt:**

```bash
pnpm add @vueuse/nuxt @vueuse/core
```

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  modules: ['@vueuse/nuxt'],
})
```

Nuxt module auto-imports composables - no import needed.

## Categories

| Category   | Examples                                                   |
| ---------- | ---------------------------------------------------------- |
| State      | useLocalStorage, useSessionStorage, useRefHistory          |
| Elements   | useElementSize, useIntersectionObserver, useResizeObserver |
| Browser    | useClipboard, useFullscreen, useMediaQuery                 |
| Sensors    | useMouse, useKeyboard, useDeviceOrientation                |
| Network    | useFetch, useWebSocket, useEventSource                     |
| Animation  | useTransition, useInterval, useTimeout                     |
| Component  | useVModel, useVirtualList, useTemplateRefsList             |
| Watch      | watchDebounced, watchThrottled, watchOnce                  |
| Reactivity | createSharedComposable, toRef, toReactive                  |
| Array      | useArrayFilter, useArrayMap, useSorted                     |
| Time       | useDateFormat, useNow, useTimeAgo                          |
| Utilities  | useDebounce, useThrottle, useMemoize                       |

## Quick Reference

Load composable files based on what you need:

| Working on...        | Load file                                              |
| -------------------- | ------------------------------------------------------ |
| Finding a composable | [references/composables.md](references/composables.md) |
| Specific composable  | `composables/<name>.md`                                |

## Loading Files

**Start with [references/composables.md](references/composables.md)** to find the right composable.

Then load the specific composable file for detailed usage: `composables/use-mouse.md`, `composables/use-local-storage.md`, etc.

**DO NOT load all files at once** - wastes context on irrelevant patterns.

## Common Patterns

**State persistence:**

```ts
const state = useLocalStorage('my-key', { count: 0 })
```

**Mouse tracking:**

```ts
const { x, y } = useMouse()
```

**Debounced ref:**

```ts
const search = ref('')
const debouncedSearch = refDebounced(search, 300)
```

**Shared composable (singleton):**

```ts
const useSharedMouse = createSharedComposable(useMouse)
```

## SSR Gotchas

Many VueUse composables use browser APIs unavailable during SSR.

**Check with `isClient`:**

```ts
import { isClient } from '@vueuse/core'

if (isClient) {
  // Browser-only code
  const { width } = useWindowSize()
}
```

**Wrap in onMounted:**

```ts
const width = ref(0)

onMounted(() => {
  // Only runs in browser
  const { width: w } = useWindowSize()
  width.value = w.value
})
```

**Use SSR-safe composables:**

```ts
// These check isClient internally
const mouse = useMouse() // Returns {x: 0, y: 0} on server
const storage = useLocalStorage('key', 'default') // Uses default on server
```

**`@vueuse/nuxt` auto-handles SSR** - composables return safe defaults on server.

## Core Concepts

### 1. Check VueUse First

Always check VueUse before writing custom composables. Most patterns are already implemented:

- State persistence → `useLocalStorage`, `useSessionStorage`
- Mouse tracking → `useMouse`
- Debouncing → `refDebounced`, `useDebounce`
- Window size → `useWindowSize`
- Media queries → `useMediaQuery`

### 2. SSR Safety

Many VueUse composables use browser APIs unavailable during SSR:

```ts
import { isClient } from '@vueuse/core'

if (isClient) {
  const { width } = useWindowSize()
}
```

### 3. Target Element Refs

When targeting component refs instead of DOM elements:

```ts
import { unrefElement } from '@vueuse/core'

const compRef = ref<ComponentInstance>()
const el = computed(() => unrefElement(compRef)) // Gets .$el
const { width } = useElementSize(el) // ✅ Works
```

### 4. Shared Composables

Create singleton composables:

```ts
const useSharedMouse = createSharedComposable(useMouse)
```

## Project Structure

```
project/
├── composables/          # Custom composables (only if VueUse doesn't cover)
└── nuxt.config.ts       # @vueuse/nuxt module config
```

## Implementation Guide

### Installation

```bash
# Vue 3
pnpm add @vueuse/core

# Nuxt
pnpm add @vueuse/nuxt @vueuse/core
```

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  modules: ['@vueuse/nuxt']
})
```

### Common Patterns

**State persistence:**

```ts
const state = useLocalStorage('my-key', { count: 0 })
```

**Mouse tracking:**

```ts
const { x, y } = useMouse()
```

**Debounced ref:**

```ts
const search = ref('')
const debouncedSearch = refDebounced(search, 300)
```

**Shared composable:**

```ts
const useSharedMouse = createSharedComposable(useMouse)
```

## References

| File | Purpose |
|------|---------|
| `references/composables.md` | Composable index by category |
| `composables/*.md` | Per-composable documentation |

## Usage Pattern

**Start with `references/composables.md`** to find the right composable.

Then load the specific composable file for detailed usage: `composables/use-mouse.md`, `composables/use-local-storage.md`, etc.

**DO NOT load all files at once** - wastes context on irrelevant patterns.

## Categories

| Category   | Examples                                                   |
| ---------- | ---------------------------------------------------------- |
| State      | useLocalStorage, useSessionStorage, useRefHistory          |
| Elements   | useElementSize, useIntersectionObserver, useResizeObserver |
| Browser    | useClipboard, useFullscreen, useMediaQuery                 |
| Sensors    | useMouse, useKeyboard, useDeviceOrientation                |
| Network    | useFetch, useWebSocket, useEventSource                     |
| Animation  | useTransition, useInterval, useTimeout                     |
| Component  | useVModel, useVirtualList, useTemplateRefsList             |
| Watch      | watchDebounced, watchThrottled, watchOnce                  |
| Reactivity | createSharedComposable, toRef, toReactive                  |
| Array      | useArrayFilter, useArrayMap, useSorted                     |
| Time       | useDateFormat, useNow, useTimeAgo                          |
| Utilities  | useDebounce, useThrottle, useMemoize                       |

## Resources

- [VueUse Documentation](https://vueuse.org)
- [GitHub](https://github.com/vueuse/vueuse)

---

*This skill aligns with the [Drift Skills](https://github.com/dadbodgeoff/drift/wiki/Skills) format: reusable implementation guides so AI agents produce production-grade Vue composable code using existing VueUse utilities.*
