---
title: De Agile a Agéntico
description: >-
  Cómo el marco de Desarrollo Agéntico evoluciona las prácticas Agile para
  equipos de software nativos de IA
authors:
  - dpavancini
lastUpdated: '2026-02-23'
---

## Panorama general

Agile transformó el desarrollo de software de procesos rígidos y basados en planes a una entrega iterativa y centrada en las personas. [[agentic-engineering|Agentic Development]] es la próxima evolución, que preserva el énfasis de Agile en la retroalimentación y la adaptabilidad, al tiempo que cambia fundamentalmente lo que limita la entrega, quién (o qué) realiza el trabajo y cómo se garantiza la calidad.

## Por qué Agile por sí solo no es suficiente

Agile fue diseñado para un mundo donde los developers humanos son el cuello de botella en la ejecución. Sus ceremonias, roles y artefactos optimizan la coordinación del esfuerzo humano a través de iteraciones con límite de tiempo. Ese modelo se rompe cuando los agent autónomos pueden ejecutar tareas bien definidas en minutos en lugar de días.

Las tensiones principales:

- **Los Sprints asumen un ritmo humano.** Las iteraciones de dos semanas tienen sentido cuando la implementación lleva días. Cuando los agent pueden implementar una feature en minutos, los límites del Sprint se convierten en restricciones artificiales en lugar de horizontes de planificación útiles.
- **Las user stories asumen interpretación humana.** "Como usuario, quiero..." se basa en el juicio de un developer para rellenar las brechas. Los agent necesitan precisión legible por máquina, no flexibilidad narrativa.
- **La revisión manual de código no escala.** Cuando los agent producen código en volumen, los revisores humanos se convierten en el cuello de botella. La evaluación automatizada debe encargarse de la mayor parte del control de calidad.

Agentic development no abandona Agile, sino que lo evoluciona. Los valores de colaboración con el cliente, respuesta al cambio y software funcional sobre documentación se mantienen. Las prácticas cambian para adaptarse a un nuevo modelo de ejecución.

## Diferencias clave

### De la limitación de tiempo a la limitación de contexto

En Agile, la limitación principal es el tiempo. Los equipos planifican lo que pueden entregar dentro de un Sprint, y la velocity mide los story points por iteración.

En Agentic Development, la limitación principal es el context. La velocidad de entrega está determinada por la claridad con la que se especifica la intención y la exhaustividad con la que el Context Index cubre el dominio del problema. Cuando el context es excelente, los agent entregan a velocidad de máquina. Cuando el context es deficiente, los agent se estancan, alucinan o producen un trabajo que falla la validación.

Este cambio modifica aquello por lo que los equipos optimizan. En lugar de eliminar blockers del calendario de los developers, los líderes se centran en mejorar la calidad de las especificaciones, enriquecer el Context Index y reducir la ambigüedad en las definiciones de las tareas.

### Gobernanza continua y control de calidad

Agile típicamente se basa en puertas de calidad post-implementación: code review, QA testing, staging environments. Estas ocurren después de que un developer ha escrito el código.

Agentic development traslada el control de calidad antes y durante la ejecución:

- **Evaluaciones automatizadas previas al código** verifican que la especificación en sí esté bien formada, completa y libre de contradicciones antes de que cualquier agent toque el codebase.
- **Evaluación en curso** se ejecuta continuamente durante la ejecución del agent, detectando problemas a medida que surgen en lugar de después del hecho.
- **Progresión automática de puertas** reemplaza la revisión manual para tareas estándar, con revisión humana reservada para cambios de alto riesgo.

El resultado es un modelo de calidad que escala con el throughput del agent en lugar de ser un cuello de botella por la disponibilidad del revisor humano.

### Nuevo rol del humano

Agile posiciona a los developers como artesanos que escriben código, participan en ceremonias y toman decisiones de implementación. Agentic development traslada el rol humano de ejecutor a director:

- **Arquitectos de contexto** definen qué debe suceder y por qué, en lugar de implementarlo ellos mismos.
- **Operadores de agent** supervisan la ejecución e intervienen en las excepciones, en lugar de manejar cada tarea personalmente.
- **Revisores técnicos** validan la adecuación arquitectónica y la seguridad, en lugar de revisar cada línea de código.

Esto no significa que se requiera menos habilidad. Dirigir a los agent de manera efectiva exige una profunda comprensión técnica: es necesario saber qué aspecto tiene lo bueno para especificarlo con precisión y validarlo de manera eficiente.

