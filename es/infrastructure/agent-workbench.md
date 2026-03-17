---
title: El Banco de Trabajo del Agente
description: >-
  Entornos de ejecución efímeros y el Registro de Herramientas Empresariales que
  impulsan operaciones seguras de agentes
authors:
  - dpavancini
lastUpdated: '2026-02-23'
---

## Resumen

El Agent Workbench es el entorno de ejecución aislado, seguro y desechable donde los AI agents llevan a cabo sus tareas. Cada ejecución de agent se aprovisiona con su propia micro-máquina virtual, precargada con el código base, las herramientas y el context necesarios. Cuando la tarea se completa, el workbench se destruye. Esta página cubre la arquitectura del workbench, el Enterprise Tool Registry que equipa a los agents con capacidades curadas, las opciones de deploy, y el modelo de seguridad que hace que la ejecución autónoma sea segura a escala empresarial.

## El Workbench

Un Agent Workbench es una microVM efímera aprovisionada bajo demanda para una única tarea de agent. Contiene todo lo que el agent necesita —una copia limpia del código base relevante, dependencias preinstaladas, herramientas configuradas y el *context slice* definido por la Live Spec de la tarea— y nada de lo que no.

### Ciclo de vida

1.  **Aprovisionamiento** —Cuando se despacha una tarea, la plataforma inicia una nueva microVM en segundos. El workbench se precarga con la instantánea del código base, el *context* relevante del Context Index y las herramientas especificadas en la configuración de la tarea.
2.  **Ejecución** —El agent opera dentro del workbench: leyendo archivos, ejecutando comandos, llamando a APIs y produciendo *artifacts*. Todas las acciones están en un *sandbox* —el agent no puede acceder a sistemas de producción, a los workbenches de otros agents o a infraestructura compartida.
3.  **Extracción de Artifacts** —Cuando el agent señala la finalización (o la ejecución agota el tiempo), la plataforma extrae los resultados: cambios de código, resultados de pruebas, *logs* y rastros de evaluación.
4.  **Desmantelamiento** —El workbench se destruye. Ningún estado persiste. Ninguna credencial permanece. La siguiente tarea comienza desde cero.

Este ciclo de vida elimina toda una clase de riesgos operativos. Los agents no pueden acumular estados obsoletos, filtrar secretos entre tareas ni crear efectos secundarios persistentes que corrompan ejecuciones futuras.

### Garantías de aislamiento

Cada workbench proporciona:

-   **Aislamiento del sistema de archivos** —El agent solo ve su propio espacio de trabajo. No puede leer ni escribir archivos fuera de sus directorios designados.
-   **Aislamiento de red** —El acceso a la red de salida está restringido a una *allowlist* explícita: registros de paquetes aprobados, fuentes de documentación y API endpoints. Todo otro egreso está bloqueado por defecto.
-   **Aislamiento de procesos** —Los procesos del agent se ejecutan en su propio *namespace*. No pueden observar ni interactuar con procesos de otros workbenches o del sistema anfitrión.
-   **Límites de recursos** —La CPU, la memoria y el tiempo de ejecución están limitados. Un agent descontrolado no puede consumir recursos ilimitados ni ejecutarse indefinidamente.

## Seguridad por diseño

La infraestructura efímera hace de la seguridad una propiedad estructural del sistema en lugar de una capa de política que debe ser aplicada manualmente.

### Contención del radio de explosión

Si un agent hace algo incorrecto —malinterpreta una *spec*, escribe un comando destructivo o es víctima de un ataque de [[prompt-injection]]— el daño se contiene dentro de un entorno desechable. El peor de los casos es una ejecución de workbench desperdiciada, no una base de datos de producción corrompida.

### Gestión de secretos

Los secretos y las credenciales se inyectan en el workbench en tiempo de ejecución a través de una integración segura de *vault* y nunca se persisten en disco. Cuando el workbench se destruye, los secretos desaparecen con él. Los agents nunca poseen credenciales de larga duración, *tokens* o claves de sesión. Esto significa que un agent comprometido no puede ser utilizado como cabeza de playa para el movimiento lateral a través de su infraestructura.

### Redacción de secretos

