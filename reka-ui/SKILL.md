---
name: reka-ui
description: Build accessible Vue 3 components using Reka UI (headless component primitives). Provides component API, accessibility patterns, composition (asChild), controlled/uncontrolled state, virtualization, and styling integration. WAI-ARIA compliant. Formerly Radix Vue. Use when building headless components, implementing accessible UI, or using Reka-based libraries.
license: MIT
compatibility: Vue 3.5+, Reka UI v2.7.0+
metadata:
  category: frontend
  time: 3h
  source: reka-ui-skill
  triggers:
    - "build accessible components"
    - "use Reka UI"
    - "create headless components"
    - "implement WAI-ARIA components"
    - "add dialog/menu/popover"
---

# Reka UI

Build accessible Vue 3 components using Reka UI headless component primitives. WAI-ARIA compliant. Previously Radix Vue. This skill provides a reusable implementation guide so AI agents generate production-quality accessible Vue components.

**Current version:** v2.7.0 (December 2025)

## Quick Start

When the user asks for accessible components, follow this workflow:

1. **Install Reka UI** – Add `reka-ui/nuxt` module or use resolver
2. **Choose component** – Select from Form, Date, Overlay, Menu, Data categories
3. **Import parts** – Import Root, Trigger, Content, Portal components
4. **Configure state** – Use controlled (`v-model`) or uncontrolled (`default*`) state
5. **Style components** – Add custom styling to unstyled primitives
6. **Test accessibility** – Verify WAI-ARIA compliance

## When to Use This Skill

- **Building headless components** – Creating unstyled components from scratch
- **WAI-ARIA compliance** – Need accessible, compliant components
- **Using Reka-based libraries** – Working with Nuxt UI, shadcn-vue, or other Reka-based libraries
- **Accessible forms** – Implementing accessible form controls
- **Dialogs & overlays** – Creating dialogs, menus, popovers, tooltips
- **Virtualization** – Optimizing large lists with virtual scrolling

**Related skills:**
- **Vue patterns:** use `vue` skill

## Available Guidance

| File                                                     | Topics                                                              |
| -------------------------------------------------------- | ------------------------------------------------------------------- |
| **[references/components.md](references/components.md)** | Component index by category (Form, Date, Overlay, Menu, Data, etc.) |
| **components/\*.md**                                     | Per-component details (dialog.md, select.md, etc.)                  |

