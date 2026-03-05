---
title: Configurando Tu Entorno
description: Cómo configurar tu entorno de desarrollo para flujos de trabajo agénticos
authors:
  - dpavancini
lastUpdated: '2026-02-19'
---

## Descripción general

Antes de poder trabajar con agents de codificación de AI, necesita un entorno de desarrollo correctamente configurado. Esta página lo guía a través de lo esencial.

## Elija su herramienta de codificación de AI

Varias herramientas admiten flujos de trabajo de desarrollo con agents:

- **Claude Code** — El agent CLI de Anthropic para desarrollo basado en terminal
- **Cursor** — Editor de código nativo de AI basado en VS Code
- **GitHub Copilot** — Programador de pares de AI integrado en VS Code y JetBrains
- **Windsurf** — IDE con tecnología de AI de Codeium

Cada herramienta tiene diferentes fortalezas. Claude Code se destaca en cambios de varios archivos y refactorizaciones complejas. Cursor ofrece una estrecha integración con el IDE. Elija el que mejor se adapte a su flujo de trabajo, o use varios para diferentes tareas.

## Pasos esenciales de configuración

### 1. Control de versiones

Los flujos de trabajo con agents realizan cambios frecuentes. Una configuración sólida de Git es innegociable:

- Inicialice o clone su repository
- Cree una feature branch para el trabajo con agents
- Configure `.gitignore` correctamente
- Considere usar git worktrees para sesiones de agents paralelas

### 2. Contexto del proyecto

Los agents de AI funcionan mejor con context. Proporciónelo a través de:

- **README.md** — Descripción general del proyecto, instrucciones de configuración, arquitectura
- **CLAUDE.md / .cursorrules** — Instrucciones y convenciones específicas del agent
- **Type definitions** — Tipos de TypeScript, esquemas de API, modelos de base de datos
- **Test suites** — Las pruebas existentes informan a los agents sobre el comportamiento esperado

### 3. Configuración del entorno

```bash
# Ejemplo: Configuración de Claude Code
npm install -g @anthropic-ai/claude-code
cd your-project
claude
```

### 4. Red de seguridad

Siempre tenga salvaguardias implementadas:

- **Pre-commit hooks** — Lint y format antes de commit
- **CI/CD pipeline** — Las pruebas automatizadas detectan regresiones
- **Code review** — Revisión humana del código generado por el agent
- **Branch protection** — Evite los push directos a main

## Lista de verificación de verificación

Antes de comenzar a trabajar con agents, verifique:

- [ ] Repository de Git inicializado con un working tree limpio
- [ ] Herramienta de AI instalada y autenticada
- [ ] Archivos de context del proyecto en su lugar (README, type definitions)
- [ ] Linting y formatting configurados
- [ ] Suite de pruebas ejecutada con éxito
- [ ] CI/CD pipeline activo

## Próximos pasos

Con su entorno listo, pase a [Su primer flujo de trabajo con agents](/en/handbook/getting-started/your-first-agentic-workflow) para ponerlo en práctica.
