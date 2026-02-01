---
name: nuxt-better-auth
description: Implement authentication in Nuxt 4+ applications using @onmax/nuxt-better-auth. Provides useUserSession composable, server auth helpers, route protection, and Better Auth plugins integration. Use when adding authentication, protecting routes, or integrating auth plugins.
license: MIT
compatibility: Nuxt 4+, Better Auth
metadata:
  category: authentication
  time: 3h
  source: nuxt-better-auth-skill
  triggers:
    - "add authentication to Nuxt app"
    - "implement login/signup"
    - "protect Nuxt routes"
    - "add auth middleware"
    - "integrate Better Auth plugins"
    - "set up user session"
---

# Nuxt Better Auth

Authentication module for Nuxt 4+ built on [Better Auth](https://www.better-auth.com/). Provides composables, server utilities, and route protection for production-grade authentication.

> **Alpha Status**: This module is currently in alpha (v0.0.2-alpha.14) and not recommended for production use. APIs may change.

## Quick Start

When the user asks for authentication in a Nuxt app, follow this workflow:

1. **Install module** – Add `@onmax/nuxt-better-auth` and configure environment variables
2. **Set up database** – Configure NuxtHub or external database with Drizzle schema
3. **Implement client auth** – Create login/signup forms using `useUserSession()`
4. **Protect routes** – Add route rules and middleware for protected pages
5. **Add server protection** – Use `requireUserSession()` in API routes
6. **Integrate plugins** – Add Better Auth plugins (admin, passkey, 2FA) if needed

## When to Use This Skill

- **Installing/configuring** – Setting up `@onmax/nuxt-better-auth` module
- **Login/signup flows** – Implementing authentication forms and flows
- **Route protection** – Protecting client and server routes
- **Session management** – Accessing user session in components and API routes
- **Better Auth plugins** – Integrating admin, passkey, 2FA plugins
- **Database setup** – Configuring NuxtHub or external database with auth tables
- **External auth backends** – Using clientOnly mode for external auth services

**Related skills:**
- **Nuxt patterns:** use `nuxt` skill
- **NuxtHub database:** use `nuxthub` skill

## Core Concepts

### 1. Client Authentication

Use `useUserSession()` composable for client-side auth:

```ts
const { user, loggedIn, signIn, signOut } = useUserSession()
await signIn.email({ email, password }, { onSuccess: () => navigateTo('/') })
```

### 2. Server Protection

Use `requireUserSession()` in server routes:

```ts
const { user } = await requireUserSession(event, { user: { role: 'admin' } })
```

### 3. Route Protection

Configure route rules in `nuxt.config.ts`:

```ts
routeRules: {
  '/admin/**': { auth: { user: { role: 'admin' } } },
  '/login': { auth: 'guest' },
  '/app/**': { auth: 'user' }
}
```

### 4. Auth Modes

Route `auth` mode options:
- `'user'` – Requires authenticated user
- `'guest'` – Requires unauthenticated user
- `{ user: {...} }` – Requires user matching conditions
- `false` – No auth requirement

## Project Structure

```
project/
├── server/
│   └── api/
│       └── protected/      # Protected API routes
├── pages/
│   ├── login.vue          # Login page (auth: 'guest')
│   ├── signup.vue         # Signup page (auth: 'guest')
│   └── app/
│       └── dashboard.vue  # Protected page (auth: 'user')
├── middleware/
│   └── auth.ts            # Custom auth middleware
└── nuxt.config.ts         # Route rules configuration
```

## Implementation Guide

### Installation

```bash
npm install @onmax/nuxt-better-auth
```

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  modules: ['@onmax/nuxt-better-auth'],
  betterAuth: {
    baseURL: process.env.BETTER_AUTH_URL,
    secret: process.env.BETTER_AUTH_SECRET
  }
})
```

### Client Auth

```vue
<!-- pages/login.vue -->
<script setup>
const { signIn } = useUserSession()
const email = ref('')
const password = ref('')

async function handleLogin() {
  await signIn.email(
    { email: email.value, password: password.value },
    { onSuccess: () => navigateTo('/dashboard') }
  )
}
</script>
```

### Server Protection

```ts
// server/api/protected/user.ts
export default defineEventHandler(async (event) => {
  const { user } = await requireUserSession(event)
  return { user }
})
```

### Route Rules

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  routeRules: {
    '/admin/**': { auth: { user: { role: 'admin' } } },
    '/login': { auth: 'guest' },
    '/app/**': { auth: 'user' }
  }
})
```

## References

| File | Purpose |
|------|---------|
| `references/installation.md` | Module setup, env vars, config files |
| `references/client-auth.md` | useUserSession, signIn/signUp/signOut, BetterAuthState, safe redirects |
| `references/server-auth.md` | serverAuth, getUserSession, requireUserSession |
| `references/route-protection.md` | routeRules, definePageMeta, middleware |
| `references/plugins.md` | Better Auth plugins (admin, passkey, 2FA) |
| `references/database.md` | NuxtHub integration, Drizzle schema, custom tables with FKs |
| `references/client-only.md` | External auth backend, clientOnly mode, CORS |
| `references/types.md` | AuthUser, AuthSession, type augmentation |

## Usage Pattern

**Load based on context:**

- Installing module? → `references/installation.md`
- Login/signup forms? → `references/client-auth.md`
- API route protection? → `references/server-auth.md`
- Route rules/page meta? → `references/route-protection.md`
- Using plugins? → `references/plugins.md`
- Database setup? → `references/database.md`
- External auth backend? → `references/client-only.md`
- TypeScript types? → `references/types.md`

**DO NOT read all files at once.** Load based on context.

## Resources

- [Module Docs](https://github.com/onmax/nuxt-better-auth)
- [Better Auth Docs](https://www.better-auth.com/)

## Token Efficiency

Main skill: ~300 tokens. Each sub-file: ~800-1200 tokens. Only load files relevant to current task.

---

*This skill aligns with the [Drift Skills](https://github.com/dadbodgeoff/drift/wiki/Skills) format: reusable implementation guides so AI agents produce production-grade Nuxt authentication code.*
