---
title: Tu Primer Flujo de Trabajo con Agentes
description: >-
  Un recorrido paso a paso para completar una tarea con un agente de
  codificación de AI
authors:
  - dpavancini
lastUpdated: '2026-02-19'
---

## Resumen

Esta página le guía a través de un flujo de trabajo de desarrollo *agentic* completo —desde la definición de una tarea hasta la revisión del resultado. Usaremos un ejemplo sencillo: añadir una función de utilidad con pruebas.

## El flujo de trabajo

El desarrollo *agentic* sigue un ciclo predecible:

1.  **Definir** — Establezca claramente lo que desea
2.  **Context** — Asegúrese de que el *agent* tenga lo que necesita
3.  **Ejecutar** — Deje que el *agent* trabaje
4.  **Revisar** — Verifique el resultado
5.  **Iterar** — Perfeccione si es necesario

## Paso 1: Definir la tarea

Las buenas definiciones de tareas son específicas y acotadas. Compare:

**Vago:** "Haga la aplicación más rápida"

**Específico:** "Añada una función de utilidad `debounce` en `src/utils/debounce.ts` que acepte una función y un retraso en milisegundos, y devuelva una versión *debounced*. Incluya pruebas unitarias."

Cuanto más precisa sea su solicitud, mejor será el resultado.

## Paso 2: Proporcionar Context

Dirija el *agent* a los archivos relevantes:

-   Funciones de utilidad existentes (para consistencia de estilo)
-   La configuración del *test framework*
-   Convenciones de tipos utilizadas en el proyecto

La mayoría de las herramientas detectan el *context* automáticamente, pero las referencias explícitas ayudan.

## Paso 3: Dejar que el Agent trabaje

Inicie la tarea y observe. Un *agent* bien configurado hará lo siguiente:

1.  Leer archivos relacionados para entender las convenciones
2.  Escribir la implementación
3.  Escribir pruebas
4.  Ejecutar las pruebas para verificar
5.  Corregir cualquier problema

Resista la tentación de interrumpir a mitad del flujo. Deje que el *agent* complete su ciclo, luego revise.

## Paso 4: Revisar el resultado

Revise el trabajo del *agent* como lo haría con el *pull request* de un colega:

-   **Corrección** — ¿Hace lo que pidió?
-   **Estilo** — ¿Coincide con las convenciones del proyecto?
-   **Casos extremos** — ¿Se manejan las condiciones de contorno?
-   **Pruebas** — ¿Son significativas, no solo pasan?
-   **Sin extras** — ¿Añadió código o dependencias innecesarias?

## Paso 5: Iterar

Si algo necesita ajuste, sea específico:

**En lugar de:** "Arréglelo"

**Intente:** "La función `debounce` debería cancelar las llamadas pendientes cuando el componente se desmonte. Añada un método `cancel` a la función devuelta."

## Patrones comunes

A medida que gane experiencia, reconocerá patrones que funcionan bien con los *agents*. Explore la sección [Patrones](/en/patterns) para una colección curada, incluyendo:

-   [Prompt Chaining](/en/patterns) — Dividir tareas complejas en pasos secuenciales
-   Test-Driven Development — Escribir pruebas primero, luego hacer que el *agent* implemente

## Consejos para el éxito

-   **Empiece pequeño** — Gane confianza con tareas simples antes que con las complejas
-   **Sea explícito** — Los *agents* siguen las instrucciones literalmente; la ambigüedad lleva a sorpresas
-   **Confíe, pero verifique** — Siempre revise el código generado antes de hacer un *merge*
-   **Aprenda las herramientas** — Cada herramienta de AI tiene fortalezas únicas; aprenda a aprovecharlas
-   **Itere rápido** — Los ciclos de retroalimentación rápidos producen mejores resultados que las especificaciones extensas

## Qué sigue

Está listo para empezar a usar el desarrollo *agentic* en sus proyectos. Explore el resto de este manual para profundizar en temas específicos, o navegue por las [Plantillas](/en/templates) para *prompts* listos para usar.
