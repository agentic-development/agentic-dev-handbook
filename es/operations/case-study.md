---
title: 'Estudio de Caso: Del Diseño a la Producción'
description: >-
  Un ejemplo de principio a fin de rutinas de desarrollo agéntico aplicadas a un
  sistema recomendador de comercio electrónico
authors:
  - dpavancini
lastUpdated: '2026-02-24'
---

## Visión general

Esta página describe un ejemplo completo de cómo las rutinas de gobernanza definidas en la página anterior funcionan en conjunto en la práctica. El escenario: un equipo que desarrolla una característica de Recommender System para una plataforma de e-commerce.

## Definición de Feature Charter (Trimestral)

Durante la Definición Estratégica de Especificaciones, el Context Architect crea tres documentos [[feature-charter]] para la iniciativa "Recomendaciones Personalizadas":

1.  **User Behavior Tracking** — Captura eventos de navegación, búsqueda y compra.
2.  **Recommendation Engine** — Construye el ML pipeline que produce recomendaciones.
3.  **Frontend Integration** — Muestra las recomendaciones en la UI del producto.

Cada Feature Charter define su visión arquitectónica, mapa de descomposición y configuración de gobernanza. El charter de Recommendation Engine se clasifica como de alto riesgo (arquitectura novedosa, complejidad del ML pipeline) y se configura con HITL gates obligatorios en cada etapa.

## Tarea de Live Spec (Semanal)

Durante el Bloque de Ingeniería de Especificaciones, el Context Architect produce un Context Packet para la primera Live Spec en el mapa de descomposición del charter User Behavior Tracking: "Implementar la captura de eventos para vistas de página de productos".

El Context Packet incluye:

-   **Live Spec** — Criterios de aceptación: capturar eventos de vista de página con ID de producto, user session, timestamp y datos de viewport. Casos límite: usuarios anónimos, bot traffic filtering, sesiones concurrentes.
-   **Architectural Rules** — Los eventos deben fluir a través del event bus existente. No hay escrituras directas en la base de datos desde la capa de captura. Los eventos cumplen con el CloudEvents schema de la plataforma.
-   **Golden Sample** — Una implementación de referencia de la captura de eventos "agregar al carrito" existente, mostrando los patrones correctos, las naming conventions y la estructura de las pruebas.

## Ejecución del Agent (Diaria)

El Agent Operator alimenta el Context Packet a un Feature Agent. El prompt de CLAUDE Code sigue esta estructura:

```
You are implementing event capture for product page views.

Context Packet:
- Live Spec: [attached]
- Architectural Rules: Events flow through EventBus. No direct DB writes.
  CloudEvents schema required.
- Golden Sample: See add-to-cart event implementation at
  src/events/add-to-cart.ts

Requirements:
1. Implement ProductPageViewEvent following the CloudEvents schema
2. Register the event handler with the EventBus
3. Include bot traffic filtering using the existing BotDetector service
4. Write unit tests covering all acceptance criteria and edge cases

Constraints:
- Do not modify any files outside src/events/ and src/events/__tests__/
- Follow the naming conventions shown in the Golden Sample
- All tests must pass before submitting the PR
```

## Bucle de Retroalimentación Iterativo

El agent produce una implementación inicial. El Evaluation Harness ejecuta tests automatizados. Dos tests fallan: la lógica de bot filtering no maneja el caso límite en el que falta una user agent string, y la validación del CloudEvents schema rechaza el evento porque el campo `source` utiliza el formato incorrecto.

El agent itera: corrige el manejo del user-agent nulo, corrige el formato del campo `source` y vuelve a enviarlo. Todos los tests pasan.

## Agent Recovery

En la segunda tarea — "Implementar la captura de eventos de búsqueda" — el agent se atasca. Intenta importar un módulo del dominio del recommendation engine, violando el bounded context boundary. El Evaluation Harness señala una violación arquitectónica, pero el agent no puede resolverla porque carece de context sobre el patrón correcto de comunicación inter-dominio.

El Agent Operator interviene:

1.  **Diagnostica** el problema: el context del agent no incluye el contrato de messaging inter-dominio.
2.  **Inyecta** el context faltante: la definición de la interfaz AsyncMessageBus y un ejemplo de publishing de eventos entre dominios.
3.  **Reanuda** el agent, que ahora publica correctamente el evento de búsqueda a través del message bus en lugar de importar desde el dominio de recomendación.

## Finalización de la Tarea

El Agent Operator revisa el PR final. El código sigue los patrones del Golden Sample, pasa todos los automated tests y respeta los límites arquitectónicos. El PR es aprobado y merged. El Flow Manager actualiza el AgentOps Dashboard, marcando la tarea como completa y registrando el token consumption contra el presupuesto semanal.

Tiempo total transcurrido desde el Context Packet hasta el PR merged: 47 minutos para dos tareas, incluyendo una Agent Recovery. El trabajo equivalente le habría llevado a un desarrollador humano aproximadamente 1.5-2 días.

## Conclusiones clave

Este ejemplo ilustra varios principios del framework:

-   **La calidad del context determina la calidad de la ejecución.** La primera tarea tuvo éxito en la segunda iteración porque el Context Packet era exhaustivo. La segunda tarea requirió una Agent Recovery porque el patrón de comunicación inter-dominio faltaba en el context.
-   **El Evaluation Harness detecta errores que el agent no puede autodiagnosticar.** Las violaciones arquitectónicas son invisibles para el agent sin restricciones explícitas. Las comprobaciones automatizadas detectaron lo que el agent no pudo.
-   **Las Agent Recoveries son de diagnóstico, no punitivas.** El operator no reescribió el código — identificó el context faltante, lo inyectó y dejó que el agent completara el trabajo.
-   **El ciclo de vida completo se conecta.** La planificación trimestral produjo los Feature Charters, la ingeniería de especificaciones semanal produjo los context packets y la ejecución diaria los consumió. Cada cadencia alimenta a la siguiente.

## Qué Sigue

La siguiente página cubre las métricas y los frameworks de seguimiento de éxito que miden si estas rutinas realmente funcionan — desde las leverage ratios y la flow efficiency hasta las métricas económicas que justifican la inversión en infraestructura agentic.
