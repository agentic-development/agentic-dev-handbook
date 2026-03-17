---
title: Roles en Desarrollo Agéntico
description: >-
  Seis definiciones de roles para el equipo de Desarrollo Agéntico — desde
  Arquitecto de Contexto hasta Arquitecto Principal de Sistemas
authors:
  - dpavancini
lastUpdated: '2026-02-23'
---

## Resumen

El desarrollo agentic no elimina los roles de ingeniería, los transforma. Los títulos tradicionales como "Product Manager" y "QA Engineer" persisten en nombre, pero cambian drásticamente en la práctica. Esta página define seis roles diseñados específicamente para el Equipo Híbrido, los mapea desde sus contrapartes tradicionales y detalla las responsabilidades diarias y las habilidades que cada uno requiere.

## Evolución de Roles

El cambio de roles tradicionales a agentic sigue un patrón consistente: el esfuerzo humano se aleja de la ejecución directa y se dirige hacia la especificación, la orquestación y la evaluación.

| Rol Tradicional | Rol Agentic | Cambio Clave |
|---|---|---|
| Product Manager | Context Architect | De escribir *user stories* a diseñar especificaciones legibles por máquina |
| Software Engineer | Agent Operator / Reviewer | De escribir código a orquestar *agents* y auditar la salida |
| QA Engineer | Evaluation Engineer | De encontrar *bugs* manualmente a diseñar sistemas de restricciones automatizados |
| Scrum Master | Flow Manager / AgentOps | De facilitar ceremonias a gestionar el rendimiento del *pipeline* de AI |
| DevOps / Platform Engineer | Agent Platform Engineer | De *CI/CD pipelines* a entornos de ejecución de *agents* en *sandbox* |
| Tech Lead | Principal Systems Architect | De la codificación práctica a la definición de leyes arquitectónicas y ejemplos *golden* |

Observe que cada cambio mueve al humano más "río arriba" — más cerca de la intención, las restricciones y la gobernanza, más lejos de la ejecución línea por línea.

## 1. Context Architect

El Context Architect es el orquestador estratégico de la fuerza de trabajo híbrida. Mientras que un *Product Manager* tradicional traduce las necesidades del negocio en *user stories* para desarrolladores humanos, el Context Architect las traduce en especificaciones legibles por máquina que los *AI agents* pueden ejecutar sin ambigüedad.

### Deberes Principales

- **Elaboración de especificaciones en vivo** — Producir documentos de especificación estructurados que incluyen criterios de aceptación precisos, *edge cases*, contratos de entrada/salida y referencias explícitas de *context*. Estas especificaciones son la entrada principal para la ejecución del *agent* — su calidad determina directamente la calidad de la salida. Esta es la práctica de [[context-engineering]] aplicada a nivel de equipo.
- **Curación del Context Index** — Mantener la base de conocimiento organizada de la que los *agents* se nutren durante la ejecución: esquemas de API, glosarios de dominio, registros de decisiones, registros de decisiones arquitectónicas y ejemplos de implementaciones previas. Un Context Index bien curado reduce la alucinación y la desviación del *agent*.
- **Configuración de puertas HITL** — Definir qué tareas requieren aprobación humana antes del *merge* y cuáles pueden proceder de forma autónoma. Esto implica evaluar el riesgo por tipo de tarea y establecer umbrales: el trabajo de mantenimiento de bajo riesgo fluye automáticamente, mientras que el trabajo de características de alto riesgo requiere la revisión del Agent Operator.

### Habilidades Clave

- **Arquitectura lógica** — La capacidad de descomponer requisitos de negocio complejos en especificaciones estructuradas e inequívocas. Esto está más cerca del análisis de sistemas que del *product management* tradicional.
- **Organización de datos** — Experiencia en la estructuración de conocimiento para el consumo de máquinas: taxonomías, sistemas de etiquetado, documentación optimizada para la recuperación.
- **Diplomacia con los stakeholders** — Traducir entre los *stakeholders* del negocio que piensan en resultados y los sistemas técnicos que requieren entradas precisas. El Context Architect cierra la brecha entre la intención humana y la ejecución de la máquina.