**New guides** (see [reka-ui.com](https://reka-ui.com)): Controlled State, Inject Context, Virtualization, Migration

## Usage Pattern

**Load based on context:**

- Component index → [references/components.md](references/components.md)
- Specific component → [components/dialog.md](components/dialog.md), [components/select.md](components/select.md), etc.
- For styled Nuxt components built on Reka UI → use **nuxt-ui** skill

## Key Concepts

| Concept                 | Description                                                           |
| ----------------------- | --------------------------------------------------------------------- |
| `asChild`               | Render as child element instead of wrapper, merging props/behavior    |
| Controlled/Uncontrolled | Use `v-model` for controlled, `default*` props for uncontrolled       |
| Parts                   | Components split into Root, Trigger, Content, Portal, etc.            |
| `forceMount`            | Keep element in DOM for animation libraries                           |
| Virtualization          | Optimize large lists (Combobox, Listbox, Tree) with virtual scrolling |
| Context Injection       | Access component context from child components                        |

## Installation

```ts
// nuxt.config.ts (auto-imports all components)
export default defineNuxtConfig({
  modules: ['reka-ui/nuxt']
})
```

```ts
import { RekaResolver } from 'reka-ui/resolver'
// vite.config.ts (with auto-import resolver)
import Components from 'unplugin-vue-components/vite'

export default defineConfig({
  plugins: [
    vue(),
    Components({ resolvers: [RekaResolver()] })
  ]
})
```

## Basic Patterns

```vue
<!-- Dialog with controlled state -->
<script setup>
import { DialogRoot, DialogTrigger, DialogPortal, DialogOverlay, DialogContent, DialogTitle, DialogDescription, DialogClose } from 'reka-ui'
const open = ref(false)
</script>

<template>
  <DialogRoot v-model:open="open">
    <DialogTrigger>Open</DialogTrigger>
    <DialogPortal>
      <DialogOverlay class="fixed inset-0 bg-black/50" />
      <DialogContent class="fixed left-1/2 top-1/2 -translate-x-1/2 -translate-y-1/2 bg-white p-6 rounded">
        <DialogTitle>Title</DialogTitle>
        <DialogDescription>Description</DialogDescription>
        <DialogClose>Close</DialogClose>
      </DialogContent>
    </DialogPortal>
  </DialogRoot>
</template>
```

```vue
<!-- Select with uncontrolled default -->
<SelectRoot default-value="apple">
  <SelectTrigger>
    <SelectValue placeholder="Pick fruit" />
  </SelectTrigger>
  <SelectPortal>
    <SelectContent>
      <SelectViewport>
        <SelectItem value="apple"><SelectItemText>Apple</SelectItemText></SelectItem>
        <SelectItem value="banana"><SelectItemText>Banana</SelectItemText></SelectItem>
      </SelectViewport>
    </SelectContent>
  </SelectPortal>
</SelectRoot>
```

```vue
<!-- asChild for custom trigger element -->
<DialogTrigger as-child>
  <button class="my-custom-button">Open</button>
</DialogTrigger>
```

## Recent Updates (v2.5.0-v2.7.0)

- **New composables exposed**: `useLocale`, `useDirection` (v2.6.0)
- **Select**: Added `disableOutsidePointerEvents` prop to Content
- **Toast**: Added `disableSwipe` prop for swipe control
- **DatePicker**: Added `closeOnSelect` property
- **ContextMenu**: Added `pressOpenDelay` for long-press configuration
- **Virtualization**: `estimateSize` now accepts function for Listbox/Tree (v2.7.0); supported in Combobox, Listbox, Tree

## Core Concepts

### 1. Component Parts

Components split into Root, Trigger, Content, Portal, etc.:

```vue
<DialogRoot>
  <DialogTrigger>Open</DialogTrigger>
  <DialogPortal>
    <DialogOverlay />
    <DialogContent>
      <DialogTitle>Title</DialogTitle>
      <DialogDescription>Description</DialogDescription>
    </DialogContent>
  </DialogPortal>
</DialogRoot>
```

### 2. Controlled vs Uncontrolled

- **Controlled:** Use `v-model` for state management
- **Uncontrolled:** Use `default*` props for initial state

### 3. asChild Pattern

Render as child element instead of wrapper, merging props/behavior:

```vue
<DialogTrigger as-child>
  <button class="my-custom-button">Open</button>
</DialogTrigger>
```

### 4. Virtualization

Optimize large lists (Combobox, Listbox, Tree) with virtual scrolling:

```vue
<ListboxRoot virtual>
  <ListboxViewport>
    <ListboxItem v-for="item in items" :key="item.id">
      {{ item.label }}
    </ListboxItem>
  </ListboxViewport>
</ListboxRoot>
```

## Project Structure

```
project/
├── components/
│   └── ui/              # Styled Reka components
│       ├── Dialog.vue
│       ├── Select.vue
│       └── Menu.vue
└── nuxt.config.ts       # Reka UI module config
```

## Implementation Guide

### Installation

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  modules: ['reka-ui/nuxt']
})
```

### Basic Dialog

```vue
<script setup>
import { DialogRoot, DialogTrigger, DialogPortal, DialogOverlay, DialogContent, DialogTitle, DialogDescription, DialogClose } from 'reka-ui'
const open = ref(false)
</script>

<template>
  <DialogRoot v-model:open="open">
    <DialogTrigger>Open</DialogTrigger>
    <DialogPortal>
      <DialogOverlay class="fixed inset-0 bg-black/50" />
      <DialogContent class="fixed left-1/2 top-1/2 -translate-x-1/2 -translate-y-1/2 bg-white p-6 rounded">
        <DialogTitle>Title</DialogTitle>
        <DialogDescription>Description</DialogDescription>
        <DialogClose>Close</DialogClose>
      </DialogContent>
    </DialogPortal>
  </DialogRoot>
</template>
```

### Select with Uncontrolled State

```vue
<SelectRoot default-value="apple">
  <SelectTrigger>
    <SelectValue placeholder="Pick fruit" />
  </SelectTrigger>
  <SelectPortal>
    <SelectContent>
      <SelectViewport>
        <SelectItem value="apple">
          <SelectItemText>Apple</SelectItemText>
        </SelectItem>
        <SelectItem value="banana">
          <SelectItemText>Banana</SelectItemText>
        </SelectItem>
      </SelectViewport>
    </SelectContent>
  </SelectPortal>
</SelectRoot>
```

## References

| File | Purpose |
|------|---------|
| `references/components.md` | Component index by category (Form, Date, Overlay, Menu, Data, etc.) |
| `components/*.md` | Per-component details (dialog.md, select.md, etc.) |

## Usage Pattern

**Load based on context:**

- Component index → `references/components.md`
- Specific component → `components/dialog.md`, `components/select.md`, etc.
- For styled Nuxt components built on Reka UI → use **nuxt-ui** skill

## Resources

- [Reka UI Docs](https://reka-ui.com)
- [GitHub](https://github.com/unovue/reka-ui)
- [Nuxt UI](https://ui.nuxt.com) (styled Reka components)
- [shadcn-vue](https://www.shadcn-vue.com) (styled Reka components)

## Token Efficiency

Main skill: ~350 tokens base. `components.md` index: ~100 tokens. Per-component: ~50-150 tokens.

---

*This skill aligns with the [Drift Skills](https://github.com/dadbodgeoff/drift/wiki/Skills) format: reusable implementation guides so AI agents produce production-grade accessible Vue component code.*
