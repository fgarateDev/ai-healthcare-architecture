# 02. Interoperabilidad e integración clínica

## 1. Propósito de la capa

### 1.1. Qué cubre esta capa

Esta capa se centra en cómo los sistemas clínicos intercambian, exponen y encaminan la información para que los datos puedan moverse desde el lugar donde se generan hasta el lugar donde realmente se necesitan.

Si la capa anterior trataba de entender qué datos existen, en qué sistemas viven y qué calidad o limitaciones tienen, esta capa trata de cómo hacer que esa información circule de forma utilizable dentro del ecosistema clínico.

El objetivo no es describir una arquitectura ideal construida desde cero, sino entender cómo conectar sistemas hospitalarios reales de forma suficientemente robusta como para soportar analítica, alertas, soporte a la decisión o servicios basados en IA sin romper los flujos operativos.

### 1.2. Por qué la IA no puede vivir aislada

Un componente de IA puede ser técnicamente competente y, aun así, no aportar valor si permanece desconectado de los sistemas y procesos donde realmente ocurre la actividad clínica.

En la práctica, una IA útil en sanidad suele depender de su capacidad para recibir información desde sistemas existentes, procesarla a tiempo y devolver un resultado por un canal que encaje con el flujo clínico real.

Por eso, la interoperabilidad no es solo una cuestión técnica. Es una de las condiciones que determinan si un sistema de IA puede pasar de una demo a formar parte del trabajo habitual.

---

## 2. Qué significa interoperabilidad en entornos reales

### 2.1. Ecosistemas clínicos heterogéneos

Un hospital rara vez funciona como una única plataforma digital coherente. Lo habitual es convivir con múltiples sistemas incorporados a lo largo del tiempo para funciones distintas: historia clínica electrónica, laboratorio, farmacia, imagen, admisión, alta, aplicaciones departamentales, repositorios secundarios, etc.

Cada uno de estos sistemas puede manejar estructuras de datos distintas, identificadores diferentes, ritmos de actualización propios y mecanismos de integración no homogéneos.

Por eso, en la práctica, la interoperabilidad rara vez consiste en conectar dos sistemas modernos y limpios. Lo normal es intentar que la información circule entre componentes heterogéneos.

### 2.2. Convivencia con sistemas legacy

Una parte importante de la interoperabilidad hospitalaria sigue dependiendo de sistemas legacy y de patrones de integración establecidos hace años.

En muchos entornos, una parte del flujo clínico sigue apoyándose en mensajería HL7 v2.x, interfaces antiguas, conectores específicos, bases intermedias o integraciones desarrolladas para necesidades concretas, no pensando originalmente en casos de uso de IA.

Esto no las convierte en algo irrelevante. Al contrario: en muchos casos, esas piezas son precisamente la base real sobre la que deben construirse nuevos servicios.

### 2.3. Intercambio de datos frente a integración real

Enviar o recibir datos no equivale a integrar un proceso.

Un sistema puede recibir mensajes o exponer un endpoint y, aun así, no encajar de verdad en el flujo clínico. La integración real exige que la información llegue al lugar adecuado, en el momento adecuado, con el formato adecuado y de una forma que facilite la acción clínica en lugar de añadir fricción.

Por eso, la interoperabilidad no debería reducirse a la existencia de un estándar o de una interfaz. También implica ajuste al proceso, fiabilidad operativa y capacidad de sostener el intercambio útil en el tiempo.

### 2.4. Interoperabilidad técnica, semántica y de proceso

La interoperabilidad tiene varias dimensiones.

A nivel técnico, los sistemas deben poder intercambiar información mediante algún mecanismo. A nivel semántico, esa información debe significar lo mismo en ambos lados. Y a nivel de proceso, el intercambio debe encajar en el flujo de trabajo donde se utiliza.

Muchos problemas no aparecen porque no exista conexión, sino porque lo que se intercambia no se interpreta igual, no llega con el contexto necesario o no se incorpora bien al circuito clínico correspondiente.## 3. Problemas reales de interoperabilidad

### 3.1. Sistemas aislados

En muchos entornos hospitalarios, la información relevante para un caso de uso clínico se encuentra repartida entre aplicaciones que no fueron diseñadas para trabajar de forma conjunta.

Esto obliga a construir integraciones específicas o a depender de circuitos parciales, con el riesgo de que parte del contexto no llegue al sistema que lo necesita.

