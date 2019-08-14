## Algoritmo K-means

Es una tecnica de clusterización basado en Aprendizaje no supervisado, permitiendo agrupar elementos similares. Por ejemplo, segmentar clientes de una empresa en función a números de productos que compran, el valor de quantía que gastan en compra, la zona, etc. Todo esto para dirigirnos a sectores específicos, y conocer clientes.

Objetivo es agrupar en diferentes clústers la información y diferenciarlos unos de otros.

### Cómo funciona

Imaginemos que tenemos un conjunto de datos con series de variables, por ejemplo, tenemos un montón de individuos y sus alturas y pesos.
Debemos:

- Selección del número de clústers: En cuántos grupos queremos agrupar la información? 2, 3, x subconjutos. Para esto, se usa el método elbow, se toma el conjunto de datos y hacemos el kmeans. Esto hará una gráfica y se toma el codo.
- Calculamos la distancia entre todos los puntos al centro del clúster. Ver distancia de lejanía de cada clúster.
- Asociar cada punto al clúster más cercano
- Recalcular de nuevo el centro de los clústers a partir de los puntos que lo componen.
- Repetir el proceso iterativamente hasta que algoritmo converge, esto es cuando la suma de los centros no varía casi nada en el tiempo.

En *R* usamos la función Kmeans. 


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzNzM5Nzg3NzNdfQ==
-->