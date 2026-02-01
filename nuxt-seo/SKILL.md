---
name: nuxt-seo
description: Configure SEO for Nuxt 4+ applications using the Nuxt SEO meta-module. Provides robots.txt, sitemap.xml, dynamic OG image generation, JSON-LD structured data, and SEO utilities. Use when configuring SEO, generating sitemaps, creating OG images, or adding structured data.
license: MIT
compatibility: Nuxt 4+
metadata:
  category: seo
  time: 2h
  source: nuxt-seo-skill
  triggers:
    - "add SEO to Nuxt app"
    - "configure sitemap"
    - "generate OG images"
    - "add structured data"
    - "set up robots.txt"
    - "improve SEO"
---

# Nuxt SEO

Configure SEO for Nuxt 4+ applications with robots.txt, sitemap.xml, dynamic OG images, and structured data. This skill provides a reusable implementation guide so AI agents generate production-quality SEO configurations.

## Quick Start

When the user asks for SEO in a Nuxt app, follow this workflow:

1. **Install module** – Add `@nuxtjs/seo` module
2. **Configure site** – Set up site URL, name, and indexability in `nuxt.config.ts`
3. **Set up robots/sitemap** – Configure robots.txt and sitemap.xml generation
4. **Generate OG images** – Create dynamic OG images with `defineOgImage()`
5. **Add structured data** – Implement JSON-LD with `useSchemaOrg()`
6. **Integrate with content** – Use `asSeoCollection()` for Nuxt Content v3

## When to Use This Skill

- **SEO configuration** – Setting up site URL, name, indexability
- **Robots.txt** – Generating robots.txt with `useRobotsRule()`
- **Sitemap.xml** – Creating sitemaps with `defineSitemapEventHandler()`
- **OG images** – Generating dynamic Open Graph images
- **Structured data** – Adding JSON-LD schema.org markup
- **SEO utilities** – Using breadcrumbs, canonical URLs, meta utilities
- **Content integration** – Integrating SEO with Nuxt Content v3 collections

**Related skills:**
- **Nuxt Content:** use `nuxt-content` skill for MDC rendering with SEO frontmatter

## Core Concepts

### 1. Site Config

Foundation for all SEO modules. Configure `site` in `nuxt.config.ts`, access via `useSiteConfig()`:

```ts
export default defineNuxtConfig({
  site: {
    url: 'https://example.com',
    name: 'My Site',
    description: 'Site description',
    defaultLocale: 'en'
  }
})
```

### 2. Module Overview

| Module            | Purpose         | Key API                       |
| ----------------- | --------------- | ----------------------------- |
| nuxt-site-config  | Shared config   | `useSiteConfig()`             |
| @nuxtjs/robots    | robots.txt      | `useRobotsRule()`             |
| @nuxtjs/sitemap   | sitemap.xml     | `defineSitemapEventHandler()` |
| nuxt-og-image     | OG images       | `defineOgImage()`             |
| nuxt-schema-org   | JSON-LD         | `useSchemaOrg()`              |
| nuxt-seo-utils    | Meta utilities  | `useBreadcrumbItems()`        |
| nuxt-link-checker | Link validation | Build-time checks             |

### 3. Nuxt Content Integration

Use `asSeoCollection()` for automatic sitemap, og-image, and schema-org from frontmatter:

```ts
// content.config.ts
import { defineCollection, defineContentConfig } from '@nuxt/content'
import { asSeoCollection } from '@nuxtjs/seo/content'

export default defineContentConfig({
  collections: {
    posts: defineCollection(asSeoCollection({ type: 'page', source: 'posts/**' }))
  }
})
```

**Important:** Load `@nuxtjs/seo` before `@nuxt/content` in modules array.

## Project Structure

```
project/
├── nuxt.config.ts         # Site config, modules
├── server/
│   └── routes/
│       └── sitemap.xml.ts  # Custom sitemap handler
└── app.vue                 # OG image definitions
```

## Implementation Guide

### Installation

```bash
npx nuxi module add @nuxtjs/seo
```

### Site Configuration

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  modules: ['@nuxtjs/seo'],
  site: {
    url: 'https://example.com',
    name: 'My Site',
    description: 'Site description',
    defaultLocale: 'en'
  }
})
```

### Robots.txt

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  robots: {
    rules: {
      UserAgent: '*',
      Allow: '/',
      Disallow: '/admin'
    }
  }
})
```

### OG Images

```ts
// app.vue or page
defineOgImage({
  title: 'Page Title',
  description: 'Page description',
  image: '/og-image.png'
})
```

### Structured Data

```ts
// composables or pages
useSchemaOrg({
  '@context': 'https://schema.org',
  '@type': 'WebSite',
  name: 'My Site',
  url: 'https://example.com'
})
```

## References

| File | Purpose |
|------|---------|
| `references/site-config.md` | Site configuration, useSiteConfig() options |
| `references/crawlability.md` | Robots.txt, sitemap.xml setup |
| `references/og-image.md` | Dynamic OG image generation |
| `references/schema-org.md` | JSON-LD structured data |
| `references/utilities.md` | Breadcrumbs, canonical URLs, meta utilities |

## Usage Pattern

**Progressive loading - only read what you need:**

- Configuring site? → `references/site-config.md`
- Setting up robots/sitemap? → `references/crawlability.md`
- Generating OG images? → `references/og-image.md`
- Adding JSON-LD? → `references/schema-org.md`
- Breadcrumbs, links, icons? → `references/utilities.md`

## Resources

- [Documentation](https://nuxtseo.com)
- [GitHub](https://github.com/harlan-zw/nuxt-seo)

## Token Efficiency

Main skill: ~250 tokens. Each sub-file: ~400-600 tokens. Only load files relevant to current task.

---

*This skill aligns with the [Drift Skills](https://github.com/dadbodgeoff/drift/wiki/Skills) format: reusable implementation guides so AI agents produce production-grade Nuxt SEO configurations.*