### Infraestructura efímera

Los equipos Agile típicamente comparten entornos de desarrollo, staging servers y pipelines de CI/CD. Los equipos Agentic aprovisionan bancos de trabajo aislados y desechables para cada tarea. Esto transforma la infraestructura de un recurso compartido y persistente a uno bajo demanda y efímero.

La implicación: la infraestructura se convierte en un costo por tarea en lugar de un costo fijo del equipo, y la configuración del entorno pasa a ser parte de la especificación en lugar de una preocupación separada de DevOps.

## Tabla comparativa

| Dimensión | Cascada | Agile | Agentic |
|-----------|-----------|-------|---------|
| **Limitación principal** | Alcance (requisitos fijos) | Tiempo (límites de Sprint) | Contexto (claridad de la especificación) |
| **Unidad de trabajo** | Documento de requisitos | User story | Especificación en vivo |
| **Ciclo de ejecución** | Fases secuenciales | Sprints con límite de tiempo (1-4 semanas) | Bucle continuo de especificación a deploy |
| **Artefacto principal** | Documento de especificación | Incremento de software funcional | Código validado + Context Index enriquecido |
| **Control de calidad** | Revisiones de puerta de fase | Revisión de Sprint + retrospectiva | Eval Harness automatizado + puertas HITL |
| **Rol del humano** | Autor y ejecutor | Artesano y colaborador | Director y validador |
| **Infraestructura** | Entornos compartidos y de larga duración | Pipelines de CI/CD compartidos | Bancos de trabajo efímeros por tarea |

## De CI/CD al Bucle de Desarrollo Continuo

El Continuous Development Loop (CDL) extiende CI/CD para cubrir todo el ciclo de vida agentic. Mientras que CI/CD automatiza desde el code commit hasta el deploy, CDL automatiza desde la creación de la especificación hasta el deploy y viceversa.

### Mapeo de fases

| Fase CDL | Equivalente CI/CD | Mejora clave |
|-----------|-----------------|-----------------|
| Inyección de especificaciones | Creación de branch / inicio de ticket | La especificación estructurada y legible por máquina reemplaza el ticket informal |
| Ensamblaje de contexto | Incorporación del developer a la tarea | Recuperación automatizada del Context Index |
| Ejecución autónoma | Escritura de código | Implementación impulsada por agent en un banco de trabajo aislado |
| Eval Harness | Suite de pruebas CI | Verificaciones de comportamiento, seguridad y arquitectura más allá de las pruebas unitarias |
| Revisión de puerta | Revisión de pull request | Puertas automatizadas para tareas estándar; HITL para alto riesgo |
| Actualización de contexto | Actualización de documentación | Enriquecimiento automático del Context Index a partir de los resultados de ejecución |
| Deployment | Pipeline de CD | Infraestructura de deploy estándar, sin cambios |

### Mejoras arquitectónicas clave

Tres capacidades distinguen al CDL del CI/CD tradicional:

1.  **La seguridad como un hilo continuo.** En lugar de una etapa de revisión de seguridad separada, las verificaciones de seguridad se ejecutan en cada fase, desde la validación de la especificación hasta la ejecución y el deploy. Esto detecta los problemas de seguridad en el punto de introducción, en lugar de después del hecho.

2.  **El Eval Harness reemplaza a CI como la principal puerta de calidad.** El CI tradicional ejecuta pruebas unitarias y linters. El Eval Harness ejecuta verificación de comportamiento, verificaciones de conformidad arquitectónica, líneas de base de rendimiento y escaneos de seguridad como una suite de evaluación unificada.

3.  **Bucle de retroalimentación centrado en el conocimiento.** Cada ciclo de CDL produce datos estructurados que retroalimentan al Context Index. Las evaluaciones fallidas se convierten en patrones documentados. Las intervenciones del operator se convierten en actualizaciones de context. Las ejecuciones exitosas refuerzan los enfoques probados. El sistema aprende continuamente.

## El surgimiento de los agent de codificación

La transición de Agile a Agentic es posible gracias a una rápida evolución en las herramientas de codificación de AI, que pasan de asistentes pasivos a agent autónomos.

### De los copilotos a los equipos autónomos

La trayectoria de las herramientas de codificación de AI sigue una progresión clara:

1.  **Code completion** (2021-2023) —Herramientas de sugerencia de código en línea. GitHub Copilot, TabNine y otras sugieren la siguiente línea o bloque de código. El humano mantiene el control total.

