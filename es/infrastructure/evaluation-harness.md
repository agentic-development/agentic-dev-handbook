---
title: El Arnés de Evaluación
description: >-
  Pruebas automatizadas, compuertas de calidad y mecanismos de gobernanza que
  validan el código generado por agentes
authors:
  - dpavancini
lastUpdated: '2026-02-23'
---

## Resumen

El Eval Harness es el conjunto de pruebas automatizado que se ejecuta continuamente durante la ejecución del agent, validando cada resultado antes de que llegue a un revisor humano. Combina pruebas funcionales, escaneos de seguridad, comprobaciones de conformidad arquitectónica y evaluaciones de LLM-as-a-Judge en una puerta de calidad unificada. Ningún código generado por el agent se presenta a un humano hasta que pasa el Eval Harness.

## Por qué existe el Eval Harness

En el desarrollo tradicional, la garantía de calidad ocurre después de la implementación: un desarrollador escribe código, luego las pruebas y los revisores lo comprueban. En el desarrollo con agents, esta secuencia se invierte. El Eval Harness define los criterios de aceptación antes de que el agent comience, ejecuta la validación continuamente durante la ejecución y controla el resultado antes de que entre en la cola de revisión.

Esta inversión cumple dos propósitos:

1.  **Hace que la ejecución autónoma sea confiable.** Los humanos no pueden revisar cada línea que un agent produce a la velocidad a la que los agents la producen. El Eval Harness actúa como un primer revisor automatizado que detecta la mayoría de los problemas, para que los humanos puedan centrar su atención en los casos que requieren juicio.
2.  **Crea un ciclo de retroalimentación.** Cuando el resultado de un agent falla la evaluación, el harness devuelve retroalimentación estructurada que el agent utiliza para corregir su trabajo e intentar de nuevo. Este ciclo continúa hasta que el resultado pasa o el presupuesto de ejecución se agota.

## Dos Tipos de Validación

No toda la validación es igual. El Eval Harness distingue entre dos tipos de comprobaciones fundamentalmente diferentes, cada una adecuada para diferentes aspectos de la calidad del código.

### Validación Determinista

La validación determinista produce resultados binarios de aprobación/rechazo basados en reglas estrictas e inequívocas. No hay área gris: la comprobación aprueba o no lo hace.

Ejemplos de comprobaciones deterministas:

-   **Imports prohibidos** —El código del agent importa un módulo prohibido por las reglas arquitectónicas del proyecto. La comprobación bloquea el pull request automáticamente.
-   **Seguridad de tipos** —El compilador de TypeScript informa errores de tipo. El código no compila.
-   **Escaneos de seguridad** —El análisis estático detecta un patrón de vulnerabilidad conocido, como inyección SQL o credenciales codificadas.
-   **Paso del conjunto de pruebas** —Todas las pruebas unitarias y de integración deben pasar. Una sola falla bloquea el resultado.
-   **Linting y formato** —El código debe ajustarse a las reglas de estilo del proyecto. El código no conforme es rechazado.
-   **Política de dependencias** —Las nuevas dependencias deben estar en la lista aprobada. Los paquetes no aprobados son marcados.

Las comprobaciones deterministas son rápidas, confiables y económicas. Deben cubrir toda regla que pueda expresarse como una condición binaria. Cuando una regla puede hacerse determinista, debe hacerse; no dejes decisiones deterministas a la evaluación probabilística.

### Evaluación Probabilística

La evaluación probabilística utiliza un LLM-as-a-Judge para evaluar aspectos no deterministas del resultado del agent que no pueden reducirse a reglas binarias. Estas evaluaciones producen puntuaciones, calificaciones o evaluaciones clasificadas en lugar de simples resultados de aprobación/rechazo.

Ejemplos de evaluaciones probabilísticas:

-   **Calificación de calidad del código** —Un modelo judge evalúa si el código generado sigue patrones idiomáticos, utiliza nombres de variables significativos y mantiene niveles de abstracción adecuados.
-   **Calidad de la documentación** —El judge evalúa si la documentación generada es lo suficientemente clara, precisa y completa para la audiencia objetivo.
-   **Alineación arquitectónica** —Para casos que van más allá de lo que el análisis estático puede comprobar, el judge evalúa si la implementación se alinea con la intención arquitectónica del proyecto.
-   **Tono y estilo** —Para comunicaciones generadas por el agent (mensajes de commit, descripciones de pull request, texto orientado al usuario), el judge evalúa si el tono coincide con los estándares organizacionales.

Las evaluaciones probabilísticas se configuran con rúbricas, que son criterios de puntuación estructurados que guían el modelo judge. Una rúbrica define las dimensiones evaluadas, la escala y el umbral de aprobación.

### Combinando Ambos Tipos

El Eval Harness más eficaz combina ambos tipos de validación en un enfoque por capas:

