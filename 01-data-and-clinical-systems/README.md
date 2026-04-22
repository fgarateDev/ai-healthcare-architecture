
# 01. Datos y sistemas clínicos

La primera capa de una arquitectura de IA en sanidad no es el modelo: son los datos y los sistemas clínicos que los generan.

## Por qué esta capa importa

En entornos sanitarios, los datos relevantes no suelen estar en un único sistema ni en un formato homogéneo.
Se distribuyen entre historia clínica, laboratorio, imagen, farmacia, informes y notas en texto libre.

Antes de plantear copilotos, agentes o sistemas RAG, conviene responder a preguntas más básicas:

* ¿Qué datos están realmente disponibles?
* ¿Con qué calidad?
* ¿Con qué nivel de estructura?
* ¿Con qué contexto temporal y clínico?
* ¿Qué limitaciones tienen para su reutilización?

## Qué suele encontrarse en la práctica

En un hospital real, esta capa suele estar formada por una combinación de:

* historia clínica electrónica (HCE / EHR)
* sistemas de laboratorio
* sistemas de imagen
* prescripción y farmacia
* informes clínicos
* notas en texto libre
* bases de datos secundarias o repositorios analíticos
* motores de integración
* mensajería HL7 v2.x
* APIs específicas y, en algunos casos, servicios FHIR

## Nota práctica sobre interoperabilidad

Aunque FHIR tiene un papel relevante en nuevas capas de exposición e intercambio estandarizado, en muchos hospitales la realidad diaria sigue dependiendo en gran medida de HL7 v2.x, integraciones legacy y circuitos heterogéneos entre sistemas.

Por eso, al diseñar una arquitectura de IA en sanidad, el reto no suele ser “usar FHIR”, sino integrarse de forma realista con el ecosistema existente.

## Problemas frecuentes

### 1. Datos incompletos

No todas las variables necesarias están registradas o accesibles.

### 2. Datos inconsistentes

La misma información puede aparecer de forma distinta según el sistema o el profesional que la registre.

### 3. Pérdida de contexto

Un valor aislado puede no tener sentido sin fecha, episodio asistencial, diagnóstico asociado o evolución temporal.

### 4. Exceso de texto libre

Parte del valor clínico está en notas no estructuradas, lo que complica su uso directo.

### 5. Fragmentación

Laboratorio, imagen, farmacia y HCE pueden vivir en sistemas diferentes, con modelos de acceso distintos.

### 6. Dependencia de integraciones legacy

Buena parte del dato útil puede llegar a través de mensajería HL7 v2.x, réplicas, tablas intermedias o integraciones específicas que no fueron diseñadas pensando en IA.

## Qué debería incluir una arquitectura seria

* Inventario de fuentes de datos
* Evaluación de calidad del dato
* Identificación de campos estructurados y no estructurados
* Contextualización temporal
* Reglas mínimas de validación
* Trazabilidad del origen de cada dato
* Definición clara de qué variables alimentan al sistema de IA
* Identificación explícita de ausencias, sesgos y limitaciones

## Cómo se suele resolver esta capa en entornos reales

Una implementación real rara vez parte de una única base de datos “limpia”.
Lo habitual es combinar varias piezas:

* sistemas fuente: HCE/EHR, laboratorio, farmacia, imagen, informes
* capa de integración o intercambio: HL7 v2.x, motores de integración, APIs, ETL/ELT, y a veces FHIR
* capa de validación: reglas de calidad, consistencia, completitud y contexto temporal
* capa de trazabilidad: origen del dato, fecha, sistema fuente y transformaciones aplicadas
* capa de consumo: analítica, soporte a decisión, RAG o modelos de IA

## Herramientas y tecnologías que suelen aparecer

### Interoperabilidad clínica en la práctica

* **HL7 v2.x (2.3 / 2.5)**: muy frecuente en mensajería clínica hospitalaria
* **Motores de integración**: pieza habitual para enrutar, transformar y monitorizar mensajes
* **APIs y conectores específicos**: frecuentes cuando no existe una capa estándar unificada
* **FHIR**: relevante sobre todo en nuevas capas de interoperabilidad, modernización y exposición estandarizada de datos

### Calidad, transformación y consumo