2.  **Asistentes interactivos** (2023-2024) —Interfaces basadas en chat como ChatGPT, Claude y el modo compositor de Cursor. Los developers describen lo que quieren en lenguaje natural e iteran sobre el output. Sigue siendo impulsado por humanos, pero con mayor leverage.

3.  **Agent de codificación** (2024-2025) —Herramientas como Claude Code, el modo agent de GitHub Copilot y los agent en segundo plano de Cursor que pueden planificar implementaciones de varios pasos, ejecutarlas en múltiples archivos, ejecutar pruebas e iterar sobre fallos. El humano define la tarea; el agent maneja la ejecución.

4.  **Equipos autónomos** (2025-presente) —Sistemas multi-agent donde los agent especializados manejan diferentes aspectos del desarrollo (planificación, implementación, pruebas, revisión) con supervisión humana en puntos de control estratégicos. Aquí es donde [[vibe-coding]] evoluciona de una actividad en solitario a un workflow orquestado.

### Capacidades de los agent generalistas

Los agent de codificación modernos son generalistas. Un solo agent puede:

-   Leer y comprender codebases completos
-   Planificar implementaciones de varios pasos antes de escribir código
-   Escribir código en múltiples archivos y lenguajes
-   Ejecutar e interpretar suites de pruebas
-   Debuggear fallos e iterar sobre soluciones
-   Interactuar con herramientas de desarrollo (Git, package managers, build systems, APIs)
-   Seguir convenciones específicas del proyecto definidas en archivos de context

Esta capacidad generalista es lo que hace viable el modelo de [[software-factory]]. No necesitas una herramienta diferente para cada tarea: un único agent bien contextualizado puede manejar todo el rango de trabajo de desarrollo dentro de su límite de competencia.

### Comparación de plataformas

El panorama de los agent de codificación está evolucionando rápidamente. A principios de 2026, las principales plataformas incluyen:

| Plataforma | Enfoque | Fortalezas |
|----------|----------|-----------|
| **Claude Code** (Anthropic) | Agent nativo de CLI con uso profundo de herramientas | Cambios en múltiples archivos, refactoring complejo, integración con terminal |
| **GitHub Copilot / Codex** (Microsoft) | IDE integrado + agent en la nube | Integración con el ecosistema GitHub, ejecución asíncrona en segundo plano |
| **Gemini Code Assist / Jules** (Google) | Plugin de IDE + agent autónomo | Soporte multi-modelo, integración con Google Cloud |
| **Open Source** (Aider, OpenCode, SWE-agent) | Agent de CLI impulsados por la comunidad | Transparencia, personalización, flexibilidad de modelo |

La elección de la plataforma importa menos que las prácticas que la rodean. Un equipo con excelentes especificaciones, un Context Index rico y una gobernanza disciplinada de [[human-in-the-loop]] superará a un equipo con un agent "mejor" pero con un context deficiente y workflows ad-hoc.

## Haciendo la transición

Los equipos que pasan de Agile a Agentic no necesitan cambiar todo a la vez. Un camino práctico:

1.  **Empiece con las especificaciones.** Elija un equipo o un proyecto y requiera especificaciones legibles por máquina en lugar de (o además de) las user stories. Mida si mejora la calidad del output del agent.

2.  **Introduzca el Eval Harness.** Añada verificaciones de comportamiento automatizadas junto con el CI existente. Ejecútelas tanto en código humano como en código de agent para establecer líneas de base.

3.  **Ponga a prueba la ejecución autónoma.** Dirija tareas bien definidas y de bajo riesgo a los agent. Mantenga la revisión humana en el loop inicialmente para generar confianza.

4.  **Construya el Context Index.** Capture las lecciones aprendidas de las ejecuciones del agent, las intervenciones del operator y la retroalimentación de la revisión en un formato estructurado y searchable.

5.  **Expanda el límite.** A medida que aumenta la confianza, dirija más tipos de tareas a los agent y desplace el esfuerzo humano hacia la calidad de la especificación, la arquitectura y las decisiones estratégicas.

El objetivo no es reemplazar las ceremonias de Agile de la noche a la mañana. Es evolucionarlas incrementalmente a medida que los agent asumen más carga de trabajo de ejecución, liberando a los humanos para que se centren en el trabajo que solo los humanos pueden hacer.

## Próximos pasos

Con el framework definido, el próximo capítulo cubre el [Team Model](/en/handbook/team-model), cómo estructurar roles, habilidades y responsabilidades en una organización de desarrollo agentic.
