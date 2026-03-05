---
title: Sistemas Multiagente
description: >-
  Cómo múltiples agentes de AI se coordinan a través de patrones de orquestación
  y estándares de interoperabilidad como MCP
authors:
  - dpavancini
lastUpdated: '2026-02-23'
---

## Descripción general

Un [[multi-agent-systems|sistema multi-agente]] (MAS) distribuye la ejecución del flujo de trabajo entre múltiples agentes de AI especializados que se coordinan para resolver problemas que ningún agente individual podría manejar de manera eficiente por sí solo. En lugar de construir un agente monolítico que haga todo, las arquitecturas multi-agente asignan roles distintos a agentes enfocados (un agente de planificación, un agente de codificación, un agente de pruebas) y definen cómo se comunican y entregan el trabajo.

## ¿Por qué múltiples agentes?

Un solo agente con acceso a docenas de herramientas y un prompt masivo del sistema se vuelve cada vez más poco confiable a medida que crece la complejidad. Su context window se llena, sus instrucciones entran en conflicto y su toma de decisiones se degrada.

Los sistemas multi-agente abordan esto aplicando el mismo principio que hace que los equipos humanos sean efectivos: la especialización. Cada agente tiene un rol limitado, un conjunto de herramientas enfocado y claras instrucciones para su dominio.

Considere un flujo de trabajo de desarrollo de software:

- **Agente de Planificación** — Analiza requisitos, divide el trabajo en tareas, define criterios de aceptación
- **Agente de Ingeniería de Software** — Escribe código de implementación, realiza refactoring, maneja migraciones
- **Agente de QA** — Genera pruebas, ejecuta suites de pruebas, valida el comportamiento según los criterios de aceptación
- **Agente de Revisión** — Verifica la calidad del código, la seguridad, la adherencia a las convenciones

Cada agente se destaca en su rol porque solo lleva el contexto y las herramientas relevantes para ese rol. El sistema en su conjunto maneja flujos de trabajo complejos que abrumarían a cualquier agente individual.

## Patrones de Orquestación

La [[multi-agent-orchestration|Orquestación]] define cómo se coordinan los agentes: quién decide qué sucede a continuación, cómo fluye la información entre los agentes y cómo se resuelven los conflictos. La elección del patrón de orquestación tiene un impacto directo en la confiabilidad, flexibilidad y complejidad del sistema.

### Orquestación Centralizada

Un solo agente conductor (a veces llamado maestro o supervisor) gestiona todo el flujo de trabajo. Recibe la tarea inicial, decide qué agentes especialistas invocar, les pasa el contexto y ensambla el resultado final.

**Cómo funciona:**

1. El conductor recibe una tarea del usuario
2. Divide la tarea en subtareas
3. Envía cada subtarea al agente especialista apropiado
4. Recopila los resultados y decide el siguiente paso
5. Ensambla la salida final

**Fortalezas:**

- Fácil de razonar: un agente tiene la imagen completa
- Fácil de implementar el registro y la observabilidad
- Clara rendición de cuentas por las decisiones

**Debilidades:**

- El conductor es un punto único de falla
- Puede convertirse en un cuello de botella a medida que crece el número de agentes especialistas
- La context window del conductor debe acomodar el estado completo del flujo de trabajo

### Orquestación Descentralizada

Los agentes se comunican directamente entre sí de manera peer-to-peer. No hay un coordinador central: los agentes pasan el trabajo al siguiente agente de la cadena basándose en su propia evaluación de lo que debe suceder a continuación.

**Cómo funciona:**

1. Un agente completa su tarea
2. Evalúa el resultado y decide qué agente debe manejar el siguiente paso
3. Pasa el contexto directamente a ese agente
4. El proceso continúa hasta que el flujo de trabajo esté completo

**Fortalezas:**

- No hay un punto único de falla
- Escala naturalmente a medida que se agregan agentes
- Cada agente solo necesita contexto para su tarea inmediata

**Debilidades:**

- Más difícil monitorear el estado general del flujo de trabajo
- Potencial de bucles o bloqueos si los agentes no están de acuerdo
- La depuración requiere el rastreo a través de múltiples interacciones de agentes

### Orquestación Híbrida

Combina enfoques centralizados y descentralizados. Un conductor gestiona el flujo de trabajo de alto nivel, mientras permite que los agentes especialistas se coordinen directamente entre sí para las subtareas.

Este es el patrón más común en sistemas en producción. El conductor se encarga de la descomposición de tareas y el ensamblaje final, mientras que los agentes especialistas colaboran en los detalles de implementación sin enrutar cada mensaje a través del conductor.

### Patrón Broker/Mediador

Un agente intermediario actúa como un enrutador de mensajes sin comprender el flujo de trabajo en sí. Recibe solicitudes, las hace coincidir con el agente más apropiado en función de sus capacidades y enruta la respuesta de vuelta.

Este patrón es útil cuando se tiene un gran grupo de agentes intercambiables y se desea equilibrar la carga o enrutar en función de la disponibilidad. El intermediario no planifica ni razona sobre el flujo de trabajo, simplemente conecta a los solicitantes con los proveedores.

### Orquestación Basada en Flujos de Trabajo

