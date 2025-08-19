#BigData #Intruducción
El concepto de Big Data no se refiere a una cantidad o volumen absoluto en datos, mas bien, el termino ==Big Data es un concepto relativo== a la capacidad de almacenamiento y procesamiento actualmente.
## Naturaleza de los datos

Actualmente, los datos se caracterizan por ser:

* Masivos:
  El Big Data actual hace referencia a datos en el orden de magnitud de los petabytes, mayor o igual a 200 o 1000 terabytes.
* Heterogéneos:
  Los datos puedes provenir de diferentes dominios como videos, texto, audio, base de datos SQL, etc.
* Distribuidos:
  Por las limitaciones tecnológicas actuales, su almacenamiento y procesamiento debe ser distribuido.
* De alta complejidad:
  Normalmente los datos que obtendremos serán una mezcla entre datos SQL y NoSQL (logs, comentarios en redes sociales, etc).

Debido a la naturaleza actual de los datos, el hardware tradicional no sirve y se requiere de un paradigma distinto que permite

* Aumentar el procesamiento.
* Aumentar el almacenamiento.

## Estrategia de solución

Para enfrentar los desafíos que plantea la naturaleza actual de los datos —masivos, heterogéneos, distribuidos y complejos— no basta con mejorar el hardware tradicional. Se requiere un cambio de enfoque en cómo almacenamos y procesamos la información. A lo largo del tiempo, han surgido distintas estrategias para abordar este problema, desde el uso de supercomputadoras hasta el paradigma moderno del procesamiento distribuido.

### Supercomputadoras hace 30 años

* Almacenamiento y transferencia no eran un problema
* Procesamiento centralizado: 
  Los datos eran almacenados y accedidos de forma local.
* Pocas CPU, pero rápidas (según el estándar comercial de la época).
* Al constar con mas de una CPU, se permitía el procesamiento paralelo a costa de hardware especializado y caro (sistemas diseñados a la medida).
* Foco en alto *throughput* (lograr la ejecución de un gran números de tareas por unidad de tiempo).
### Supercomputadoras en la actualidad

* Procesamiento distribuido, *clusters* basados en hardware de menor costo (*[[commodity hardware]]*).
* Miles de nodos, CPUs y núcleos:
  Escalamiento horizontal.
* Procesamiento altamente paralelo.
* Foco en alto *throughput*, escalabilidad y robustez.


![[Pasted image 20250729192510.png]]

## Como debe funcionar un software especializado en un sistema distribuido

![[Pasted image 20250731170725.png]]
Para que un software o framework funcione en un sistema distribuido debe cumplir y garantizar las siguientes capacidades:

- **Alta capacidad de procesamiento:**
  Un clúster será poderoso, siempre y cuando, las tareas sean coordinadas de forma correcta.
- **Alta capacidad de almacenamiento:**
  Podemos incrementar esta capacidad de forma lineal al agregar más nodos al clúster. El sistema debe reconocer los recursos que tiene para poder distribuir los datos a lo largo de sus unidades de almacenamiento.
- **Comunicación eficiente:**
  Para evitar problemas de comunicación, se debe de diseñar estrategias que eviten al máximo el envío de grandes volúmenes de datos sobre la red interna y no saturar los canales de comunicación.
- **Robustez ante fallas:**
  Para lograr un sistema robusto, será necesario considerar la probabilidad de fallas del hardware, decidiendo el cursos de acción ante pérdida de unidades de almacenamiento y/o procesamiento.

