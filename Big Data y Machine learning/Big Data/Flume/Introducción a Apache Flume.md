#Flume #BigData
Apache **Flume** es una herramienta de **ingestión y transporte de datos distribuidos y en tiempo real**, diseñada principalmente para **recoger, agregar y mover grandes volúmenes de datos de log** (registros) desde múltiples fuentes hacia un sistema de almacenamiento centralizado, como **HDFS (Hadoop Distributed File System)** o **HBase**.

![[Pasted image 20250818142858.png]]

**Flume** funciona como un *buffer* entre los productores de datos y el destino final. Al utilizar un *buffer*, evitamos que un productor sature a un consumidor, sin necesidad de preocuparnos de que algún destino esté inalcanzable o inoperable (por ejemplo, en el caso de que haya caído HDFS), etc.
### Características principales:

- **Recolección masiva de datos:** pensado para manejar datos continuos como logs de servidores, clics en sitios web, eventos de aplicaciones, etc.
- **Arquitectura distribuida y confiable:** permite desplegar múltiples agentes que trabajan en paralelo.
- **Escalable:** se adapta al crecimiento del flujo de datos agregando más agentes o canales.
- **Tolerancia a fallos:** soporta persistencia y recuperación en caso de errores.
- **Extensible:** puedes personalizar fuentes, canales y sumideros según tus necesidades.
## Arquitectura
En **Flume** un agente es la unidad básica de ejecución que se encarga de mover los datos desde una fuente (source) hasta un destino (sink). Los agentes son como **procesos independientes** que corre en una máquina y que actúa como un **trabajdor autónomo** en la tubería de datos.
La arquitectura es sencilla, y se basa en el uso de agentes que se dividen en tres componentes:
- **Source** (fuente):
  Este es la fuente de origen de los datos, ya sea *Twitter, Kafka,* una petición *Http*, etc.
  La componente **Source** de los agentes son activo que recibe datos desde otra aplicación que produce datos. **Source** puede escuchar uno o más puertos de red para recibir y leer datos del sistema de archivos.
  **Source** define el como se inyectan los datos externos dentro del agente.
  ![[Pasted image 20250818202433.png]]
  
  - **Source.interceptor**
	Un **interceptor en Flume** es como un **filtro/modificador inteligente** que actúa **después de que el Source crea un evento, pero antes de que llegue al Channel**, permitiendo transformar, enriquecer o filtrar los datos.
	    Un interceptor puede:
		1. **Filtrar eventos** → eliminar los que no cumplan ciertas condiciones.
		2. **Modificar eventos** → por ejemplo, agregar cabeceras o transformar el cuerpo del evento.
		3. **Enriquecer eventos** → añadir información extra, como timestamps, nombre del host, tipo de log, etc.
		4. **Redirigir eventos** → usar cabeceras personalizadas para que luego distintos sinks los manejen de forma diferente.
	![[Pasted image 20250818203331.png]]
- **Source.selector**
  Un **Source Selector** en Flume es el **mecanismo que controla cómo se enrutan los eventos desde el Source hacia uno o más Channels**. El más común es el **Replicating** (todos los canales) y el **Multiplexing** (dependiendo de cabeceras).
  Cuando un **Source** está conectado a **más de un Channel**, el **Source Selector** define la **política de enrutamiento**:
	- ¿El evento se manda a todos los canales?
	- ¿Se manda solo a uno en particular?
	- ¿Se elige de forma aleatoria, por prioridad, o en base a alguna cabecera del evento?
   ![[Pasted image 20250818203409.png]]
  
- **Channel** (canal):
  Es la vía por donde se tratarán los datos.
  Un canal es un componente pasivo que almacena los datos como un *buffer*. Se comportan como colas, donde los **Source** publican y los **Sink** consumen los datos. Múltiples **Source** pueden escribir de forma segura en el mismo **Channel** y múltiples **Sink** pueden leer desde el mismo **Channel**. Sin embargo, cada **Sink** sólo puede leer de un único canal. Si múltiples **Sinks** leen del mismo **Channel**, sólo uno de ellos leerá el dato
  ![[Pasted image 20250818203446.png]]
- **Sink** (sumidero):
  Este ve la persistencia/movimiento de los datos, a ficheros / base de datos.
  Toma eventos del **Channel** de manera continua leyendo y eliminando los eventos. A continuación, los transmite hacia el siguiente componente, ya sea a HDFS, _Hive_, etc... Una vez los datos han llegado al siguiente destino, el sumidero informa al canal mediante un _commit transaccional_ para que elimine dichos eventos del canal.
  ![[Pasted image 20250818203605.png]]

![[Pasted image 20250818154224.png]]

Una instalación de _Flume_ consiste en una colección de agentes conectados los cuales se ejecutan en una arquitectura distribuida. Los agentes del borde del sistema (situados, por ejemplo, en los mismos servidores web que generan los datos) recogen los datos y los reenvían a los agentes responsables de agregar y ordenar los datos en el destino final.

![[Pasted image 20250818154822.png]]

Los **eventos** son la unidad más pequeña del procesamiento de eventos de *Flume*. Cuando *Flume* lee una fuente de datos, envuelve una fila de datos (es decir, encuentra los saltos de línea) de un evento.
Un evento es una estructura de datos que se compone de dos partes:

- Encabezado (_header_), se utiliza principalmente para registrar información mediante un mapa en forma de clave y valor. No transfieren datos, pero contienen información util para el enrutado y gestión de la prioridad o importancia de los mensajes.
  
  - **Header** => metadata

- Cuerpo (_payload_): array de bytes que almacena los datos reales.
  
  - **Body** => datos

![[Pasted image 20250818155957.png]]


## Ejemplo Flume
![[Pasted image 20250818202329.png]]
![[Pasted image 20250818202351.png]]