1.  **Las comprobaciones deterministas se ejecutan primero.** Son rápidas y económicas. Si el código falla una comprobación determinista, no tiene sentido ejecutar costosas evaluaciones probabilísticas.
2.  **Las evaluaciones probabilísticas se ejecutan en segundo lugar.** Evalúan los aspectos que sobreviven al filtrado determinista: las cualidades que requieren juicio en lugar de reglas.

Esta estratificación optimiza tanto la confiabilidad como el costo. Las comprobaciones deterministas detectan los problemas fáciles rápidamente; las evaluaciones probabilísticas detectan los sutiles.

### Error de Implementación: Excesiva Dependencia de la Evaluación Probabilística

Un error común y peligroso es depender únicamente de evaluaciones probabilísticas de LLM para lógica de misión crítica. Los LLM judges son potentes pero no deterministas: pueden producir diferentes evaluaciones sobre la misma entrada en distintas ejecuciones, y pueden ser engañados por código escrito con confianza pero incorrecto.

La regla general: si una comprobación puede expresarse como una regla determinista, debe ser determinista. Reserva la evaluación probabilística para cualidades que realmente requieren juicio. Nunca utilices un LLM judge como la única puerta para la validación crítica de seguridad, crítica de cumplimiento o crítica de seguridad. Combina restricciones de código deterministas con comprobaciones de razonamiento probabilístico para obtener tanto confiabilidad como matices.

## Gobernanza

El Eval Harness es un componente de un sistema de gobernanza más amplio que garantiza que el código generado por el agent cumpla con los estándares organizacionales para seguridad, cumplimiento y calidad.

### Puertas [[human-in-the-loop|Human-in-the-Loop]]

Algunas decisiones son demasiado trascendentales para una automatización completa, independientemente de cuán sofisticado se vuelva el Eval Harness. Las puertas HITL obligatorias incluyen:

-   **Cambios críticos de seguridad** —Flujos de autenticación, lógica de autorización, implementaciones de cifrado y cualquier código que maneje datos sensibles.
-   **Decisiones arquitectónicas** —Nuevos límites de servicio, cambios de esquema de base de datos, modificaciones de contratos API y selección de tecnología.
-   **Despliegues de alto radio de impacto** —Cambios que afectan a todos los usuarios, infraestructura crítica o sistemas financieros.
-   **Patrones novedosos** —Implementaciones por primera vez de enfoques no cubiertos por especificaciones existentes o Golden Samples.

El Eval Harness marca estos cambios para revisión humana automáticamente basándose en reglas definidas por el Context Architect y el Principal Systems Architect. La idea clave es que no todos los cambios de código conllevan el mismo riesgo. Las puertas HITL asignan la atención humana donde más importa.

### Integración de [[guardrails|Guardrails]]

El Eval Harness se integra con el sistema de guardrails más amplio para hacer cumplir las restricciones en múltiples niveles:

-   **Guardrails de entrada** —Validan la especificación y el context antes de que el agent comience la ejecución. Rechazan especificaciones mal formadas, marcan context faltante y comprueban patrones de conflicto conocidos.
-   **Guardrails de tiempo de ejecución** —Monitorean el agent durante la ejecución. Intervienen si el agent intenta operaciones prohibidas, excede límites de recursos o entra en bucles improductivos.
-   **Guardrails de salida** —Validan los artefactos finales antes de que salgan del workbench. Aquí es donde se ejecutan las comprobaciones deterministas y probabilísticas del Eval Harness.

## Observabilidad

La gobernanza sin visibilidad es gobernanza solo de nombre. El Eval Harness produce trazas de ejecución estructuradas que hacen que cada decisión del agent sea auditable y depurable.

### Trazas de Ejecución

Cada ejecución de agent produce una traza que enlaza la cadena de razonamiento completa:

-   **Thought** —Lo que el agent decidió hacer y por qué (el razonamiento del modelo)
-   **Action** —Qué llamada a herramienta, cambio de código o comando ejecutó el agent
-   **Result** —Lo que sucedió: el resultado de la herramienta, los resultados de las pruebas o los mensajes de error

Estas trazas se almacenan junto con los artefactos del agent y son accesibles durante la revisión de código. Cuando un revisor quiere entender por qué el agent tomó una elección de implementación particular, la traza proporciona la ruta de razonamiento completa.

### Paneles de [[llmops|LLMOps]]

Agrega las trazas de ejecución en paneles operativos que muestran:

