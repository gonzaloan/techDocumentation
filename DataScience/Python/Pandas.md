## Pandas


### Series

Estructuras core de los pandas, mezcla entre una lista y un diccionario.  La primera columna es el index, y el segundo es el valor. 


```python
animals = ['Tiger', 'Bear', 'Moose']
pd.Series(animals)
```
Retorna una serie con todos los elementos. Panda almacena los datos según la forma que requiera

Si le pasamos un diccionario convierte al index en el primer valor, y el segundo en el país:

```python
sports = {'Archery': 'Bhutan', 'Golf': 'Scotland', 'Sumo' : 'Japan', 'Taekwondo': 'South Korea'}

s = pd.Series(sports)

```

#### Peticiones a una Series y DataFrames

Hay muchas formas de obtener datos 
```python
sports = {'Archery': 'Bhutan', 'Golf': 'Scotland', 'Sumo' : 'Japan', 'Taekwondo': 'South Korea'}

```

- Dictionary Like

```python

sports['Golf'] #regresa un dataseries
df[['col1', 'col3']] #regresa un dataframe

```
- Numpy Like
```python

frame_test3.iloc[:] #fila
df.iloc[:, :] #fila y columna

```

- Label Based

```python
#Ver qué país practica Golf
frame_test3.loc['Golf']
df.loc[:, :] #fila, col
```

Se puede sumar todos los valores de una serie con numpy

```python
import numpy as np

total = np.sum(s)
```

Se puede ver cuánto demora algun proceso usando funciones mágicas
Por ejemplo, veremos si es más rápido el sum de numpy o una función iterativa que sume el resultado. Para ello tenemos una serie de 10000 elementos:

```python
s = pd.Series(np.random.randint(0,1000,10000))
s.head() #Muestra primeros 5 elementos

#verificamos cuándo demora, le decimos que  lo ejecute 100 veces
%%timeit -n 100
summary = 0
for item in s:
	summary += item
```
Esto en promedio da 2.44 por loop

*timeit*ejecutará la tarea un par de veces y calculará el promedio de cuánto demora en ejecutarse

```python
%%timeit -n 100
summary = np.sum(s)

```
82, nanosegundos. Es una diferencia gigante.

Podemos sumar valores a las series de manera fácil. Por ejemplo, sumamos 2 a cada valor de la serie:
```python
s += 2
```
Esto es mucho más rápido que de la forma iterativa:
```python
for label, value in s.iteritems():
	s.set_value(label, value+2)
	
```

### DataFrame

Son estructuras con más columnas

```python
purchase_1 = pd.Series({'Name': 'Chris',
'Item Purchased': 'Dog Food',
'Cost': 22.50
})

purchase_2 = pd.Series({'Name': 'Kevin',
'Item Purchased': 'Kitty Litter',
'Cost': 2.50
})
purchase_3 = pd.Series({'Name': 'Vinod',
'Item Purchased': 'Bird Seed',
'Cost': 5.00
})

df = pd.DataFrame([purchase_1, purchase_2, purchase_3], index = ['Store 1', 'Store 1', 'Store 2'])
df.head

```
Esto retorna una tabla ordenada con todos los valores que obtuvimos.

Podemos extraer data con los iloc y loc atributos.
```python
#Traer todos los datos de la tienda 2
df.loc['Store 2']

#Traer de la tienda 1
df.loc['Store 1']

#O mostrar diferentes columnas 
df.loc['Store 1', 'Cost']

#Traer todos los valores, con columnas específicas
df.loc[:, ['Name', 'Cost']]

#crear nueva columna
df['Location'] = None
```

#### DataFrame indexing and loading

```python
df = pd.read_csv('olympics.csv')

#Podemos decirle qué queremos que sea la cabecera de la tabla y la columna/fila
df = pd.read_csv('olympics.csv', index_col=0, skiprows=1)
df.head()
```
Esto muestra una tabla con los datos. 
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTcyNDQwMjM2M119
-->