Toda la salida del workbench —*logs*, salida de terminal, código generado— pasa por una capa de *redacción* antes de salir del *sandbox*. Esta capa detecta y enmascara automáticamente:

-   API keys y *tokens*
-   *Strings* de conexión a bases de datos
-   Información de identificación personal (PII)
-   Claves privadas y certificados

Incluso si un agent incluye inadvertidamente un secreto en su salida, la capa de *redacción* lo detecta antes de que llegue a un *pull request*, un agregador de *logs* o un revisor humano.

### Integración RBAC

Los controles de acceso del workbench se integran con su infraestructura de identidad existente. A través de Active Directory, LDAP o federación OIDC, las organizaciones pueden aplicar:

-   Qué equipos pueden aprovisionar workbenches
-   Qué conjuntos de herramientas están disponibles para qué roles
-   A qué repositorios y *context slices* pueden acceder los agents
-   Requisitos de aprobación para invocaciones de herramientas de alto privilegio

Esto asegura que el principio del menor privilegio se extienda desde sus ingenieros humanos hasta sus AI agents.

## El Enterprise Tool Registry

Los agents son tan capaces como las herramientas que pueden invocar. El Enterprise Tool Registry es un catálogo curado y gobernado de herramientas que los agents pueden usar durante la ejecución del workbench.

### Qué incluye el registro

El registro contiene herramientas de varias categorías:

-   **API wrappers** —Clientes preconfigurados para servicios internos, APIs de terceros y recursos de proveedores de nube. Cada *wrapper* maneja la autenticación, la limitación de tasas y el manejo de errores para que el agent no tenga que hacerlo.
-   **Integraciones de [[model-context-protocol|MCP]]** —Conexiones a servidores de Model Context Protocol que exponen capacidades estructuradas: consultas de bases de datos, operaciones de sistema de archivos, búsqueda y más. MCP proporciona una interfaz estandarizada que los modelos pueden invocar de manera confiable.
-   **Módulos de *Infrastructure-as-Code*** —Módulos de Terraform, plantillas de CloudFormation y *manifests* de Kubernetes que los agents pueden aplicar a través de rutas de ejecución controladas. Estos módulos están preaprobados y probados, asegurando que los agents aprovisionen infraestructura de manera segura.
-   **Herramientas de desarrollo** —Linters, formatters, *test runners*, herramientas de *build* y utilidades de análisis estático. Estas son las mismas herramientas que usan sus ingenieros humanos, empaquetadas para la invocación de agents.
-   **Herramientas de observabilidad** —*Log queriers*, *dashboards* de métricas y exploradores de *traces* que los agents pueden usar para diagnosticar problemas y validar su propio trabajo.

### JSON Function Definitions

Cada herramienta en el registro está envuelta en una JSON Function Definition —un esquema estructurado que le dice al LLM exactamente cómo invocar la herramienta. Una definición de función incluye:

-   **Nombre y descripción** —Lo que hace la herramienta, expresado en lenguaje natural que ayuda al modelo a decidir cuándo usarla.
-   **Esquema de parámetros** —Un JSON Schema que define las entradas que acepta la herramienta, incluyendo tipos, restricciones, campos requeridos y valores por defecto.
-   **Esquema de retorno** —Lo que la herramienta devuelve en caso de éxito y de fallo.
-   **Declaración de efectos secundarios** —Si la herramienta es de solo lectura o realiza mutaciones. Esto ayuda a la capa de gobernanza a determinar qué invocaciones requieren aprobación adicional.

Las definiciones de función bien elaboradas son la diferencia entre agents que usan herramientas de manera confiable y agents que *alucinan* llamadas a herramientas. La definición es el contrato entre el modelo y la herramienta —la ambigüedad en la definición conduce a ambigüedad en el uso.

### Gobernanza de [[tool-calling|Tool Calling]]

No todas las herramientas conllevan el mismo riesgo. El registro soporta gobernanza por niveles:

-   **Herramientas sin restricciones** —Operaciones de solo lectura como lecturas de archivos, consultas de búsqueda y *lookups* de documentación. Los agents pueden invocarlas libremente.
-   **Herramientas monitoreadas** —Operaciones con efectos secundarios limitados como escribir archivos o ejecutar pruebas. Estas se registran (*logged*) y son auditables, pero no requieren aprobación previa.
-   **Herramientas controladas** —Operaciones de alto impacto como *deployar* infraestructura, modificar controles de acceso o llamar a APIs de pago externas. Estas requieren aprobación explícita a través de un *checkpoint* de [[guardrails]] antes de que la ejecución continúe.