-   **Tasas de aprobación** —¿Qué porcentaje de ejecuciones de agents pasan el Eval Harness en el primer intento? Las tasas de aprobación decrecientes señalan degradación del context o problemas de calidad de la especificación.
-   **Categorías de falla** —¿Qué tipos de comprobaciones fallan con mayor frecuencia? Esto identifica dónde invertir en un mejor context, mejores especificaciones o mejores herramientas.
-   **Tiempos de ciclo** —¿Cuánto tiempo tarda desde el despacho de una tarea hasta que pasa el Eval Harness? El aumento de los tiempos de ciclo puede indicar que las tareas son demasiado complejas para la ejecución de un solo agent.
-   **Patrones de reintento** —¿Cuántos bucles de evaluación requiere la tarea promedio? Las tareas que consistentemente necesitan muchos reintentos pueden requerir mejores especificaciones o una descomposición diferente.

### FinOps y Gestión de Presupuesto de Token

Cada ejecución de agent consume API tokens, recursos de cómputo y tiempo de infraestructura. Sin controles de presupuesto, un solo agent atascado puede consumir recursos significativos antes de que alguien lo note.

El Eval Harness integra la gestión de presupuesto de token a través de interruptores de circuito (circuit breakers):

-   **Presupuestos por tarea** —Cada tarea tiene un presupuesto máximo de token. Cuando el presupuesto se agota, el agent se detiene y la tarea se escala a un humano.
-   **Límites por bucle** —El número máximo de bucles de evaluación-reintento está limitado. Es poco probable que un agent que no puede pasar el harness en un número razonable de intentos tenga éxito con más intentos.
-   **Seguimiento de costos** —Cada ejecución de agent registra su costo total: model tokens, invocaciones de herramientas, tiempo de cómputo. Estos datos se incorporan a los paneles de FinOps del Flow Manager para la gestión de presupuestos a nivel de equipo.
-   **Circuit breakers** —Si el costo por tarea de un agent excede un umbral configurable, el circuit breaker se activa y detiene la ejecución. Esto evita costos descontrolados de agents atascados en bucles improductivos.

Los controles de presupuesto no se tratan solo de ahorro de costos. Son una señal de calidad. Las tareas que agotan consistentemente sus presupuestos son tareas en las que algo anda mal: la especificación no es clara, el context es insuficiente o la tarea supera la capacidad actual de ejecución autónoma.

## Construyendo Tu Eval Harness

### Empieza Simple

El Eval Harness mínimo viable consta de:

1.  **Tu conjunto de pruebas existente** —Pruebas unitarias, pruebas de integración y pruebas de extremo a extremo que ya tienes.
2.  **Un linter y formateador** —Aplica el estilo de código de forma determinista.
3.  **Un escáner de seguridad** —Ejecuta análisis estático para patrones de vulnerabilidad conocidos.
4.  **Una puerta de aprobación/rechazo** —Bloquea el resultado del agent que falle cualquiera de los anteriores.

Esta base detecta los errores de agent más comunes y establece el hábito de la validación automatizada. Puedes añadir evaluaciones probabilísticas, comprobaciones arquitectónicas y gobernanza sofisticada más tarde.

### Integración de [[test-generation-workflow|Generación de Pruebas]]

Los agents pueden ayudar a construir el harness que evalúa su propio resultado. Un patrón potente es tener un agent que genere pruebas a partir del Behavioral Contract de la especificación y un agent diferente que genere la implementación. El agent que escribe las pruebas y el agent de implementación operan de forma independiente, reduciendo el riesgo de que ambos cometan el mismo error.

Este enfoque también escala el Eval Harness orgánicamente. A medida que el equipo escribe más especificaciones con Behavioral Contracts más detallados, la cobertura de pruebas del harness crece automáticamente.

### Progresión de Madurez

A medida que tu práctica con agents madura, el Eval Harness evoluciona:

1.  **Nivel 1: Comprobaciones existentes** —Ejecuta tu conjunto de pruebas actual y los linters contra el resultado del agent.
2.  **Nivel 2: Comprobaciones derivadas de la especificación** —Genera pruebas automáticamente a partir de los criterios de aceptación de Live Spec.
3.  **Nivel 3: LLM-as-a-Judge** —Añade evaluaciones probabilísticas para la calidad del código, la documentación y la alineación arquitectónica.
4.  **Nivel 4: Gobernanza continua** —Integra puertas HITL, controles de costos y observabilidad en una pipeline de gobernanza unificada.
5.  **Nivel 5: Auto-mejora** —El harness utiliza datos de ejecución para identificar lagunas en su propia cobertura y sugiere nuevas comprobaciones al Evaluation Engineer.

Cada nivel se basa en el anterior. No te saltes pasos: un LLM judge sofisticado es inútil si tu conjunto de pruebas básico no se ejecuta de forma confiable.

## Qué Sigue

El Agent Workbench, Context Management y Eval Harness juntos forman la infraestructura operativa del desarrollo con agents. Con esta infraestructura en su lugar, el siguiente paso es construir el equipo que la opera: los roles, habilidades y estructuras organizacionales cubiertos en el capítulo de Modelo de Equipo.
