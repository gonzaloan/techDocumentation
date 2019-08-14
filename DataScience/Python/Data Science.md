## Python

### Encontrar el promedio de un dato a partir de un dictionary con muchos valores.

```python
sum(float(d['cty']) for d in mpg) / len(mpg)
```
Recorremos los elementos calculando un float a cada uno y sumándolo, después los dividimos por la cantidad de elementos.


### Retornar los valores únicos en un dictionary
```python
datos = set(d['cyl'] for d in mpg)
```
*Esto retorna un set con los valores que no se repiten*

### Contar de 0 a 1000 y mostrar sólo los pares
```python
my_list = [number for number in range(0,1000) if number % 2 == 0]
```
También se puede hacer con numpy:
```python
n = np.arange(0, 30, 2) # start at 0 count up by 2, stop before 30
```

### Lambdas

```python
my_function = lambda a, b, c : a + b
```
*my_function(1, 2, 3) retorna 3*

```python
def split_title_and_name(person):
    return person.split()[0] + ' ' + person.split()[-1]
```
Se puede hacer de dos formas con lambdas:
```python
#option 1
for person in people:
    print(split_title_and_name(person) == (lambda x: x.split()[0] + ' ' + x.split()[-1])(person))

#option 2
list(map(split_title_and_name, people)) == list(map(lambda person: person.split()[0] + ' ' + person.split()[-1], people))
```

### List Comprehensions

Podemos llenar listas con valores de forma más simple, por ejemplo tenemos este código:

```python
my_list = []
for number in range(0,1000):
	if number % 2 == 0:
		my_list.append(number)
my_list
```

Podemos usar list comprehensions para hacerlo en una línea:
```python
my_list = [number in range(0,1000) if number % 2 == 0]
```

Otro ejemplo más complejo:
```python
def times_tables():
    lst = []
    for i in range(10):
        for j in range (10):
            lst.append(i*j)
    return lst
```
Con list comprehensions:

```python
times_tables() == [i*j for i in range(10) for j in range(10)]
```

## Numpy

```python
import numpy as np

mylist = [1,2,3]
#esto crea un arreglo de numpy
x = np.array(mylist)
#podemos crear otro 
y = np.array([4,5,6])

# podemos crear un array multidimensional pasando una lista de listas
m = np.array([[7,8,9][10,11,12]])

#Podemos ver el tamaño fila/columna de un array con la función shape
m.shape #esto retornará (2,3) 2 filas 3 columnas

#Tenemos la función arange, le pasamos un inicio, un final, y un tamaño de paso, y retornará un listado de elementos separados por la misma cantidad entre todos, según el intervalo
n = np.arange(0, 30, 2) #devolverá un arreglo de entre 0 y 30, con valores que van de dos en dos

#podemos volver a rearmar el array según las dimensiones que deseemos, por ejemplo de 3 x 5
n = n.reshape(3,5) #retornará la lista anterior creada separada por 3 filas y 5 columnas
#y podemos hacer un reshape con condiciones
r.reshape(36)[::2] # devuelve array con todos los valores saltandose de dos en dos


#tenemos la función linspace, que es parecida al arange, pero le indicamos el número de valores que queremos que tenga el array.
o = np.linspace(0,4,9) #Retornará nueve valores, de cero al 4, separados por el mismo cantidad.

#imprime un array con nros uno
np.ones((3,2))

#imprime un array con ceros
np.zeros((2,3))

#imprime array con unos en la diagonal
np.eye(3) #3x3

#imprime los datos que se pasan por parámetros en la diagonal
np.diag(y)

# repeat repite cada valor la cantidad que indiquemos
np.repeat([1,2,3],3)

#multiplicar todo los valores por el tipo de dato que queremos
p = np.ones([2,3], int) #crea un array de 2x3 con 1 enteros.

#podemos añadir otro array verticalmente con alguna operación
np.vstack([p, 2*p]) #esto creará un array de 2,3 con 1 enteros, y abajo de ese otro array igual pero cada valor multiplicado por 2

#Lo mismo pero horizontalmente
np.hstack([p, 2*p])


```

#### Operationes con Numpy


```python
# suma de valores de un array
x + y #retorna un array con 5,7,9
x*y
x**2

# crear un array en donde se multiplica otro array con operacion que se desea
z = np.array([y, y**2])
#imprime 4,5,6
#        16,25,36 

#transponer el array
#Si tenemos un array 2x3, con T queda en 3x2
z.T

#sumar valores en un array
a.sum()

#el máx y min
a.max()
a.min()
#promedio
a.mean()
#desv estándar
a.std()
#valo rmáximo 
a.argmax()
#valor minimo
a.argmin()

```

#### Slices

```python
r = np.arange(36)
r.resize((6,6)) #ordena el arreglo de 6x6

#podemos obtener una posición 2 en la fila 2 así:
r[2,2]

#podemos cortar trozos de arreglo
r[3, 3:6] #imprime la fila 3, del indice 3 al 6

#filas de la 0 a la 2, mostrando todas las columnas menos la última
r[:2, :-1]	

#mostrar solo elementos que cumplan alguna condición
r[r>30]

#y podemos modificar todos los elementos que cumplan dicha condición
r[r>30] = 30 #todos los valores mayores a 30 serán cambiados por número 30

#copio los elementos de la fila 0 a 3 y col 0 a 3 en r2
r2 = r[:3, :3]

#modifico todos los valores de esta array a 0 
r2[:] = 0

#Los elementos que tomamos también se modificarán en cero en la array r, para corregir eso, podemos usar la función copy
r_copy = r.copy()

```

#### Iteraciones en arrays

```python

#Creamos una rray con numeros entre 0 a 10, en un array de 4x3
test = np.random.randint(0,10, (4,3))

#imprimir por fila
for row in test:
	print(row)

#imprimir por valor
for i in range(len(test)):
	print(test[i])

#mostrar indice de cada row
for i, row in enumerate(test):
	print('row', i, 'es', row)

#Se puede iterar sobre dos arrays usando zip

test2 = test**2
for i, j in zip(test, test2):
	print(i, '+', j, '=', i+j)
#mostrará la fila del array 1 más fila del array 2 y el resultado de dicha suma de cada valor

```



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTczMDY0MjcyMF19
-->