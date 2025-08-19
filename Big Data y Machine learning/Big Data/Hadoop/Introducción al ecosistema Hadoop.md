#Hadoop #BigData #Intruducción
Apache Hadoop es un *framework* de software de código abierto que permite el **procesamiento distribuido de grandes volúmenes de datos** a través de clusters de ordenadores utilizando modelos de programación simples.
# La necesidad de Hadoop en el Big Data

Antes del auge del Big Data, los datos solían almacenarse en bases de datos relacionales (RDBMS), y los volúmenes eran lo suficientemente manejables para procesarlos en un solo servidor.
Sin embargo, a medida que:

* Internet creció exponencialmente
* Las redes sociales, dispositivos IoT, logs de servidores, etc., comenzaron a generar **volúmenes masivos de datos (terabytes, petabytes y más)**
* Se necesitaba procesar estos datos rápido y eficientemente

Las bases de datos tradicionales **dejaron de ser suficientes**. Eran costosas, difíciles de escalar horizontalmente (añadir más máquinas), y no estaban diseñadas para procesamiento paralelo a gran escala.
## Enfoque de Hadoop

Hadoop se diseñó con los siguientes principios clave:

- **Escalabilidad horizontal:**
  En vez de usar una máquina cada vez más potente, Hadoop permite distribuir el procesamiento entre muchas máquinas económicas.
- **Tolerancia a fallos:**
  Los datos se replican automáticamente y las tareas fallidas se reintentan sin intervención manual.
- **Procesamiento cercano al dato:**
  En lugar de mover grandes volúmenes de datos a donde está el procesamiento, Hadoop **lleva el procesamiento a donde están los datos**.
- **Procesamiento masivo en paralelo (MPP):**
  Miles de tareas pueden ejecutarse simultáneamente sobre un dataset.
# Ecosistema Hadoop

Al referirnos a Hadoop se hace referencia al ecosistema Hadoop...
![[Pasted image 20250801153537.png]]

Este es un conjunto de herramientas de software que, por un lado, se encargan de mantener operativo el sistema distribuido y, por otro, de proporcionar interfaces de programación que permitan al usuario implementar procesos para el análisis de datos.

El sistema Hadoop esta compuesto por:
- **[[HDFS]] (Hadoop Distributed File System)**
  Sistema de archivos distribuido que almacena grandes cantidades de datos dividiéndolos en bloques y replicándolos entre nodos del cluster.
- **[[YARN]] (Yet Another Resource Negotiator)**
  Administrador del sistema de gestión de recursos y planificación de tareas. Este administrador de recursos del cluster es independiente a la aplicación.
- **Motores de procesamiento distribuido:**
  - **Pig** - Scripting
    Lenguaje de alto nivel para tareas de procesamiento.
  - **Mahout** - Machine Learning
  - **R Connectors** - Statistics
  - **Hive** - SQL Query
    Consulta tipo SQL sobre Hadoop.
  - **Flume** - Log Collector
    Ingesta de datos.
  - **Sqoop** - Data Exchange
    Transferencia de datos entre Hadoop y RDBMS (Relational DataBase Management System)

## Beneficios perseguidos en su diseño

Hadoop proporciona:
- **Eficiencia:** Ya que permite el procesamiento de grandes volúmenes de datos en tiempo razonables. 
  Para lograr la eficiencia, utiliza los datos de manera local. Esto significa que cada nodo del clúster procesara los datos que tenga almacenados dentro de sus propias unidades de almacenamiento evitando al máximo la transferencia de datos a través de la red.
- **Escalabilidad:** Permitiendo mejorar las capacidades computacionales agregando nuevas máquinas al clúster.
  Proporciona un mecanismo para la incorporación de nodos de manera muy sencilla y sin afectar el funcionamiento y estado del resto de los nodos dentro del clúster.
- **Confiabilidad:** Altamente confiable debido a los mecanismos de tolerancia a fallas vienen incorporados en su diseño. Esto se logra implementando replicación de datos dentro del clúster.

### Estrategia para la Escalabilidad y Eficiencia

==El enfoque tradicional== en el procesamiento de datos en sistemas distribuidos consistía en definir un nodo de procesamiento maestro y múltiples nodos de procesamiento esclavos. Bajo este esquema, cada vez que se necesite procesar los datos, es necesario moverlos de las unidades esclavas hacia la unidad de computa maestra.
Si se trabaja con pocos datos, esta metodología no implica una gran dificultad, pero, si estamos trabajando con Big Data aparecerán problemas asociados ala lentitud de transferencia, problemas en la red y la necesidad de tener una alta capacidad de almacenamiento en el nodo de computo.

==El enfoque de Hadoop== busca mover el computo a los datos. Bajo este enfoque, cada unidad esclava procesara los datos almacenados de forma local según las tareas asignadas por el nodo maestro y, posteriormente, los resultados parciales serán agregados por el nodo maestro para obtener el resultado final.
Con esta estrategia, la necesidad de transferir datos es minimizada con fuerza y no es necesario imponer requisitos de almacenamiento adicionales para ningún nodo del clúster.
**Se cumple la escalabilidad horizontal.**

### Confiabilidad: Replicación de datos

Se utiliza un sistema de replicación de datos. Cada vez que copiamos un archivo al sistema, estos son particionados en bloques de igual tamaño y luego distribuidos por los distintos nodos del clúster. Cada uno de los bloques copiados son replicados en otros nodos siguiendo una política de almacenamiento redundante.

Puede existir dos principales fallas que pueden dar con la pérdida de un nodo:
- Falla en los canales de comunicación. Si a un nodo le fallan los canales que lo conectan a la red, quedara totalmente aislado.
- Un nodo puede fallar internamente como puede ser en sus unidades de almacenamiento (puede provocar la pérdida total de los datos almacenados en el nodo) o fallos momentáneos de sistema operativo.

Pese a las fallas dichas, estas no deben de inhabilitar el sistema en su conjunto, ya que deberían de existir replicas de los nodos fallidos en otros nodos activos.
