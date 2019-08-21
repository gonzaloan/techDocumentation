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



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI5NDc1NjA5MSwtMjA2ODA4MDQ2NywtNz
cwOTU2MjA3LC0xODM3NDE4NjI3LDc2NjIyNjMxN119
-->