## 2. Agent Operator

El Agent Operator es el líder técnico de alto apalancamiento para el enjambre de AI. No pasa sus días escribiendo código de características desde cero. En cambio, configura, lanza, monitorea y audita las ejecuciones de los *agents* — interviniendo solo cuando los *agents* se atascan o producen una salida deficiente.

### Deberes Principales

- **Desbloqueo de *agents* atascados** — Diagnosticar por qué un *agent* entró en un bucle, malinterpretó una especificación o no logró producir pruebas exitosas. El Agent Operator proporciona *context* correctivo, ajusta parámetros y relanza. Esta es la "Agent Recovery" — la parte más sensible al tiempo del rol.
- **Escritura de *scripts* y herramientas personalizados** — Construir la automatización que conecta los *agents* al entorno de desarrollo: servidores MCP personalizados, *scripts* de inyección de *context* y *monitoring dashboards*.
- **Escritura de código crítico de "Human-Owned Core"** — Algunos códigos son demasiado importantes o arquitectónicamente sensibles para la ejecución del *agent*. El Agent Operator escribe estos componentes a mano — rutas críticas de seguridad, algoritmos centrales y abstracciones fundamentales de las que todo lo demás depende.

### Habilidades Clave

- **Auditoría de código** — La capacidad de revisar rápidamente el código generado por *agents* en busca de corrección, vulnerabilidades de seguridad, problemas de rendimiento y cumplimiento arquitectónico. Esto requiere leer código más rápido que escribirlo.
- ***Debugging* de Linux/sistema** — Los *agents* operan en entornos *sandboxed*. Cuando algo falla a nivel de infraestructura — permisos de archivos, políticas de red, límites de recursos — el Agent Operator necesita diagnosticarlo y solucionarlo.
- ***Prompt engineering* técnico** — Elaborar los *prompts*, instrucciones del sistema y paquetes de *context* que dirigen el comportamiento del *agent*. No se trata de una redacción inteligente, sino de proporcionar la información correcta en la estructura adecuada para que el modelo razone de forma eficaz.

## 3. Evaluation Engineer

El Evaluation Engineer representa la transformación de rol más dramática en el equipo agentic. Los *QA engineers* tradicionales encuentran *bugs* probando software manualmente. Los Evaluation Engineers cambian el QA de encontrar *bugs* a diseñar las restricciones que evitan que se creen en primer lugar.

### Deberes Principales

- **Construcción de entornos de prueba Dockerizados** — Crear entornos aislados y reproducibles donde los *agents* se ejecutan y su salida es validada. Estos entornos reflejan las condiciones de producción mientras mantienen un estricto aislamiento por seguridad.
- **Escritura de casos de prueba antes de que el *agent* se inicie** — Definir los criterios de evaluación de antemano, no a posteriori. El Evaluation Engineer produce *test harnesses* que el *agent* debe satisfacer, convirtiendo el QA de un punto de control reactivo en una especificación proactiva.
- **Desarrollo de rúbricas LLM-as-a-Judge** — Crear marcos de evaluación estructurados donde un LLM secundario evalúa la salida del *agent* frente a criterios de calidad: adherencia al estilo de código, patrones de seguridad, completitud de la documentación y cumplimiento arquitectónico. Estas rúbricas automatizan los aspectos de la revisión de código que no requieren juicio humano.

### Habilidades Clave

- **Python/TypeScript** — Lenguajes principales para escribir *evaluation harnesses*, generadores de pruebas y *automation scripts*.
- **Containerization** — Experiencia profunda en Docker y orquestación de contenedores para construir los entornos aislados donde los *agents* se ejecutan de forma segura.
- **Análisis estadístico** — Evaluar el rendimiento del *agent* requiere pensamiento estadístico: seguimiento de tasas de aprobación a lo largo del tiempo, identificación de patrones de regresión, medición de intervalos de confianza en métricas de calidad y distinción entre señal y ruido en los resultados de la evaluación.

## 4. Flow Manager