Este modelo por niveles evita restringir excesivamente a los agents (lo que mata la productividad) mientras se mantiene el control sobre operaciones realmente riesgosas.

## Arquitectura de Deploy

La plataforma Agent Workbench soporta múltiples modelos de *deploy* para adaptarse a los diferentes requisitos organizacionales de soberanía de datos, cumplimiento y latencia.

### Deploy de SaaS

La opción más sencilla. La plataforma workbench se ejecuta como un servicio gestionado en la nube del proveedor. Las organizaciones conectan sus repositorios y configuran sus registros de herramientas a través de una interfaz web.

**Ideal para:** Equipos que desean empezar rápidamente sin inversión en infraestructura. Adecuado cuando el código y los datos pueden salir de la red corporativa.

**Compensaciones:** Los datos transitan por infraestructura externa. La personalización se limita a lo que expone la plataforma. La latencia depende de la disponibilidad regional del proveedor.

### Deploy de VPC

La plataforma workbench se ejecuta dentro de la Virtual Private Cloud propia de la organización. El proveedor gestiona el software; la organización es propietaria de la red y el *compute*.

**Ideal para:** Organizaciones con requisitos de cumplimiento moderados. El código permanece dentro del límite de la red corporativa. El equipo de seguridad mantiene el control sobre las políticas de red y los flujos de datos.

**Compensaciones:** Requiere experiencia en infraestructura de nube para configurar y mantener el entorno VPC. Mayor sobrecarga operativa que SaaS.

### Deploy On-Premises y Air-Gapped

Toda la plataforma se ejecuta en el hardware propio de la organización, sin dependencias de red externas. Toda la inferencia de modelos, la ejecución de herramientas y el almacenamiento de *artifacts* ocurren dentro del perímetro corporativo.

**Ideal para:** Defensa, servicios financieros, atención médica y cualquier organización donde los datos nunca deben salir de las instalaciones. Soporta entornos clasificados o altamente regulados.

**Compensaciones:** La carga operativa más alta. Requiere equipos de infraestructura dedicados para gestionar el *compute*, el almacenamiento y las actualizaciones de la plataforma. El *hosting* de modelos debe manejarse internamente, lo que limita las opciones de modelos disponibles.

### Elegir un modelo de deploy

| Factor                 | SaaS              | VPC                         | On-Premises         |
| :--------------------- | :---------------- | :-------------------------- | :------------------ |
| **Tiempo de valorización** | Días              | Semanas                     | Meses               |
| **Soberanía de datos** | Controlado por el proveedor | Controlado por la organización | Totalmente interno |
| **Cumplimiento**       | Estándar          | Moderado                    | Máximo              |
| **Carga operativa**    | Mínimo            | Moderado                    | Significativo       |
| **Personalización**    | Limitado          | Moderado                    | Completo            |
| **Disponibilidad de modelos** | Catálogo completo | Catálogo completo           | Solo *self-hosted* |

La mayoría de las organizaciones comienzan con el *deploy* de SaaS o VPC y migran a *on-premises* solo cuando los requisitos regulatorios o de seguridad lo exigen.

## Resumiendo

El Agent Workbench es la capa de ejecución que hace posible todo lo demás en el Agentic Development Framework. Sin entornos aislados y efímeros, la ejecución autónoma de *agents* sería un riesgo de seguridad inaceptable. Sin un registro de herramientas gobernado, los *agents* carecerían de las capacidades para realizar un trabajo útil. Sin opciones de *deploy* flexibles, las organizaciones no podrían adoptar flujos de trabajo *agentic* dentro de sus restricciones de cumplimiento existentes.

El workbench no opera de forma aislada. Recibe sus instrucciones del Context Index y las Live Specs (cubiertas en la siguiente página), y su salida es validada por el Evaluation Harness antes de que cualquier humano la vea. Juntos, estos tres componentes —workbench, *context* y evaluación— forman la infraestructura operativa del desarrollo *agentic*.
