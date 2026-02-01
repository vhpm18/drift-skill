# Información Relevante de Drift Wiki

Información extraída de [Drift Wiki](https://github.com/dadbodgeoff/drift/wiki) para la integración de Skills.

## Sobre Drift

**Drift** es una herramienta de inteligencia de codebase para agentes de IA que:
- Escanea el código y aprende los patrones del proyecto
- Proporciona a los agentes de IA un entendimiento profundo de las convenciones
- Incluye 50+ herramientas MCP y 60+ comandos CLI
- Soporta 10 lenguajes con núcleo nativo en Rust

### Versión Actual
- **CLI (driftdetect)**: 0.9.40
- **MCP Server (driftdetect-mcp)**: 0.9.39
- **Core (driftdetect-core)**: 0.9.39
- **Native (driftdetect-native)**: 0.9.39

## Instalación y Configuración

### Instalación Rápida

```bash
# Instalar CLI
npm install -g driftdetect

# Escanear proyecto
cd your-project
drift init
drift scan

# Ver estado
drift status
```

### Integración con MCP (Model Context Protocol)

```bash
# Instalar servidor MCP
npm install -g driftdetect-mcp
```

Configuración en el archivo de configuración del agente de IA:

```json
{
  "mcpServers": {
    "drift": {
      "command": "driftdetect-mcp"
    }
  }
}
```

## Características Clave para Skills

### 1. Cortex Memory System

Drift incluye un sistema de memoria llamado **Cortex** que reemplaza los archivos `AGENTS.md` estáticos:

```bash
# Inicializar memoria
drift memory init

# Agregar conocimiento tribal
drift memory add tribal "Always use bcrypt for passwords" --importance critical
drift memory add tribal "Services should not call controllers" --topic Architecture
```

**Características:**
- El contexto se obtiene dinámicamente
- Aprende de correcciones
- La confianza decae en conocimiento obsoleto

### 2. Pattern Learning

Drift descubre automáticamente los patrones del codebase:
- Patrones de API (rutas, middleware, formato de respuesta)
- Patrones de autenticación (decoradores, guards, middleware)
- Patrones de errores (try/catch, Result types, boundaries)

Los patrones pueden ser aprobados manualmente:
```bash
drift approve <pattern-id>
```

### 3. AI Context Tools

Drift proporciona herramientas para que los agentes obtengan contexto:

```typescript
drift_context({
  intent: "add_feature",
  focus: "auth"
})
```

Retorna:
- Patrones con ejemplos
- Código similar en el codebase
- Archivos a modificar
- Advertencias de seguridad
- Restricciones a satisfacer

## Integración de Skills con Drift

### Cómo Funciona

1. **Detección Automática**: Drift escanea el proyecto y detecta Skills disponibles
2. **Activación por Triggers**: Las Skills se activan cuando los agentes detectan los triggers definidos en el frontmatter
3. **Contexto Dinámico**: Drift proporciona contexto relevante de la Skill al agente

### Herramientas MCP para Skills

Drift expone herramientas MCP que pueden ser usadas por agentes de IA:

- **drift_context**: Obtener contexto para una tarea específica
- **drift_similar**: Encontrar código similar
- **drift_explain**: Explicar código en contexto
- **drift_validate_change**: Validar cambios propuestos
- **drift_suggest_changes**: Sugerir cambios basados en patrones

### Comandos CLI Relevantes

```bash
# Estado del proyecto
drift status

# Escanear proyecto
drift scan

# Ver patrones descubiertos
drift patterns list

# Aprobar un patrón
drift approve <pattern-id>

# Validar cambios
drift validate <file>

# Obtener contexto
drift context --intent add_feature --focus auth
```

## Estructura de Skills Compatible con Drift

### Requisitos para Skills

1. **Frontmatter YAML**: Metadata completa con triggers
2. **Estructura de Carpetas**: Organización clara con `SKILL.md`, `references/`, `assets/`
3. **Triggers Específicos**: Frases que activan la Skill automáticamente
4. **Ejemplos de Código**: Código real y funcional
5. **Referencias Organizadas**: Documentación dividida por temas

### Formato de Metadata

```yaml
---
name: skill-name
description: Descripción completa
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

## Estadísticas de Drift (v0.9.40)

- **10 Lenguajes** soportados
- **21 Frameworks** detectados
- **16 ORMs** reconocidos
- **400+ Detectors** de patrones
- **50+ MCP Tools** disponibles
- **60+ CLI Commands** para gestión

## Mejores Prácticas para Skills

### 1. Triggers Específicos

Los triggers deben ser frases naturales que los usuarios dirían:
- ✅ "build a Laravel API"
- ✅ "add authentication to Nuxt app"
- ❌ "laravel" (demasiado genérico)

### 2. Contexto Progresivo

Las Skills deben cargar información de forma progresiva:
- Cargar solo referencias relevantes
- No cargar todo el contenido de una vez
- Minimizar uso de tokens

### 3. Ejemplos Reales

Incluir código que funcione realmente:
- Ejemplos completos, no fragmentos
- Código que sigue las convenciones del proyecto
- Casos de uso comunes

### 4. Referencias Organizadas

Dividir documentación en archivos temáticos:
- Un archivo por concepto principal
- Nombres descriptivos
- Enlaces cruzados cuando sea necesario

## Integración con Cortex Memory

Las Skills pueden complementar el sistema de memoria de Cortex:

```bash
# Agregar conocimiento de una Skill
drift memory add skill "laravel-api" "Use invokable controllers only" --importance high
```

Esto permite que Drift aprenda de las Skills y las integre con el conocimiento del proyecto.

## Recursos Adicionales

- [Drift GitHub](https://github.com/dadbodgeoff/drift)
- [Drift Wiki](https://github.com/dadbodgeoff/drift/wiki)
- [MCP Setup Guide](https://github.com/dadbodgeoff/drift/wiki/MCP-Setup)
- [Skills Documentation](https://github.com/dadbodgeoff/drift/wiki/Skills)
- [Cortex Memory System](https://github.com/dadbodgeoff/drift/wiki/Cortex-V2-Overview)

## Notas Importantes

1. **Drift escanea el código**: No requiere configuración manual de Skills si están en un directorio accesible
2. **Activación automática**: Los triggers en el frontmatter activan las Skills automáticamente
3. **Contexto dinámico**: Drift proporciona contexto relevante basado en la tarea actual
4. **Aprendizaje continuo**: Cortex aprende de correcciones y actualizaciones

---

*Información extraída de [Drift Wiki](https://github.com/dadbodgeoff/drift/wiki) - Última actualización: Febrero 2026*
