---
title: Principios del Desarrollo Agéntico
description: Diez principios que definen el desarrollo agéntico como una disciplina
authors:
  - dpavancini
lastUpdated: '2026-02-24'
---

## Resumen

Existe una confusión generalizada en el mercado entre "entregar código" y "desarrollar software". Los LLM pueden generar código sintácticamente correcto a escala; ese problema está resuelto. Pero desarrollar software —entender los requisitos, gestionar la complejidad, garantizar la fiabilidad, gobernar el cambio— sigue siendo un desafío profundamente humano.

[[agentic-engineering|El desarrollo con agentes]] aborda este desafío a través de una colaboración estructurada entre humanos y agentes. Los siguientes diez principios definen lo que entendemos por desarrollo con agentes y guían cada capítulo de este manual.

## De un Vistazo

1.  **Los agentes construyen el código; los humanos dirigen el sistema.**
2.  **El context es el cuello de botella, no la velocidad de escritura.**
3.  **El código se vuelve cada vez más efímero; las especificaciones perduran.**
4.  **Las mejores prácticas de ingeniería de software se expanden, no desaparecen.**
5.  **Las especificaciones deben ser estructuradas y legibles por máquina.**
6.  **Todo el SDLC se rediseña para la colaboración entre humanos y agentes.**
7.  **Nuevos caminos de especialización reemplazan a los antiguos.**
8.  **Los equipos incluyen explícitamente tanto a agentes como a humanos.**
9.  **La gobernanza empresarial se integra, no se añade de forma externa.**
10. **El vibe coding no es desarrollo con agentes.**

## Lo que Creemos

### 1. Los Agentes Construyen el Código; los Humanos Dirigen el Sistema

El desarrollador humano pasa de escribir código a dirigir, revisar y orquestar a los agentes que lo producen.

### 2. El Context Es el Cuello de Botella

En la era de los agentes, la restricción ya no es la velocidad de escritura, es el [[context-engineering|context]]. Construir un backlog bien estructurado que los agentes puedan consumir es el mayor desafío al que se enfrentarán los equipos. El capítulo de Framework explora esto a través de la Arquitectura Context-First.

### 3. El Código se Vuelve Cada Vez Más Efímero

A medida que las capacidades de los agentes maduran, el código se vuelve cada vez más efímero — recompilable a partir de especificaciones con mínima intervención humana. La especificación, no el código, se convierte en el artefacto duradero. El capítulo de Infraestructura cubre esto a través del patrón Ephemeral Workbench.

### 4. Las Mejores Prácticas se Expandan, No Desaparecen

El desarrollo con agentes no reemplaza las mejores prácticas de ingeniería de software, las expande. Las pruebas, la revisión de código y la arquitectura importan más, no menos, cuando los agentes están produciendo código a escala. El capítulo de Operaciones detalla cómo las prácticas tradicionales evolucionan a través de ceremonias con agentes.

## Lo que Hacemos

### 5. Las Especificaciones Deben Ser Estructuradas y Legibles por Máquina

El desarrollo con agentes espera especificaciones bien diseñadas y legibles por máquina. Los agentes necesitan entradas estructuradas y no ambiguas para producir salidas fiables. El pilar de Desarrollo Dirigido por Especificaciones del capítulo de Framework cubre esto en profundidad.

### 6. Todo el SDLC se Rediseña

El desarrollo con agentes replantea el SDLC completo —desde el alcance del proyecto y la planificación del trabajo hasta la ejecución y el deploy. Cada fase se rediseña para la colaboración entre humanos y agentes. Consulte la comparación del capítulo de Framework de los enfoques Waterfall, Agile y con agentes.

### 7. Nuevos Caminos de Especialización Reemplazan a los Antiguos

La era de los agentes afectará profundamente cómo los ingenieros construyen experiencia. Surgen nuevas habilidades — [[context-engineering]], escritura de especificaciones, orquestación de agentes — mientras que la fluidez en la codificación tradicional se convierte en una habilidad básica. El capítulo de Modelo de Equipo define seis nuevos roles construidos en torno a estas habilidades.

## Cómo nos Estructuramos

### 8. Los Equipos Incluyen Tanto a Agentes como a Humanos

El software será desarrollado por equipos que incluyan explícitamente tanto a agentes como a humanos. Esto no es una metáfora; es un principio de diseño que afecta a las herramientas, los flujos de trabajo y la estructura del equipo. El capítulo de Modelo de Equipo presenta al Hybrid Squad como la unidad fundacional.

### 9. La Gobernanza Empresarial se Integra

El desarrollo de software empresarial requiere responsabilidad y gobernanza por diseño, incluso cuando los agentes producen el código. La aprobación humana —explícita o implícita— es necesaria en cada coyuntura crítica. El pilar de Gobernanza Basada en Puertas del capítulo de Framework y las ceremonias del capítulo de Operaciones lo concretan.

### 10. El Vibe Coding No Es Desarrollo con Agentes

[[vibe-coding]] es principalmente un método individual de codificación —rápido, exploratorio y no estructurado. El desarrollo con agentes es una disciplina de equipo con gobernanza, repetibilidad y responsabilidad incorporadas.

## Qué Sigue

Estos principios son la base; el resto del manual los concreta. Continúe con [¿Para Quién Es Esto?](/en/handbook/introduction/who-is-this-for) para ver cómo este manual sirve a diferentes roles, o salte directamente al capítulo de [Framework](/en/handbook/framework) para explorar los cinco pilares que ponen estos principios en práctica.