El Flow Manager es el "Controlador de Tráfico Aéreo" del Equipo Híbrido. Mientras que un *Scrum Master* tradicional facilita ceremonias humanas y elimina *blockers*, el Flow Manager asegura que todo el *pipeline* — desde la creación de especificaciones hasta la ejecución del *agent* y la revisión humana — fluya sin cuellos de botella.

### Deberes Principales

- **Observabilidad del *pipeline*** — Monitorear el flujo de trabajo de extremo a extremo a través del equipo: cuántas especificaciones están en refinamiento, cuántos *agents* se están ejecutando, cuántos *pull requests* esperan revisión y dónde se están formando colas. El Flow Manager detecta los cuellos de botella antes de que se propaguen.
- **Gestión del presupuesto de cómputo de AI (*Agent FinOps*)** — Rastrear y optimizar el costo de la ejecución del *agent*. Cada ejecución de *agent* consume *API tokens*, recursos de cómputo y tiempo de infraestructura. El Flow Manager equilibra el rendimiento con el costo, asegurando que el equipo entregue el máximo valor por cada dólar gastado. Esto se conecta directamente con las prácticas de [[llmops]] aplicadas a nivel de equipo.
- **Balanceo de carga de revisiones** — Distribuir el trabajo de revisión entre los Agent Operators para evitar que una sola persona se convierta en un cuello de botella. Cuando las colas de revisión crecen, el Flow Manager redistribuye el trabajo o escala los problemas de capacidad.

### Habilidades Clave

- **Lean/Kanban theory** — Comprensión de la gestión del trabajo basada en el flujo: límites de trabajo en curso, medición del tiempo de ciclo, optimización del rendimiento y teoría de cuellos de botella. Estos conceptos se traducen directamente a la gestión de *pipelines* híbridos humano-agente.
- **Alfabetización de datos** — Comodidad con *dashboards*, métricas y análisis de tendencias. El Flow Manager vive en herramientas de observabilidad y necesita interpretar rápidamente lo que los datos dicen sobre el rendimiento del equipo.
- **Conciencia financiera** — Comprensión del modelo de costos de la ejecución de *AI agents*: precios de *tokens*, costos de cómputo y el cálculo del ROI que justifica el uso de *agents* para diferentes tipos de tareas.

## 5. Agent Platform Engineer

El Agent Platform Engineer construye y mantiene el Workbench Runtime — el entorno de ejecución en *sandbox* donde operan los *agents*. Este rol es el equivalente agentic de un *DevOps/Platform Engineer*, pero enfocado en la infraestructura de *agents* en lugar de la infraestructura de aplicaciones.

### Deberes Principales

- **Infraestructura de microVM aisladas** — Construir los entornos seguros y *sandboxed* donde los *agents* ejecutan código. Cada ejecución de *agent* opera en su propio entorno aislado con acceso controlado al *filesystem*, la red y los recursos del sistema. Esto evita que los *agents* afecten los sistemas de producción o entre sí.
- **JSON Function Definitions** — Definir las interfaces de herramientas que los *agents* usan para interactuar con el entorno de desarrollo. Cada llamada a herramienta que un *agent* puede hacer — lecturas de archivos, ejecución de comandos, llamadas a API — debe estar explícitamente definida, documentada y asegurada.
- **Gobernanza de egreso de red** — Controlar a qué pueden acceder los *agents* a través de la red. Por defecto, los *agents* deben tener un acceso mínimo a la red. El Agent Platform Engineer define y aplica *allowlists* para *API endpoints*, registros de paquetes y fuentes de documentación.

### Habilidades Clave

- **Virtualización a nivel de *kernel*** — Comprensión profunda de *Linux namespaces*, *cgroups*, perfiles *seccomp* y *container runtimes*. El *sandboxing* de *agents* requiere un control granular sobre lo que los procesos pueden hacer a nivel del sistema operativo.
- **eBPF** — Experiencia en *extended Berkeley Packet Filter* para observabilidad en tiempo de ejecución y aplicación de seguridad. *eBPF* proporciona el monitoreo de baja sobrecarga necesario para rastrear el comportamiento del *agent* sin impactar el rendimiento de la ejecución.
- **API design** — Diseñar las interfaces de herramientas que consumen los *agents*. Estas API deben ser precisas, bien documentadas y a prueba de fallos — porque el consumidor es un modelo de AI que no puede hacer preguntas aclaratorias cuando una interfaz es ambigua.

