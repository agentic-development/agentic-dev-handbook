---
title: Pilares Fundamentales
description: >-
  Los cuatro pilares arquitectónicos que permiten una entrega de software
  asistida por AI segura y escalable
authors:
  - dpavancini
lastUpdated: '2026-02-24'
---

## Resumen

El Marco de Desarrollo Agentic se basa en cuatro pilares arquitectónicos que trabajan juntos para permitir la entrega de software asistida por AI de forma segura y escalable. Cada pilar aborda una dimensión diferente del desafío: qué saben los agent, dónde se ejecutan, cómo se gobiernan y cómo se enruta el trabajo.

## Pilar 1: Arquitectura Context-First

[[context-engineering|Context Engineering]] es la base de todo lo demás. En el desarrollo agentic, el context es código — es la entrada principal que determina la calidad, corrección y velocidad de cada ejecución del agent.

### La Claridad del Context como Restricción Primaria

El desarrollo tradicional está limitado por el tiempo del desarrollador. El desarrollo agentic está limitado por la claridad del context. Cuando los agent tienen un context preciso, completo y bien estructurado, se ejecutan de forma fiable y rápida. Cuando el context es vago o incompleto, producen alucinaciones, pasan por alto casos extremos o entran en bucles improductivos.

Esto significa que la [[context-window]] no es solo una limitación técnica de los modelos de lenguaje — es una restricción arquitectónica que da forma a cómo se descompone el trabajo, se estructuran las especificaciones y se organiza el conocimiento.

### El Context Index

El Context Index es la base de conocimiento estructurada de la que los agent se nutren durante la ejecución. Incluye:

- **Live Specs** — Especificaciones legibles por máquina para los elementos de trabajo actuales
- **System Constitution** — Principios arquitectónicos, estándares de codificación, políticas de seguridad
- **Codebase context** — Definiciones de tipo, esquemas de API, suites de prueba, documentación
- **Historical context** — Decisiones pasadas, bloqueos resueltos, lecciones aprendidas

El Context Index sirve como base de conocimiento persistente del agent. Cuanto más rico y estructurado sea, con mayor autonomía podrá operar el agent. Para un tratamiento completo del Context Index, los patrones de recuperación y las prácticas de higiene, consulte [Context Management](/en/handbook/infrastructure/context-management).

Las organizaciones que adoptan patrones [[rag|RAG]] para mostrar context relevante dinámicamente pueden reducir aún más la carga del Context Architect y aumentar la proporción de tareas que los agent manejan de forma autónoma.

## Pilar 2: Infraestructura Efímera

Cada ejecución de agent ocurre dentro de un Ephemeral Workbench — un entorno aislado, seguro y desechable que contiene todo lo que el agent necesita y nada que no necesite.

### Workbenches como MicroVMs Aislados

Un Ephemeral Workbench es un entorno de ejecución en un entorno tipo *sandbox* aprovisionado bajo demanda y destruido después de su uso. Cada *workbench*:

- Contiene una copia limpia de la *codebase* y las dependencias relevantes
- Tiene acceso solo al context y a las herramientas especificadas en la Live Spec de la tarea
- No puede modificar el estado compartido, los sistemas de producción ni los *workbenches* de otros agent
- Produce artefactos (cambios de código, resultados de pruebas, logs) que se extraen antes del desmantelamiento

Este modelo elimina toda una clase de riesgos: los agent no pueden corromper accidentalmente entornos compartidos, filtrar secretos entre tareas o crear efectos secundarios persistentes.

### Seguridad por Diseño

La infraestructura efímera convierte la seguridad en una propiedad estructural en lugar de una superposición de políticas:

- **Blast radius containment** — Si un agent hace algo mal, el daño se limita a un entorno desechable
- **No persistent access** — Los agent nunca poseen credenciales o sesiones de larga duración
- **Auditable by default** — Cada ejecución de *workbench* produce un log completo y reproducible
- **[[prompt-injection]] resistance** — Los entornos aislados limitan lo que un atacante puede lograr incluso si compromete la entrada de un agent.