La lógica de orquestación se define externamente como un grafo de flujo de trabajo (un DAG o máquina de estados) en lugar de estar incrustada en un agente. Los agentes se invocan en nodos específicos del grafo, y las transiciones entre nodos son determinadas por el motor de flujo de trabajo.

**Fortalezas:**

- La lógica del flujo de trabajo es explícita y auditable
- Fácil de modificar sin cambiar el código del agente
- Adecuado para entornos regulados que requieren rutas determinísticas

**Debilidades:**

- Menos adaptativo: los agentes no pueden alterar dinámicamente el flujo de trabajo
- Requiere un diseño de flujo de trabajo inicial
- Puede no manejar situaciones inesperadas con agilidad

## Model Context Protocol (MCP)

A medida que crecen los sistemas de agentes, necesitan interactuar con un número creciente de herramientas, fuentes de datos y servicios externos. El [[mcp|Model Context Protocol (MCP)]] es un estándar abierto introducido por Anthropic para resolver este desafío de interoperabilidad.

### El problema que resuelve MCP

Sin un protocolo estándar, cada integración de herramienta de AI se construye a medida. Si tiene 5 modelos de AI y 10 herramientas externas, necesita 50 integraciones personalizadas. Agregar un nuevo modelo significa construir 10 más. Esto no escala.

MCP estandariza cómo los modelos de AI acceden a herramientas y datos externos, de manera muy similar a cómo USB estandarizó la conexión de computadoras a periféricos. Con MCP, cualquier modelo compatible puede conectarse a cualquier herramienta compatible a través de un único protocolo.

### Cómo funciona MCP

La [[model-context-protocol|arquitectura de MCP]] sigue un modelo cliente-servidor:

- **Cliente MCP** — Integrado en la aplicación de AI (por ejemplo, Claude Code, Cursor). Descubre los servidores disponibles, negocia las capacidades y enruta las [[tool-calling|llamadas a herramientas]].
- **Servidor MCP** — Actúa como un puente entre el modelo de AI y un sistema externo. Cada servidor expone un conjunto definido de herramientas y recursos. Por ejemplo, un servidor MCP de GitHub podría exponer herramientas para crear pull requests, leer issues y buscar repositorios.
- **Capa de transporte** — MCP admite múltiples mecanismos de transporte (stdio, HTTP con SSE) para que los servidores puedan ejecutarse local o remotamente.

Un flujo de interacción típico:

1. La aplicación de AI se conecta a uno o más servidores MCP al inicio
2. El cliente MCP consulta a cada servidor para conocer sus herramientas y recursos disponibles
3. Cuando el agente necesita usar una herramienta, el cliente envía una solicitud estructurada al servidor apropiado
4. El servidor ejecuta la operación y devuelve el resultado
5. El cliente pasa el resultado de vuelta al agente

### Por qué MCP es importante

MCP aporta varios beneficios prácticos a los sistemas multi-agente:

- **Escribir una vez, usar en todas partes** — Una integración de herramienta construida como un servidor MCP funciona con cualquier modelo de AI compatible con MCP
- **Ecosistema comunitario** — El estándar abierto permite un ecosistema creciente de servidores MCP compartidos para servicios comunes
- **Límites de seguridad** — Los servidores MCP pueden aplicar su propia autenticación, autorización y limitación de tasa
- **Local y remoto** — Los servidores pueden ejecutarse en la máquina del desarrollador para un acceso de baja latencia o de forma remota para herramientas de equipo compartidas

## Principios de Interoperabilidad y DRY

Los sistemas multi-agente se benefician enormemente de los mismos principios de ingeniería que hacen que el software tradicional sea mantenible.

### No te Repitas (DRY)

Cuando varios agentes necesitan la misma capacidad (por ejemplo, leer una base de datos o llamar a una API), esa capacidad debe existir en un solo lugar. En la práctica, esto significa:

- **Bibliotecas de herramientas compartidas** — Construir herramientas comunes una vez y ponerlas a disposición de todos los agentes a través de una interfaz estándar (como MCP)
- **Configuración centralizada** — Almacenar configuraciones compartidas (endpoints de API, credenciales, parámetros de modelo) en una única ubicación
- **Componentes de prompt reutilizables** — Extraer patrones de instrucción comunes en plantillas compartidas que varios agentes incluyen

### Estandarizar la Comunicación

Los agentes que se comunican a través de protocolos bien definidos son más fáciles de desarrollar, probar y reemplazar. Cuando el Agente A envía una solicitud de revisión de código al Agente B, el formato del mensaje debe ser consistente y documentado. Esto permite intercambiar el Agente B por una implementación diferente sin cambiar el Agente A.

### Diseñar para la Reemplazabilidad

Construya los agentes de modo que cualquier agente individual pueda ser reemplazado sin rediseñar el sistema. Esto significa:

- Interfaces claras entre agentes
- Sin dependencias ocultas o estado mutable compartido
- Contratos de entrada/salida bien documentados

Cuando aparece un modelo mejor o un agente especialista necesita ser reescrito, el cambio debe localizarse solo en ese agente.

## Qué viene después

Con una comprensión de los agentes individuales y cómo se coordinan en sistemas multi-agente, el próximo capítulo cambia el enfoque a los marcos y metodologías prácticas para poner en práctica estos conceptos en equipos de desarrollo reales.