## 6. Principal Systems Architect

El Principal Systems Architect define las "Leyes de la Física" para la base de código. Mientras que un *Tech Lead* tradicional podría dividir su tiempo entre la codificación y la guía técnica, el Principal Systems Architect se concentra casi por completo en la definición de restricciones — creando las reglas y materiales de referencia que aseguran que el código generado por *agents* sea estructuralmente sólido.

### Deberes Principales

- **Bounded contexts (DDD)** — Definir límites de dominio claros utilizando principios de Domain-Driven Design (DDD). Cada *bounded context* tiene su propio lenguaje, modelos y reglas. Los *agents* que operan dentro de un *bounded context* heredan esas restricciones, lo que les impide crear acoplamiento entre dominios o violar la separación de preocupaciones.
- **Pruebas automatizadas de restricciones arquitectónicas** — Escribir pruebas ejecutables que verifiquen reglas arquitectónicas: comprobaciones de dirección de dependencia, detección de violación de capas, aplicación de convenciones de nombres y validación de contratos de API. Estas pruebas se ejecutan como parte del *pipeline* de ejecución del *agent*, detectando violaciones antes de que el código llegue a revisión.
- **Curación de *Golden Samples*** — Seleccionar y mantener implementaciones de referencia que demuestren la forma correcta de construir cada tipo de componente en la base de código. Los *Golden Samples* son la forma más eficiente de instrucción para los *AI agents* — un ejemplo bien elegido comunica patrones, estilo e intención de manera más efectiva que páginas de reglas escritas.

### Habilidades Clave

- **DDD mastery** — Dominio en Domain-Driven Design (DDD): *bounded contexts*, *aggregates*, *domain events*, *anti-corruption layers* y *context mapping*. Estos conceptos proporcionan el vocabulario para definir los límites que los *agents* deben respetar.
- **Herramientas de análisis estático** — Experiencia en herramientas como ArchUnit, dependency-cruiser, reglas personalizadas de ESLint y marcos similares que codifican reglas arquitectónicas como verificaciones ejecutables.
- ***Refactoring* de legado** — Experiencia con la modernización incremental de grandes bases de código. El Principal Systems Architect debe definir rutas de migración que los *agents* puedan ejecutar de forma segura, un *bounded context* a la vez.

## Tamaño del Equipo

No todos los equipos necesitan los seis roles desde el primer día. La composición del equipo se escala con la madurez de su adopción agentic:

- **Inicio (1-2 *agents*):** Un solo ingeniero actúa como Agent Operator y Context Architect. El *tech lead* existente asume las responsabilidades del Principal Systems Architect. Todavía no se necesita un Flow Manager o Platform Engineer dedicado.
- **Crecimiento (3-5 *agents*):** Surgen roles dedicados de Context Architect y Agent Operator. Un ingeniero con experiencia en *DevOps* comienza a construir el Workbench Runtime. QA comienza la transición al modelo de Evaluation Engineer.
- **A escala (5+ *agents*):** Los seis roles están cubiertos. El Flow Manager se vuelve esencial a medida que la complejidad del *pipeline* aumenta. El Agent Platform Engineer mantiene una infraestructura de *sandboxing* cada vez más sofisticada.

El principio clave: agregue roles a medida que surja la necesidad, no por anticipación. Deje que los cuellos de botella le indiquen cuándo se necesita un nuevo rol.

## Qué Sigue

Con los roles y la estructura del equipo definidos, el siguiente paso es construir la infraestructura sobre la que operan estos equipos — los entornos de ejecución en *sandbox*, los sistemas de observabilidad y los marcos de gobernanza que hacen posible la ejecución segura y escalable de *agents*.