## Pilar 3: Gate-Based Governance

El desarrollo agentic reemplaza la revisión informal de código con un sistema estructurado de *checkpoints* automatizados y con intervención humana (*human-in-the-loop*).

### Gates Automatizadas Continuas

Estas *gates* se ejecutan en cada ejecución del agent sin intervención humana:

- **Security scans** — Análisis estático, comprobaciones de vulnerabilidad de dependencias, detección de secretos
- **Eval Harness** — Los criterios de aceptación definidos en la *spec*, ejecutados como pruebas automatizadas
- **Architectural conformance** — Comprobaciones de que los cambios siguen patrones establecidos y no introducen dependencias prohibidas
- **Performance baselines** — *Benchmarks* automatizados que detectan regresiones

El Eval Harness es la pieza central. Transforma los criterios de aceptación de un documento que leen los humanos en comprobaciones ejecutables que los agent deben pasar. Este es el mecanismo que hace que la ejecución autónoma sea confiable.

### Gates Obligatorias HITL

Algunas decisiones son demasiado importantes para una automatización completa. Las *gates* obligatorias con intervención humana (*human-in-the-loop*) incluyen:

- **Security-critical changes** — Flujos de autenticación, lógica de autorización, cifrado de datos
- **Architectural decisions** — Nuevos límites de servicio, cambios de esquema de base de datos, contratos de API
- **High-blast-radius deployments** — Cambios que afectan a todos los usuarios o a la infraestructura crítica
- **Novel patterns** — Implementaciones por primera vez de enfoques no cubiertos por las *specs* existentes

La clave es que el riesgo de *hallucination* no es uniforme en todas las tareas. La Gate-Based Governance asigna la atención humana donde más importa, en lugar de distribuirla de forma diluida en todo.

## Pilar 4: Hybrid Engineering

Hybrid Engineering es la práctica de enrutar cada tarea al recurso más eficiente — ya sea un agent autónomo, un agent asistido o un ingeniero humano.

### Auto-Agents para Tareas Estándar

Las tareas bien definidas, con patrones existentes y de bajo riesgo son ideales para la ejecución totalmente autónoma. Los ejemplos incluyen:

- Implementar *endpoints* CRUD a partir de *API specs*
- Escribir pruebas unitarias para funciones existentes
- Aplicar patrones de *refactoring* en una *codebase*
- Generar documentación a partir de código y definiciones de tipo

### Ingenieros Humanos para el Trabajo Complejo

Las tareas que implican alta ambigüedad, arquitectura novedosa o decisiones importantes quedan en manos de ingenieros humanos. Los ejemplos incluyen:

- Diseñar nuevas arquitecturas de sistema
- Resolver requisitos conflictivos
- Tomar decisiones de compromiso de seguridad
- Manejar dominios de problemas novedosos sin patrones existentes

### La Decisión de Enrutamiento

El Sistema de Orquestación enruta las tareas basándose en una combinación de factores:

- **Spec completeness** — ¿Cuán bien definidos están los criterios de aceptación?
- **Pattern coverage** — ¿El Context Index contiene precedentes relevantes?
- **Blast radius** — ¿Cuál es el impacto potencial de una implementación incorrecta?
- **Novelty** — ¿Es esta una variación de un patrón conocido o algo completamente nuevo?

Con el tiempo, a medida que el Context Index crece y las *specs* se vuelven más precisas, la proporción de tareas de *auto-agent* aumenta. Así es como escalan los equipos agentic: no contratando a más ingenieros, sino expandiendo el límite de lo que los agent pueden manejar de forma autónoma.

## Próximos Pasos

Estos cuatro pilares proporcionan la base estructural. La siguiente página cubre [Spec-Driven Development](/en/handbook/framework/spec-driven-development), la práctica que reemplaza las historias de usuario informales con especificaciones legibles por máquina — la entrada principal que hace que estos pilares sean operativos.