* **Pipelines ETL/ELT** para consolidar datos de varias fuentes
* **Validaciones de calidad** antes de alimentar sistemas de IA
* **Repositorios analíticos o capas intermedias** para no conectar directamente el modelo a los sistemas fuente
* **Servicios de contexto clínico** para construir una vista utilizable por aplicaciones de IA

## Ejemplo práctico 1: resumen clínico para apoyo a decisión

Caso de uso:
un sistema necesita generar un resumen clínico antes de lanzar una recomendación.

Posible flujo:

1. Obtener datos demográficos desde la HCE.
2. Obtener laboratorio reciente.
3. Obtener medicación activa desde farmacia.
4. Recuperar informes recientes o notas clínicas.
5. Validar campos mínimos obligatorios.
6. Registrar origen, fecha y estado de calidad de cada dato.
7. Construir el contexto que consumirá el sistema de IA.

Riesgo:
si laboratorio y medicación no están sincronizados temporalmente, el contexto puede ser engañoso.

## Ejemplo práctico 2: patrón de integración realista

Patrón habitual:

* los sistemas fuente no se conectan directamente al modelo
* una capa intermedia consolida o expone los datos
* se aplican validaciones antes de que los datos lleguen al sistema de IA

Ejemplo conceptual:

HCE / Laboratorio / Farmacia / Informes
↓
Capa de integración / extracción
↓
Validación, contexto y trazabilidad
↓
Sistema de IA / RAG / analítica

## Ejemplo práctico 3: validaciones mínimas antes de usar IA

Comprobaciones típicas:

* ausencia de campos obligatorios vacíos
* coherencia entre identificadores
* fechas válidas y orden temporal correcto
* códigos clínicos dentro del vocabulario esperado
* correspondencia entre pacientes, episodios y resultados
* antigüedad aceptable de los datos usados como contexto

## Ejemplo de fuentes y limitaciones

| Fuente              | Valor potencial                       | Riesgo o limitación             |
| ------------------- | ------------------------------------- | ------------------------------- |
| HCE / EHR           | Antecedentes, diagnósticos, evolución | Heterogeneidad del registro     |
| Laboratorio         | Datos objetivos y seriados            | Falta de contexto clínico       |
| Imagen              | Evidencia complementaria              | Informes no estructurados       |
| Farmacia            | Tratamientos activos                  | Desfase temporal                |
| Notas clínicas      | Mucho contexto                        | Texto libre difícil de procesar |
| Mensajería HL7 v2.x | Flujo operativo entre sistemas        | Variabilidad de implementación  |

## Error habitual

Empezar por el modelo sin haber definido antes:

* qué datos se usarán,
* de dónde vendrán,
* cómo se validarán,
* cómo se actualizarán,
* y qué significado clínico tiene cada variable.

## Checklist mínimo antes de usar IA sobre datos clínicos

* [ ] Se han identificado las fuentes de datos necesarias
* [ ] Se conoce el nivel de calidad de cada fuente
* [ ] Se distinguen datos estructurados y texto libre
* [ ] Existe contexto temporal suficiente
* [ ] Se puede trazar el origen de cada dato
* [ ] Se conocen limitaciones y campos ausentes
* [ ] Se han definido variables mínimas para el caso de uso
* [ ] Se sabe qué integraciones legacy participan en el flujo
* [ ] Se ha evaluado si la capa de datos es suficiente antes de invocar IA

## Qué referencias y marcos conviene tener presentes

### IA en salud

* World Health Organization. *Ethics and governance of artificial intelligence for health*.
  https://www.who.int/publications/i/item/9789240029200

### Interoperabilidad sanitaria

* HL7. *FHIR Overview*.
  https://www.hl7.org/fhir/overview.html
* HL7. *HL7 V2.x Product Brief*.
  https://www.hl7.org/implement/standards/product_brief.cfm?product_id=185

### Gobernanza y gestión del riesgo en IA

* NIST. *AI Risk Management Framework*.
  https://www.nist.gov/itl/ai-risk-management-framework

## Idea clave

Una IA no empieza en el modelo.
Empieza en la calidad del dato clínico, en su contexto y en la comprensión de los sistemas que lo producen.
