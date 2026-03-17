---
title: Glosario del Framework
description: >-
  Definiciones de referencia rápida para los términos clave utilizados a lo
  largo del manual.
authors:
  - dpavancini
lastUpdated: '2026-02-25'
---

Este glosario define los términos clave introducidos en el manual. Cada entrada incluye una breve definición, un enlace a la entrada completa del glosario en el sitio y una referencia a la página del manual donde el concepto se explica en detalle.

## Términos

**[[agentops-dashboard|AgentOps Dashboard]]** — La interfaz de monitoreo en tiempo real que rastrea el estado de ejecución de los agent, la salud del pipeline y los costos de cómputo en todo un equipo. El Flow Manager lo revisa durante el Daily Flow Sync.
Handbook: [Ceremonies and Routines](/en/handbook/operations/ceremonies)

**[[blocker-flag|Blocker Flag]]** — Una señal levantada por un agent cuando encuentra una ambigüedad o una restricción que no puede resolver, lo que dispara una escalada a un operador humano que ejecuta una Agent Recovery.
Handbook: [The Orchestration Layer](/en/handbook/framework/orchestration-layer)

**[[context-engineering|Context Engineering]]** — La práctica de diseñar, organizar y gestionar la información que fluye hacia el context window de un LLM. Adopta una visión a nivel de sistemas sobre cómo se ensamblan y priorizan los system prompts, documentos, historial y herramientas.
Handbook: [Context Management](/en/handbook/infrastructure/context-management)

**[[context-index|Context Index]]** — La base de conocimiento estructurada de la que los agent se nutren durante la ejecución. Contiene Live Specs, reglas arquitectónicas, Golden Samples, glosarios de dominio y contexto histórico, organizado para el consumo por máquina.
Handbook: [Context Management](/en/handbook/infrastructure/context-management)

**[[context-packet|Context Packet]]** — Una colección empaquetada de especificaciones, reglas arquitectónicas, Golden Samples y contexto de dominio ensamblada para una tarea específica. Creada durante los weekly Specification Engineering Blocks y consumida durante la ejecución diaria de agent.
Handbook: [Ceremonies and Routines](/en/handbook/operations/ceremonies)

**[[continuous-development-loop|Continuous Development Loop]]** — La evolución agentic de CI/CD que automatiza todo el ciclo de vida, desde la especificación hasta la producción. Extiende el pipeline tradicional con inyección de especificaciones, ensamblaje de contexto automatizado, compuertas de Eval Harness y bucles de retroalimentación de conocimiento.
Handbook: [From Agile to Agentic](/en/handbook/framework/from-agile-to-agentic)

**[[human-owned-core|Human-Owned Core]]** — Código que es demasiado sensible a la arquitectura o crítico para la seguridad para la ejecución de agent. Escrito exclusivamente por Agent Operators humanos, incluye rutas de autenticación, algoritmos centrales y abstracciones fundamentales.
Handbook: [The Hybrid Squad](/en/handbook/team-model/hybrid-squad)

**[[eval-harness|Evaluation Harness]]** — La suite de pruebas automatizada que valida cada salida de agent antes de que llegue a un revisor humano. Combina pruebas funcionales, escaneos de seguridad, verificaciones de conformidad arquitectónica y evaluaciones LLM-as-a-Judge.
Handbook: [Evaluation Harness](/en/handbook/infrastructure/evaluation-harness)

**[[golden-samples|Golden Samples]]** — Implementaciones de referencia curadas que los agent utilizan como plantillas para código nuevo. Un golden sample bien elegido enseña a un agent más que páginas de instrucciones escritas al demostrar patrones esperados a través de ejemplos concretos.
Handbook: [The Hybrid Squad](/en/handbook/team-model/hybrid-squad)

**[[human-in-the-loop|Human-in-the-Loop]]** — Un principio de diseño donde se requiere el juicio humano en puntos de decisión críticos antes de que un agent proceda con acciones con consecuencias. Permite una autonomía gradual a través de modelos de permisos escalonados.
Handbook: [The Orchestration Layer](/en/handbook/framework/orchestration-layer)

**[[live-spec|Live Spec]]** — Un contrato determinista, legible por máquina, que define qué construir, por qué es importante y cómo verificar que funciona. Contiene un Behavioral Contract, System Constitution y Actionable Task Map. Evoluciona junto con la base de código bajo control de versiones.
Handbook: [Spec-Driven Development](/en/handbook/framework/spec-driven-development)

**[[agent-recovery|Agent Recovery]]** — El flujo de trabajo de intervención donde un Agent Operator diagnostica un agent atascado, inyecta contexto faltante y reanuda la ejecución. Sigue un proceso de tres pasos: Diagnose, Inject, Resume.
Handbook: [Ceremonies and Routines](/en/handbook/operations/ceremonies)

**[[spec-driven-development|Spec-Driven Development]]** — La práctica de reemplazar las historias de usuario informales con especificaciones legibles por máquina que sirven como contratos deterministas entre humanos y agent. Hace que los pilares centrales sean operativos al proporcionar entradas precisas para el contexto, la gobernanza y el enrutamiento.
Handbook: [Spec-Driven Development](/en/handbook/framework/spec-driven-development)

**[[token-budget|Token Budget]]** — El gasto máximo de cómputo autorizado por período de tiempo, que funciona como una restricción estricta que previene bucles de agent descontrolados. Aplicado en la Orchestration Layer y rastreado en el AgentOps Dashboard.
Handbook: [Ceremonies and Routines](/en/handbook/operations/ceremonies)