### 3.2. Integraciones frágiles

No todas las integraciones son igual de robustas. Algunas dependen de formatos poco consistentes, transformaciones difíciles de mantener o supuestos que dejan de cumplirse cuando cambia un sistema emisor.

Una integración puede funcionar durante mucho tiempo y, aun así, ser frágil frente a cambios pequeños en versiones, campos, estructuras o reglas locales.

### 3.3. Duplicidad y desalineación de la información

La misma información puede circular por varios canales o aparecer representada de forma distinta en sistemas diferentes.

Esto genera problemas de coherencia, dificulta saber cuál es la fuente más fiable y puede producir desalineación entre lo que un sistema emite, otro interpreta y un tercero utiliza para tomar una decisión o generar una alerta.

### 3.4. Dependencia de procesos manuales

En algunos circuitos, parte de la integración real sigue dependiendo de pasos manuales: revisión de pantallas, reenvío de información, extracción no automatizada o validaciones humanas intermedias.

Estos procesos pueden resolver necesidades prácticas, pero también añaden latencia, variabilidad y puntos de fallo difíciles de escalar o mantener.

### 3.5. Variabilidad entre implementaciones

Aunque dos sistemas utilicen el mismo estándar o mecanismo general, no siempre lo hacen de la misma manera.

En la práctica, las diferencias entre implementaciones, versiones, convenciones locales o campos opcionales pueden generar bastante complejidad. Por eso, interoperabilidad no significa solo “usar HL7” o “tener una API”, sino entender cómo está implementado realmente cada intercambio.

### 3.6. Dificultad para incorporar nuevos casos de uso

Muchas integraciones resuelven necesidades concretas y funcionan razonablemente bien mientras el escenario no cambia.

El problema aparece cuando se quiere añadir un nuevo servicio, un nuevo consumidor de datos o una nueva lógica clínica. Si la integración está demasiado acoplada, cualquier ampliación exige mucho esfuerzo técnico y organizativo.

---

## 4. Capacidades necesarias en esta capa

### 4.1. Identificación de sistemas implicados

Una arquitectura seria debería empezar por identificar con claridad qué sistemas participan en el flujo: cuáles generan información, cuáles la transmiten, cuáles la transforman y cuáles la consumen.

No basta con saber que “el dato existe”. Hay que entender qué papel cumple cada sistema dentro del circuito real.

### 4.2. Definición de flujos de entrada y salida

Conviene definir qué información entra al sistema, desde dónde llega, con qué frecuencia, en qué formato y hacia dónde debe salir después.

Esta definición ayuda a evitar integraciones ambiguas, dependencias implícitas y expectativas poco realistas sobre lo que el sistema puede recibir o devolver.

### 4.3. Mecanismos fiables de intercambio

La arquitectura necesita mecanismos de intercambio suficientemente robustos para sostener el caso de uso en el tiempo.

Eso puede implicar mensajería, conectores, servicios intermedios, APIs o combinaciones de varios mecanismos, pero el criterio principal no debería ser lo “moderno” que suene una tecnología, sino su capacidad real para soportar el flujo necesario con estabilidad razonable.

### 4.4. Trazabilidad del recorrido de la información

Debería ser posible entender por dónde ha pasado la información, qué sistema la emitió, qué transformaciones ha sufrido y en qué punto se utilizó.

Esta trazabilidad es importante no solo para depuración técnica, sino también para interpretar errores, revisar comportamientos inesperados y mantener confianza en el flujo de información.

### 4.5. Separación entre sistemas operacionales y servicios de IA

Siempre que sea posible, conviene evitar que los sistemas de IA se conecten de forma demasiado directa o frágil a los sistemas operacionales.

Una capa intermedia, un servicio desacoplado o un mecanismo bien definido de entrada y salida suele ayudar a reducir impacto sobre el entorno clínico, mejorar mantenibilidad y controlar mejor la evolución del sistema.

### 4.6. Capacidad de adaptación sin romper el flujo clínico

Una integración útil no debería quedar bloqueada ante cualquier cambio menor ni requerir rediseños completos para incorporar nuevas necesidades.

La arquitectura de esta capa debería permitir cierta evolución: nuevos consumidores, nuevos mensajes, nuevas reglas o nuevos servicios, sin que cada cambio ponga en riesgo el funcionamiento del circuito clínico principal.

