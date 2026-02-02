# Drift Skills Collection

Colección de Skills para [Drift](https://github.com/dadbodgeoff/drift) - Guías de implementación reutilizables para que los agentes de IA generen código de calidad producción.

## ¿Qué son las Skills?

Las Skills son guías de implementación curadas y listas para producción que los agentes de IA pueden usar para implementar patrones comunes correctamente. Cada skill contiene:

- **SKILL.md** — Guía completa de implementación con ejemplos de código
- **Metadata** — Categoría, tiempo estimado, compatibilidad, triggers
- **Referencias** — Documentación adicional organizada por tema
- **Templates** (opcional) — Plantillas para scaffolding

Las Skills siguen el formato estándar de [Drift Skills](https://github.com/dadbodgeoff/drift/wiki/Skills) y se activan automáticamente cuando los agentes detectan los triggers definidos en el frontmatter.

## Skills Disponibles

### Backend

| Skill | Descripción | Tiempo | Categoría |
|-------|-------------|--------|-----------|
| **[laravel-api](./laravel-api/SKILL.md)** | Build production-grade Laravel REST APIs using opinionated architecture patterns. Stateless design, versioned endpoints, invokable controllers, Form Request DTOs, Action classes, JWT authentication, and PSR-12 code quality. | 6h | api |

### Frontend

| Skill | Descripción | Tiempo | Categoría |
|-------|-------------|--------|-----------|
| **[nuxt](./nuxt/SKILL.md)** | Build production-grade Nuxt 4+ applications using server routes, file-based routing, middleware patterns, Nuxt-specific composables, and configuration. | 4h | frontend |
| **[vue](./vue/SKILL.md)** | Build Vue 3 components, composables, and utilities using Composition API patterns. Provides props/emits best practices, reactive destructuring guidance, VueUse integration, TypeScript patterns, and testing practices. | 3h | frontend |
| **[reka-ui](./reka-ui/SKILL.md)** | Build accessible Vue 3 components using Reka UI (headless component primitives). WAI-ARIA compliant. Provides component API, accessibility patterns, composition (asChild), controlled/uncontrolled state, virtualization, and styling integration. | 3h | frontend |
| **[vueuse](./vueuse/SKILL.md)** | Use VueUse composables for reactive utilities (state, browser APIs, sensors, network, animations). Check VueUse before writing custom composables - most patterns already implemented. | 2h | frontend |
| **[frontend-design](./frontend-design/SKILL.md)** | Design distinctive, production-grade frontend interfaces with bold, non-generic aesthetics and high UX polish. Use for building components, pages or full apps with strong visual direction. | 3h | frontend-design |
| **[web-design-guidelines](./web-design-guidelines/SKILL.md)** | Review UI code against Vercel Web Interface Guidelines to catch accessibility, UX, and implementation issues, producing concise file:line audits. | 3h | frontend |

### Nuxt Modules & Integrations

| Skill | Descripción | Tiempo | Categoría |
|-------|-------------|--------|-----------|
| **[nuxt-better-auth](./nuxt-better-auth/SKILL.md)** | Implement authentication in Nuxt 4+ applications using @onmax/nuxt-better-auth. Provides useUserSession composable, server auth helpers, route protection, and Better Auth plugins integration. | 3h | authentication |
| **[nuxt-content](./nuxt-content/SKILL.md)** | Build content-driven Nuxt 4+ applications using Nuxt Content v3. Provides collections (local/remote/API sources), queryCollection API, MDC rendering, database configuration, NuxtStudio integration, hooks, i18n patterns, and LLMs integration. | 4h | content |
| **[nuxt-modules](./nuxt-modules/SKILL.md)** | Create Nuxt modules for published npm packages, local project modules, or runtime extensions. Provides defineNuxtModule patterns, Kit utilities, hooks, E2E testing, and release automation. | 5h | infrastructure |
| **[nuxt-seo](./nuxt-seo/SKILL.md)** | Configure SEO for Nuxt 4+ applications using the Nuxt SEO meta-module. Provides robots.txt, sitemap.xml, dynamic OG image generation, JSON-LD structured data, and SEO utilities. | 2h | seo |
| **[nuxthub](./nuxthub/SKILL.md)** | Build full-stack Nuxt 4+ applications with NuxtHub v0.10.4. Provides database (Drizzle ORM with sqlite/postgresql/mysql), KV storage, blob storage, and cache APIs. Multi-cloud deployment support. | 6h | infrastructure |

## Cómo Usar las Skills

### Integración con Drift

Las Skills se integran automáticamente con Drift cuando están en un directorio accesible. Drift detecta las Skills mediante:

1. **Estructura de carpetas** — Cada skill debe estar en su propia carpeta con `SKILL.md`
2. **Frontmatter con triggers** — Los triggers definen cuándo activar la skill
3. **Metadata completa** — Categoría, tiempo, compatibilidad, etc.

### Comandos CLI de Drift

```bash
# Listar skills disponibles
drift skills list

# Instalar una skill
drift skills install <skill-name>

# Desinstalar una skill
drift skills uninstall <skill-name>

# Ver detalles de una skill
drift skills show <skill-name>

# Filtrar por categoría
drift skills list --category frontend

# Buscar skills
drift skills search "authentication"
```

### Uso Automático

Los agentes de IA activan automáticamente las Skills cuando detectan los triggers en las consultas del usuario:

- **Ejemplo:** Si el usuario dice "build a Laravel API", Drift activa automáticamente la skill `laravel-api`
- **Ejemplo:** Si el usuario dice "add authentication to Nuxt app", Drift activa `nuxt-better-auth`

### Uso Manual

Puedes referenciar una skill específica en tu consulta:

```
Usa la skill laravel-api para crear un endpoint de tareas
```

## Estructura de una Skill

Cada skill sigue esta estructura estándar:

```
skill-name/
├── SKILL.md              # Guía principal (requerido)
├── references/           # Documentación adicional
│   ├── topic1.md
│   └── topic2.md
├── assets/               # Templates y assets (opcional)
│   └── templates/
│       └── template.php
└── scripts/              # Scripts de automatización (opcional)
    └── generate.ts
```

### Formato de SKILL.md

Cada `SKILL.md` debe incluir:

1. **Frontmatter** — Metadata en formato YAML
2. **Quick Start** — Workflow paso a paso
3. **When to Use This Skill** — Casos de uso
4. **Core Concepts** — Conceptos clave organizados
5. **Project Structure** — Estructura de directorios
6. **Implementation Guide** — Ejemplos de código
7. **References** — Tabla de archivos de referencia
8. **Nota final** — Alineación con Drift Skills

Ejemplo de frontmatter:

```yaml
---
name: skill-name
description: Descripción completa de la skill
license: MIT
compatibility: Framework 1.0+
metadata:
  category: frontend
  time: 3h
  source: skill-name-skill
  triggers:
    - "build something"
    - "create feature"
---
```

## Cómo Agregar una Nueva Skill

### 1. Crear la Estructura

```bash
mkdir nueva-skill
cd nueva-skill
mkdir references assets/templates scripts
```

### 2. Crear SKILL.md

Copia el formato de `laravel-api/SKILL.md` como plantilla y ajusta:

- Frontmatter con metadata correcta
- Quick Start workflow
- Core Concepts específicos
- Implementation Guide con ejemplos
- References table

### 3. Agregar Referencias

Crea archivos en `references/` para documentación adicional:

- Organiza por temas específicos
- Usa nombres descriptivos
- Mantén cada archivo enfocado en un tema

### 4. Agregar Templates (Opcional)

Si la skill incluye templates, colócalos en `assets/templates/`:

- Usa placeholders como `{Resource}`, `{Operation}`
- Documenta cómo usar cada template
- Incluye ejemplos de uso

### 5. Verificar

```bash
# Verificar formato
drift skills validate nueva-skill

# Probar la skill
drift skills test nueva-skill
```

## Categorías de Skills

Las skills están organizadas por categorías:

- **api** — REST APIs, GraphQL, integraciones
- **authentication** — Autenticación y autorización
- **content** — Gestión de contenido, CMS
- **frontend** — Componentes, UI, frameworks frontend
- **infrastructure** — Deployment, CI/CD, módulos
- **seo** — SEO, meta tags, sitemaps

## Mejores Prácticas

### Para Desarrolladores de Skills

1. **Sé específico** — Define claramente cuándo usar la skill
2. **Proporciona ejemplos** — Incluye código real y funcional
3. **Organiza referencias** — Divide documentación en archivos temáticos
4. **Actualiza triggers** — Asegúrate de que los triggers sean relevantes
5. **Mantén compatibilidad** — Especifica versiones compatibles

### Para Usuarios de Skills

1. **Revisa la skill primero** — Lee `SKILL.md` antes de usar
2. **Carga progresivamente** — Solo carga referencias relevantes
3. **Sigue el workflow** — Usa el Quick Start como guía
4. **Personaliza según necesidad** — Adapta ejemplos a tu proyecto

## Integración con Drift

Para información detallada sobre cómo Drift detecta y usa las Skills, consulta:

- **[DRIFT_INTEGRATION.md](./DRIFT_INTEGRATION.md)** — Información extraída de la wiki de Drift sobre integración, MCP, Cortex Memory System, y herramientas disponibles

### Características Clave

- **Detección Automática**: Drift escanea y detecta Skills automáticamente
- **Cortex Memory System**: Integración con el sistema de memoria de Drift
- **MCP Tools**: 50+ herramientas MCP para acceso a Skills
- **Pattern Learning**: Drift aprende patrones y los integra con Skills

## Recursos

- [Drift Skills Wiki](https://github.com/dadbodgeoff/drift/wiki/Skills) — Documentación oficial
- [Drift GitHub](https://github.com/dadbodgeoff/drift) — Repositorio principal
- [Drift Wiki](https://github.com/dadbodgeoff/drift/wiki) — Wiki completa
- [MCP Setup Guide](https://github.com/dadbodgeoff/drift/wiki/MCP-Setup) — Guía de configuración MCP
- [Cortex Memory System](https://github.com/dadbodgeoff/drift/wiki/Cortex-V2-Overview) — Sistema de memoria
- [Agent Skills Specification](https://agentskills.io/home) — Especificación estándar

## Contribuir

Las contribuciones son bienvenidas. Para agregar o mejorar una skill:

1. Fork el repositorio
2. Crea una nueva skill o mejora una existente
3. Sigue el formato estándar de `laravel-api/SKILL.md`
4. Agrega tests si es posible
5. Envía un pull request

## Licencia

MIT — Ver archivo LICENSE para más detalles.

---

*Este repositorio contiene Skills que siguen el formato [Drift Skills](https://github.com/dadbodgeoff/drift/wiki/Skills) para guías de implementación reutilizables que ayudan a los agentes de IA a generar código de calidad producción.*
