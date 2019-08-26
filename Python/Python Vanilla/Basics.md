
## VirtualEnv

Creación de virtualenv:

```python
virtualenv {folder}\{nombreapp}
#Ejemplo
virtualenv python/new_app
```
##### Activar venv

Ir a carpeta bin dentro de donde se creó la app
```python
cd python/new_app/bin
source activate
```

### List 
Utilidades de List:

```python
a = [1,2,3]
a.append(4) # Agrega elemento 4
a.count(1) #Cuántos 1 hay en la lista?
a.extend([5,7]) #agrega lista al final de lista 
a.index(3) # Posición del 3 en la lista
a.insert(0,17) # Agrega el 17 en la posición 0
a.pop() # Saca ultimo elemento y lo imprime

```
#### Operaciones comunes

```python
a = [1,2,3]
min(a) #retorna el menor: 1
max(a) #retorna el máximo: 3
sum(a) #Retorna suma de todos los elementos: 6
len(a) # Numero de elementos en la lista
b = [4,5,6]
a+b #COncatena lista a y b
a*2 #imprime 1,2,3,1,2,3
sorted(a) #Ordena lista de menor a mayor
```
#### Ordenar lista de tuplas

```python
a= [(5,3),(1,3), (1,2), (2,-1), (4,9)]

sorted(a) #Ordena primero la tupla 0, y si es el mismo valor, compara por el segundo parámetro de la tupla
sorted(a, key=itemgetter(0)) 
#Ordena sólo por el primer valor de la tupla
sorted(a, key=itemgetter(0,1)) #Igual el el sorted normal
sorted(a, key=itemgetter(1)) #Ordena por el valor 1 de cada tupla.

```

##### List Comprehensions
```python
[x+5 for x in [1,2,3]] 
#imprime 6,7,8
```



#### Dictionary

Formas de crear un diccionario con los valores {'A':1, 'Z': -1}

```python
a=dict(a=1, z=-1)
b={'A':1, 'Z':-1}
c = dict(zip(['A','Z'], [1,-1])
d= dict([('A', 1), ('Z', -1)])
e = dict({'A': 1, 'Z': -1})

#Metodos
d['C']= 5 #Añade un valor c con value 5
len(d) #Cuantos pares hay
d['A'] # Cuál es el valor de a?
del d['a'] #Borrar a

e = dict(('hello', range(5)))
#Imrpimr dictionary de h:0, e:1, l:3, o:4 (el primer L lo sobrescribe)
e.keys() #Imrpime llaves
e.values() # Imprime valores
e.items() #imprime todo

e.popitem() # Saca un item al azar
d.pop('l') #Saca item con el value 'l'
d.pop('not-a-key', 'default-value') #Saca el notakey, si no existe, imprime eldefault value
d.update({'another':'value'})
#Actualiza elemento
d.update(a=13) #también
d.get('a') #obtiene
```
### Otros tipos de datos de contenedores

- **namedtuple()**: 

```python
from collections import namedtuple
vision = namedtuple('Vision', ['left', 'right'])
vision = Vision(9.5, 8.8)
vision[0] #9.5
vision.left # 9.5
vision.right #8.8
```

- **defaultdict**: 

```python
#dictionario normal
d = {}
d['age'] = d.get('age', 0) +1 #Si no hay edad, el default es 0 y se suma 1
#con default dict
from collections import defaultdict
dd = defaultdict(int)
dd['age'] += 1
dd #daría 1

```
- **chainMap**

Sirve para unir grupos de maps

```python
from collections import ChainMap
default_connection = {'host': 'lo
```


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ1NTg1NzI0NiwxNTU0NDIwNDM0LC0yMD
c0NTA2OTU0LC03NTEwMDM4NjUsMTA4MzIwNDIyMV19
-->