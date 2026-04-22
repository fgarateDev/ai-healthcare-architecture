# AI Healthcare Architecture

Serie de notas prácticas sobre arquitectura de IA en sanidad.

El objetivo de este repositorio es aterrizar, con un enfoque técnico y realista, qué capas suelen ser necesarias para construir sistemas de IA clínicamente útiles, seguros, trazables e integrables en entornos sanitarios reales.

## Por qué este repositorio

En IA aplicada a sanidad, gran parte de la conversación gira en torno al modelo.

Sin embargo, en entornos reales, el valor no depende solo de la capacidad del modelo para generar o predecir, sino de la arquitectura que hace posible su uso de forma segura, gobernada, interoperable y clínicamente confiable.

Este repositorio reúne notas breves, ejemplos, checklists y esquemas para bajar esa conversación a tierra.

## Capas de la arquitectura

### 1. Datos y sistemas clínicos
La IA no empieza en el modelo. Empieza en la calidad del dato clínico y en la comprensión de los sistemas que lo producen.

### 2. Interoperabilidad
La IA no puede vivir aislada. Tiene que integrarse con la realidad del hospital y con sus flujos asistenciales.

### 3. Gobernanza y seguridad
No basta con que el sistema funcione: debe hacerlo con control de acceso, privacidad, trazabilidad y gestión del riesgo.

### 4. Motor IA + RAG
Un RAG útil en sanidad no consiste solo en conectar documentos, sino en trabajar con fuentes fiables, versionadas y recuperadas con criterio.

### 5. Supervisión humana e impacto clínico
La IA debe apoyar la decisión, no desplazar el juicio profesional ni romper los circuitos de validación clínica.

## Diagrama general

```text
[1. Datos y sistemas clínicos]
            ↓
[2. Interoperabilidad]
            ↓
[3. Gobernanza y seguridad]
            ↓
[4. Motor IA + RAG]
            ↓
[5. Supervisión humana e impacto clínico]
