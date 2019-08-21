## Data Munging

Proceso en el cual ya se obtiene los datos, a partir de Scrapping, o bases de datos (RDBMS o NoSQL) y se debe manipular dicha información, ya que seguramente, no está lista para ser utilizada para análisis y/o experimentación.
La finalidad es limpiar esa data, y manipularla para convertirla en algo que se pueda digerir, como Datasets, 

Una vez se tiene definido el dataset, se abre una nueva fase, se debe observar la data, se desarrolla y prueba la hipótesis en un loop recurrente. Luego de todo esto, se crea un modelo. 

El proceso de Data Science puede darse con el siguiente acrónimo
**OSEMN(Observe, Scrub, Explore, Model, iNterpret)**. El munging corresponde a segunda etapa, de Scrub. 

### Lectura de CSV

Para lectura de CSV, podemos usar PANDAS. Esto leerá información y la almacenará en un formato especial de pandas, indexará cada fila, inferirá cada tipo de dato, convertirá data (Si es necesario), parseará datos, los campos vacíos y erróneos.

Para leer un dataset, por ejemplo IRIS:

```python
import pandas as pd
import urllib

url = "http://aima.cs.berkeley.edu/data/iris.csv"
set1 = urllib.request.Request(url)
iris_p = urllib.request.urlopen(set1)
iris = pd.read_csv(iris_p, sep=',', decimal='.', header= None,
              names = ['sepal_length', 'sepal_width', 'petal_length', 'petal_width', 'target'])
```
Para leer un archivo, se debe especificar el caracter separador **sep=**, si hay una cabecera en el dataset **(header)**, y los nombres de las variables, usando **names** y una **list**.  El campo **decimal** tienen valores por defecto, y puede ser obviado. 

El resultado de iris, es nuestro dataframe. 

```python
iris.head() # ver los primeros 5 elementos
iris.tail() #Ultimos 5 elementos	
iris.head(x) #X es el numero elementos que queremos mostrar. iris.head(2) muestra los primeros 2
iris.columns # Ver los nombres de las columnas
iris['target'] #target es el nombre de la columna, muestra todos los elementos de dicha columna
iris[['sepal_length', 'target']] # ver columnas sepal_length y target



```

### Trabajando con DataSets problemáticos

Al tener dataset que tienen datos faltantes, por ejemplo:

```markup
Date,Temperature_city_1,Temperature_city_2,Temperature_city_3,Which_destination
20140910,80,32,40,1
20140911,100,50,36,2
20140912,102,55,46,1
20140912,60,20,35,3
20140914,60,,32,3
20140914,,57,42,2
```

Al leerlo mediante pandas, podemos ver que la estructura resultante rellenó los datos faltantes con NaN, 

Para parsear los datos, podemos leer el csv con una nueva sintaxis:

```python
fake_dataset_2 = pd.read_csv('filename.csv', parse_dates=[0])
```

#### Ahora, podemos reemplazar los Nan por un valor más legible, en este caso que sean 50 ° F el default:
```python
dataset.fillna(50) #reemplazo todos los NaN por 50
```
Ojo que esto sólo muestra los datos parseados, para guardarlos en el dataset, se debe enviar el parámetro **inplace=True**.

#### Un approach común es reemplazar los NaN por la mediana o la media:

```python
dataset.fillna(dataset.mean(axis=0))
```
*axis=0 indica que el calculo de la media se extenderá a las filas, así que los datos obtenidos están derivados de cálculos en las columnas*

### Trabajando con DataSets gigantes

SI el dataset a cargar es muy grande para caber en memoria, se puede usar un algoritmo de machine learning de batch, que trabaja sólo con parte del dataset a la vez. Además, es buena idea si sólo necesitamos una parte de él. En Python, se puede cargar la data en chunks:

```python
iris_chunks = pd.read_csv(filename, header=None, names= ['C1','C2', 'C3', 'C4', 'C5'], chunksize = 10)
#Con esto de debe iterar los chunks ahora:
for chunk in iris_chunks:
	print('Shape: ', chunk.shape
	print(chunk, '\n')
```
Esto imprimirá 15 chunks de 10, 5 cada uno.

Y también al leer el csv, se puede especificar un iterador, y luego solicitar el chunk requerido:

```Python
iris_iterator = pd.read_csv(iris, header=None, names = ['C1','C2','C3','C4','C5'], iterator = True)
iris_iterator.get_chunk(10).shape #(10,5)
iris_iterator.get_chunk(20).shape #(20,5)

iris_iterator.get_chunk(2) #imprime data set de dos valores
```

