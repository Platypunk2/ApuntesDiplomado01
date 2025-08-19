#Spark #Streaming #BigData
Spark streaming es la propuesta de Apache Spark para *aplicaciones Near Real Time*. Un ejemplo podría ser una aplicación donde inyectamos datos con [Flume](obsidian://open?vault=Big%20Data%20y%20Machine%20learning&file=Big%20Data%2FFlume%2FIntroducci%C3%B3n%20a%20Apache%20Flume)
==Tiene un enfoque de procesar pequeños *batch* de datos a intervalos regulares de tiempo==
![[Pasted image 20250819120745.png]]

### Como opera Spark Streaming

![[Pasted image 20250819120859.png]]
1. **Stream de datos de entrada**
   - Aquí es donde Spark Streaming recibe datos en tiempo real desde una fuente como Kafka, Flume, sockets TCP, archivos HDFS en streaming, etc.
   - Este flujo es continuo, pero Spark no lo procesa directamente como flujo infinito.
2. **Spark Streaming → conversión a batches**
   - Spark Streaming transforma ese flujo en **micro-lotes (batches)** de datos.
   - Cada batch contiene los datos recibidos en un intervalo de tiempo fijo (ejemplo: cada 2 segundos).
   - Estos batches se representan internamente como **RDDs** (Resilient Distributed Datasets), la estructura de datos básica de Spark.
   - Aquí aparece el concepto de **DStream (Discretized Stream)**:
     ==Un **DStream** es una secuencia de RDDs que representa el flujo de datos en Spark. Es decir, Spark Streaming convierte el stream en una colección de RDDs distribuidos que se procesan uno tras otro.==
1. **Procesamiento con Spark**
   - Una vez que los batches son creados, pasan a Apache Spark (motor principal).
   - Spark ejecuta las transformaciones y acciones definidas por el usuario sobre cada RDD de la secuencia.
   - Ejemplo: map, reduceByKey, count, join, etc.
4. **Batches de salida**
   - Después del procesamiento, los resultados también se generan en forma de batches (RDDs).
   - Estos resultados pueden enviarse a:
     - Bases de datos
     - Dashboards en tiempo real
     - Sistemas de mensajería (Kafka, Flume, etc.)
     - Archivos en HDFS, S3, etc.
   - El resultado sigue siendo un **DStream** (RDD tras RDD en el tiempo).
## Ejemplo
![[Pasted image 20250819122816.png]]
![[Pasted image 20250819122935.png]]
## Inputs
En Spark Streaming se puede recibir, en grandes rasgos, dos tipos de **inputs**: Básicos y avanazdos.
==La diferencia esta en cómo se reciben los datos y el nivel de integración con las fuentes externas.==

### Input básico
Los inputs básicos son las fuentes de datos más simples que Spark Streaming soporta de manera directa. ==Esto puede ser desde abrir un socket o revisar archivos en un directorio==. Spark convierte estos datos en DStreams.
Se llaman básicos porque no necesitan de una integración compleja y ==están más orientados a **testing** o **casos simples** de streaming==.

![[Pasted image 20250819123654.png]]

Los ejemplos típicos son:
- **SocketTextStream** => conecta a un puerto TCP y recibe texto en tiempo real.
- **FileStream** => monitorea un directorio y procesa archivos que se vayan agregando.
- **QueueStream** => recibe RDDs predefinidos (útil para pruebas).

### Input avanzado
En Spark Streaming, los inputs avanzados son fuentes más complejas y de uso real en producción.
Estos son integraciones con **sistemas de mensajería o recolección de datos distribuidos** que ya generan streams
Se llaman avanzados porque requieren configuraciones adicionales (conexión, offsets, tolerancia a fallos, etc.) y, además, estos inputs están pensados para **aplicaciones distribuidas en producción** con grandes volúmenes de datos.

![[Pasted image 20250819123704.png]]

Los ejemplos típicos son:
- **Kafka** => integración con Apache Kafka para consumir mensajes en tiempo real.
- **Flume** => integración con Apache Flume (colector de logs).
- **Kinesis** => integración con AWS Kinesis Streams.
- **MQTT** o **Custom Receivers** => crea tu propio receptor con `Receiver` en Spark.

## Transformaciones y Acciones

Las transformaciones y acciones de **DStream** son similares a transformaciones de RDD. Algunas de las diferencias son como que `reduce` en **DStream** es una transformación y que `sortBy` no esta definido para **DStream**. Para el resto de transformaciones y acciones funcional igual o muy similar.
![[Pasted image 20250819142351.png]]

### Nuevas funciones en Spark Streaming

![[Pasted image 20250819142439.png]]
En **Spark Streaming**, las **operaciones de ventana (window operations)** permiten aplicar transformaciones **sobre un rango de tiempo (ventana)** de un **DStream**, en lugar de hacerlo solo sobre cada micro-batch por separado.
Un **DStream** es una secuencia de RDDs generados cada intervalo de batch (ej: cada 2 segundos).  
Con las **operaciones de ventana**, Spark permite **agrupar varios batches dentro de un rango de tiempo mayor (la ventana)** y procesarlos juntos.

Esto se define con dos parámetros:
1. **Duración de la ventana (window length)** → cuánto tiempo abarca la ventana.
2. **Duración del desplazamiento (sliding interval)** → cada cuánto se mueve la ventana.

#### Ejemplo

- Batch interval = 2 segundos
- Window length = 6 segundos
- Slide interval = 4 segundos

👉 Cada 4 segundos Spark tomará los últimos 6 segundos de datos (3 batches) y aplicará la operación.
![[Pasted image 20250819145957.png]]

## Outputs

![[Pasted image 20250819150300.png]]

# Síntesis

- **Spark Streaming** es un módulo simple y robusto de Spark, para extender su funcionalidad a stream de datos en tiempo "real".
- Cada dato es puesto dentro de una cadena de RDD llamada **DStream**, particionado por tiempo.
- Con pocas líneas es posible transformar una aplicación batch a una streaming.

