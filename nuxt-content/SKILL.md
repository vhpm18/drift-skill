---
name: nuxt-content
description: Build content-driven Nuxt 4+ applications using Nuxt Content v3. Provides collections (local/remote/API sources), queryCollection API, MDC rendering, database configuration, NuxtStudio integration, hooks, i18n patterns, and LLMs integration. Use when building content sites, documentation, or blogs.
license: MIT
compatibility: Nuxt 4+, Nuxt Content v3
metadata:
  category: content
  time: 4h
  source: nuxt-content-skill
  triggers:
    - "build content site"
    - "create documentation site"
    - "set up Nuxt Content"
    - "add content collections"
    - "query content"
    - "render markdown"
---

# Nuxt Content v3

Build content-driven Nuxt 4+ applications with typed collections, SQL-backed queries, and MDC rendering. This skill provides a reusable implementation guide so AI agents generate production-quality content-driven applications.

## Quick Start

When the user asks for a content-driven Nuxt app, follow this workflow:

1. **Install Nuxt Content** – Add `@nuxt/content` module
2. **Define collections** – Create `content.config.ts` with collection schemas
3. **Add content files** – Create markdown files in `content/` directory
4. **Query content** – Use `queryCollection()` API for content queries
5. **Render content** – Use `<ContentRenderer>` for MDC rendering
6. **Configure database** – Set up SQLite, PostgreSQL, D1, or LibSQL if needed

## When to Use This Skill

- **Content collections** – Setting up typed content groups with schemas
- **Remote sources** – Integrating GitHub repos or external APIs as content sources
- **Content queries** – Querying content with `queryCollection`, navigation, search
- **MDC rendering** – Rendering markdown with Vue components using `<ContentRenderer>`
- **Database configuration** – Setting up SQLite, PostgreSQL, D1, or LibSQL for content
- **Content hooks** – Using `content:file:beforeParse`, `content:file:afterParse` hooks
- **i18n content** – Multi-language content management
- **NuxtStudio** – Integrating preview mode and live editing
- **LLMs integration** – Using `nuxt-llms` for AI-powered content features

**Related skills:**
- **Nuxt basics:** use `nuxt` skill
- **NuxtHub deployment:** use `nuxthub` skill
- **Writing documentation:** use `document-writer` skill

## Core Concepts

### 1. Collections

Typed content groups with schemas defined in `content.config.ts`:

```ts
export default defineContentConfig({
  collections: {
    blog: defineCollection({
      type: 'page',
      schema: z.object({
        title: z.string(),
        date: z.date(),
        author: z.string()
      })
    })
  }
})
```

### 2. Page vs Data

- **`page`** – Routes + body (renders as pages)
- **`data`** – Structured data only (no routes)

### 3. Remote Sources

GitHub repos or external APIs as content sources:

```ts
source: {
  repository: 'owner/repo',
  branch: 'main',
  path: '/docs'
}
```

### 4. Query API

SQL-like fluent API for content queries:

```ts
const posts = await queryCollection('blog')
  .where('published', true)
  .sort('date', 'desc')
  .limit(10)
  .find()
```

### 5. MDC Rendering

Vue components inside markdown:

```vue
<ContentRenderer :value="content" />
```

## Project Structure

```
project/
├── content/                    # Content files
│   ├── blog/                   # Maps to 'blog' collection
│   │   └── post.md
│   └── .navigation.yml         # Navigation metadata
├── components/content/         # MDC components
│   └── CustomComponent.vue
└── content.config.ts           # Collection definitions
```

## Implementation Guide

### Installation

```bash
npm install @nuxt/content
```

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  modules: ['@nuxt/content']
})
```

### Define Collections

```ts
// content.config.ts
import { defineCollection, defineContentConfig } from '@nuxt/content'
import { z } from 'zod'

export default defineContentConfig({
  collections: {
    blog: defineCollection({
      type: 'page',
      schema: z.object({
        title: z.string(),
        date: z.date(),
        author: z.string(),
        published: z.boolean().default(false)
      }),
      source: {
        repository: 'owner/repo',
        branch: 'main'
      }
    })
  }
})
```

### Query Content

```ts
// pages/blog/index.vue
<script setup>
const posts = await queryCollection('blog')
  .where('published', true)
  .sort('date', 'desc')
  .find()
</script>
```

### Render Content

```vue
<!-- pages/blog/[slug].vue -->
<script setup>
const route = useRoute()
const content = await queryCollection('blog')
  .where('slug', route.params.slug)
  .findOne()
</script>

<template>
  <ContentRenderer :value="content" />
</template>
```

## References

| File | Purpose |
|------|---------|
| `references/collections.md` | defineCollection, schemas, sources, content.config.ts |
| `references/querying.md` | queryCollection, navigation, search, surroundings |
| `references/rendering.md` | ContentRenderer, MDC syntax, prose components, Shiki |
| `references/config.md` | Database setup, markdown plugins, renderer options |
| `references/studio.md` | NuxtStudio integration, preview mode, live editing |

## Usage Pattern

**Progressive loading - only read what you need:**

- Setting up collections? → `references/collections.md`
- Querying content? → `references/querying.md`
- Rendering markdown/MDC? → `references/rendering.md`
- Configuring database/markdown? → `references/config.md`
- Using NuxtStudio? → `references/studio.md`

**DO NOT read all files at once.** Load based on context.

## Official Documentation

- Nuxt Content: https://content.nuxt.com
- MDC syntax: https://content.nuxt.com/docs/files/markdown#mdc-syntax
- Collections: https://content.nuxt.com/docs/collections/collections

## Token Efficiency

Main skill: ~300 tokens. Each sub-file: ~800-1200 tokens. Only load files relevant to current task.

---

*This skill aligns with the [Drift Skills](https://github.com/dadbodgeoff/drift/wiki/Skills) format: reusable implementation guides so AI agents produce production-grade content-driven Nuxt application code.*
