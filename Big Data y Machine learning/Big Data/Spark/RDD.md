#spark

## Ejemplo secuencia funcional Spark

![[Pasted image 20250813184922.png]]

1. **Fuente de datos (HDFS)**
   - El proceso comienza leyendo datos desde **HDFS** (Haddop Distributed File System), aunque en la práctica podría ser también S3, una base de datos o incluso un archivo local.
   - En este momento, Spark crear el primer RDD ($RDD_{1}$) a partir de los datos brutos
2. **$RDD_{1}$  -> $RDD_{2}$  (Primera transformación)**
   - Aquí se aplica una **transformación** (por ejemplo `map()`, `filter()` o `flatMap()`)
   - Esta transformación **no se ejecuta inmediatamente** porque Spark trabaja de forma *lazy* (perezosa).
   - El resultado es un **nuevo RDD ($RDD_{2}$)** que representa la nueva vista de los datos.
3.  **$RDD_{2}$  -> $RDD_{3}$  (Segunda transformación, posiblemente con "shuffle")**
   - En este paso, se aplica otra transformación, y por el diagrama de flechas cruzadas parece una **operación de agrupamiento o reducción (`groupByKey`, `reduceByKey`. `join`)**
   - Este tipo de transformación implica un **shuffle**: los datos se redistribuyen entre nodos para reagruparse según una clave.
   - el resultado es $RDD_{3}$.
4.  **$RDD_{3}$  -> $RDD_{4}$  (Transformación final)
   - Aquí se realiza una última transformación sobre el RDD, preparando los datos para el resultado final
   - Puede ser un formato adecuado para guardarlos o enviarlos a un servicio web.
5. **Acción final (Salida: HDFS o Web)**
   - Finalmente se ejecuta una **acción** (`saveAsTextFile`,`collect`,`count`, etc.).
   - Esta acción **dispara la ejecución real** de todas las transformaciones anteriores.
   - El resultado se guarda en:
     - **HDFS** (almacenamiento distribuido).
     - **Un servicio web** (posiblemente a través de una API o sistema en tiempo real).

Nada se ejecuta hasta que se llama a una **acción**. ([[Lazy Execution]])