En el primer get_chunk, traemos un trozo del dataset de 10 líneas, luego de 20 líneas y finalmente, traemos un trozo de dos líneas.

### Trabajando con bases de datos

En este caso usamos SQLITE, creamos una nueva tabla y base de datos:
```python
import sqlite3

drop_query = "DROP TABLE IF EXISTS temp_data;"
create_query = "CREATE TABLE temp_data (date INTEGER, city VARCHAR(80),  temperature REAL, destination INTEGER);"
connection = sqlite3.connect('example.db')
connection.execute(drop_query)
connection.execute(create_query)
connection.commit()
```
 En este punto, creamos la base de datos con la tabla temp_data
 
```python
# Llenamos con algunos datos
data = [(20140910, "Rome",   80.0, 0),
            (20140910, "Berlin", 50.0, 0),
            (20140910, "Wien",   32.0, 1),
            (20140911, "Paris",  65.0, 0)]
insert_query = "INSERT INTO temp_data VALUES(?, ?, ?, ?)"
connection.executemany(insert_query, data)
connection.commit()
```
Llenamos con algunos datos.

##### Podemos leer estos datos en un dataset muy fácil:

```python
#Leemos datos

selection_query = "SELECT date, city, temperature, destination FROM temp_data WHERE Date =20140910"
retrieved = pd.read_sql_query(selection_query, connection)
```
Esto retorna un data set con nuestra información.

### Trabajando con HDF5

Es un formato de archivo que permite arreglar 	el almacenamiento de datos de manera jerárquica, que permite guardar arreglos multidimensionales de tipo o grupo homogéneo que contienen arrays y otros grupos.

```python
storage = pd.HDFStore('example.h5')
storage['iris'] = iris
storage.close()
```
Cuando se quiere sacar datos de un h5, puedes ver todos los keys de un storage así:
```python
storage = pd.HDFStore('example.h5')
storage.keys()
```
Luego de esto, se selecciona el que se quiere obtener 

```python
#Extraer info
fast_iris_upload = storage['iris']
```

## Uniendo DATA con Pandas

Finalmente, los DataFrames pueden ser creados al unir series u otro data en listas. 

```python
my_own_dataset = pd.DataFrame({
    'Col1': range(5),
    'Col2':[1.0]*5,
    'Col3':1.0,
    'Col4':'Hello World!'
})
my_own_dataset
```

Para unir un dataset ya existente, se debe usar **concat**, útil para Series y DataFrame. El axis igual a 0 es para apilar como fila, y el 1 para columnas

```python
col5 = pd.Series([4,3,2,1,0])
col6 = pd.Series([0,0,1,1,1])
a_new_dataset = pd.concat([col5, col6], axis= 1, ignore_index=True, keys=['Col5','Col6'])
a_new_dataset

#UNimos los dos últimos
my_new_dataset = pd.concat([my_own_dataset, a_new_dataset], axis=1)
my_new_dataset
```

En alguna ocasión será necesario unir distintos Series y DataFrame en columnas específicas, o Series de columnas. En ese caso es útil usar **merge**.

Para un ejemplo tenemos esta tabla de referencia:

```python
key = pd.Series([1, 2, 4])
    value = pd.Series(['alpha', 'beta', 'gamma'])
    reference_table = pd.concat([key, value], axis=1,
ignore_index = True,
                                keys=['Col5', 'Col7'])
    reference_table
```
El merge se opera seteando el operador **how** a **left**, generando un SQL left inner join. También puede ser **right**, **outer** e **inner**. 

```python
my_new_dataset.merge(reference_table,
                         on='Col5', how='left')
```
Esto hizo un inner join de la columna 5. Es decir, unió la columna 7, y dependiendo del valor de la Col5 de reference_table, indico qué valor debía ser Col7

![Resultado](data:image/png;base64,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)

Podemos llamar a **dtypes** en un dataset, para ver los tipos de datos de cada columna.  Podemos además, reasignar data de las columnas

```python
my_own_dataset['Col1'] = my_own_dataset['Col1'].astype(float)
```

## Preprocesamiento de datos

Ahora se empieza a manipular los datos.

Primero, si se necesita aplicar una función a una sección limitada de filas, se puede usar una **mask**. Esto es una serie de valores Boolean que indican si la línea está seleccionada o no.

Por ejemplo, queremos obtener en iris todos los valores cuyo *sepal length* sea mayor a 6

```python
mask_feature = iris['sepal_length
```

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjU1Mzg0MjcxLDcwMDg3MjE3OSwtMjA2OD
A4MDQ2NywtNzcwOTU2MjA3LC0xODM3NDE4NjI3LDc2NjIyNjMx
N119
-->