#spark 

La **lazy execution** (o ejecución perezosa) en Apache Spark significa que las **transformaciones** sobre los datos **no se ejecutan inmediatamente**.  
En lugar de eso, Spark **registra un plan** de lo que debe hacer (un _DAG_, Directed Acyclic Graph) y solo ejecuta el trabajo cuando se llama a una **acción**.

Esto permite a Spark **optimizar el flujo completo** antes de procesar, evitando pasos innecesarios y reduciendo el número de lecturas/escrituras.

## Ejemplo en PySpark

```python
from pyspark import SparkContext

sc = SparkContext("local", "LazyExecutionExample")

# 1. Crear un RDD a partir de una lista
rdd = sc.parallelize([1, 2, 3, 4, 5])

# 2. Aplicar transformaciones (NO se ejecutan todavía)
rdd2 = rdd.map(lambda x: x * 2)   # Multiplica por 2
rdd3 = rdd2.filter(lambda x: x > 5)  # Filtra mayores a 5

# Hasta aquí Spark no ha procesado ningún dato

# 3. Acción: dispara la ejecución real
result = rdd3.collect()  # Aquí Spark procesa todo de golpe

print(result)

```

### **Qué pasa aquí:**

1. **`parallelize`** crea el primer RDD.
2. `map` y `filter` son **transformaciones** → Spark las **registra**, pero no las ejecuta.
3. Cuando se llama a `collect()` (una **acción**), Spark:
   - Optimiza el plan.
   - Lee los datos.
   - Aplica **ambas transformaciones en una sola pasada**.
4. Devuelve `[6, 8, 10]` como resultado.

![[Pasted image 20250814182054.png]]
