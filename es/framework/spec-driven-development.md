---
title: Desarrollo Impulsado por Especificaciones
description: >-
  Cómo las especificaciones legibles por máquina reemplazan las historias de
  usuario como la unidad de trabajo en el desarrollo agéntico
authors:
  - dpavancini
lastUpdated: '2026-02-24'
---

## Resumen

El Desarrollo Guiado por Especificaciones (Spec-Driven Development) reemplaza las historias de usuario informales y las descripciones de tickets con especificaciones legibles por máquina que sirven como contratos determinísticos entre humanos y agents. Es la práctica que hace operativos los cuatro Pilares Fundamentales; sin especificaciones precisas, el contexto está incompleto, las compuertas de gobernanza no tienen nada contra lo que verificar, y las decisiones de enrutamiento carecen de la información que necesitan.

## ¿Qué es una Especificación?

Una especificación es un contrato determinístico y legible por máquina que define:

- **Qué** necesita construirse (requisitos de comportamiento)
- **Por qué** es importante (contexto empresarial y restricciones)
- **Cómo verificar** que funciona (criterios de aceptación ejecutables)

A diferencia de una historia de usuario ("Como usuario, quiero..."), una especificación no deja lugar a interpretaciones. Es lo suficientemente precisa como para que un agent pueda implementarla sin hacer preguntas aclaratorias y verificar su propio trabajo contra los criterios de aceptación.

## Los Tres Activos Mínimos

Toda Especificación Viva (Live Spec) debe contener al menos tres activos:

1.  **Contrato de Comportamiento** — Una descripción precisa del comportamiento esperado, incluyendo entradas, salidas, casos extremos y manejo de errores. Esto es lo que implementa el agent.

2.  **Constitución del Sistema** — El [[system-prompt]] y las restricciones que gobiernan cómo opera el agent. Esto incluye estándares de codificación, patrones arquitectónicos, políticas de seguridad y cualquier regla específica del dominio. Define los límites de las soluciones aceptables.

3.  **Mapa de Tareas Accionable** — Una lista descompuesta y ordenada de pasos de implementación que el agent puede seguir. Cada paso hace referencia a la sección relevante del Contrato de Comportamiento e incluye sus propios criterios de aceptación.

## Similitudes con TDD

El Desarrollo Guiado por Especificaciones comparte ADN con el Desarrollo Guiado por Pruebas (Test-Driven Development, TDD). Ambos enfoques definen el comportamiento esperado antes de escribir la implementación. Las diferencias clave:

-   **Alcance** — Las especificaciones de TDD son a nivel de función; las Especificaciones Vivas cubren funcionalidades o flujos de trabajo completos.
-   **Audiencia** — Las pruebas de TDD están escritas para ejecutores de pruebas; las Especificaciones Vivas están escritas para agents y humanos.
-   **Riqueza** — Las Especificaciones Vivas incluyen contexto arquitectónico, justificación y descomposición que va más allá de lo que un archivo de prueba captura.

Los equipos que ya practican TDD encontrarán natural la transición al Desarrollo Guiado por Especificaciones. La disciplina de "definir primero el comportamiento esperado" es la misma, lo que se expande es el formato y el alcance.

## Marcos de Trabajo Alternativos

Han surgido varios marcos de trabajo (frameworks) para estructurar los flujos de trabajo guiados por especificaciones:

-   **BMAD Method** — Un framework integral para definir tareas de agent con prompts estructurados y especificaciones de comportamiento.
-   **GitHub Spec Kit** — El enfoque de GitHub para estructurar especificaciones para flujos de trabajo dirigidos por Copilot.
-   **Tessl** — Un enfoque a nivel de plataforma para la orquestación de agent guiada por especificaciones.

Cada uno adopta un ángulo diferente, pero todos comparten el principio fundamental: los agents necesitan una entrada estructurada y legible por máquina para producir resultados fiables.

## Desafíos

La adopción de flujos de trabajo guiados por especificaciones no está exenta de fricción:

-   **Percepción de pérdida de velocidad** — Escribir especificaciones detalladas parece más lento que "solo codificar". Los equipos deben internalizar que la especificación es el trabajo, no una sobrecarga antes del trabajo.
-   **Trampa del mantenimiento** — Las especificaciones que no se mantienen sincronizadas con la base de código se vuelven engañosas. Las especificaciones obsoletas son peores que ninguna especificación porque los agents las siguen fielmente.
-   **Brecha de traducción** — Los interesados del negocio piensan en resultados; los agents necesitan contratos técnicos precisos. Cerrar esta brecha es la habilidad fundamental del Arquitecto de Contexto.
-   **Degradación del contexto** — A medida que los sistemas crecen, el contexto total requerido para especificar un cambio puede superar lo que cabe en una sola context window. El diseño modular de especificaciones y la recuperación de contexto basada en RAG mitigan esto.
-   **Ambigüedad semántica** — El lenguaje natural es intrínsecamente ambiguo. Incluso las especificaciones bien escritas pueden ser interpretadas de manera diferente por distintos agents o versiones de modelos. Los criterios de aceptación ejecutables son el antídoto.

## El Concepto de Especificación Viva

Una Especificación Viva (Live Spec) no es un documento estático. Es un plano modular y versionado que evoluciona junto con la base de código:

-   **Versionado** — Las especificaciones residen en el repositorio junto con el código que describen, con un historial completo de Git.
-   **Modular** — Las funcionalidades grandes se descomponen en múltiples especificaciones que se referencian entre sí.
-   **Ejecutable** — Los criterios de aceptación se escriben como verificaciones automatizadas, no como descripciones en prosa.
-   **Evolutivo** — Las especificaciones se actualizan a medida que los requisitos cambian, la implementación revela nuevos casos extremos o la arquitectura del sistema cambia.

Este enfoque es una evolución del [[prompt-driven-development]], pasando de prompts ad-hoc a especificaciones estructuradas y persistentes que acumulan conocimiento institucional.

## El Ciclo de Desarrollo Continuo

Los cuatro Pilares Fundamentales y el Desarrollo Guiado por Especificaciones se unen en el Ciclo de Desarrollo Continuo (CDL) — la evolución agéntica de CI/CD que automatiza todo el ciclo de vida, desde la especificación hasta la producción. El CDL extiende el pipeline tradicional de construcción-prueba-despliegue con inyección de especificaciones, ensamblaje automatizado de contexto, el Eval Harness como la compuerta de calidad principal y ciclos de retroalimentación centrados en el conocimiento que enriquecen el Índice de Contexto después de cada ciclo de ejecución.

Para un desglose detallado de las fases del CDL, sus equivalentes de CI/CD y las mejoras arquitectónicas clave, consulte [De Agile a Agéntico](/en/handbook/framework/from-agile-to-agentic).

## Próximos Pasos

Una vez definidos los pilares arquitectónicos y las prácticas guiadas por especificaciones, la siguiente página explora cómo estos conceptos se mapean a las ceremonias y roles Agile familiares. [De Agile a Agéntico](/en/handbook/framework/from-agile-to-agentic) cubre la tabla comparativa, la estrategia de transición y la evolución de los coding agents que hace posible este cambio.
