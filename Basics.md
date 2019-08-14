# Django

## Debugger
Se puede instalar un debugger importando la linea
```python
import pdb
```

Y luego en la línea que se desea debuggear, se llama a esa función y método set_trace, lo que abrirá en la consola y se podrá por ejemplo hacer operaciones para traer cabeceras, tipos de peticiones, meta, methods:

```python
pdb.trace()
#Abre consola

request.META
request.method
request.GET
```

## Apps en Django

Una app en Django es un módulo de python que proporciona un conjunto de funcionalidades relacionadas entre si. Las apps son un conjito de models, vistas, urls y archivos estáticos.

Para crear una app en PyCharm se hace **Ctrl + Alt + R ** y se escribe en la línea de comandos : 
```python
startapp {nombre de la app}
``` 
Esto crea una carpeta con el nombre que le dimos a la app con lo indicado anteriormente.

Para agregarla al proyecto actual, debemos ir a **settings.py** y agregarlo en **INSTALLED_APPS**


## Templates

Templates es una forma de crear archivos HTML que sean respondidos por el backend de Django. Para ello, dentro de la app se debe crear una carpeta templates, y luego los archivos html.

Para renderizar un html desde las *views.py*, se usa así: 

```python
def list_posts(request):  
    # List existing posts  
return render(request, 'feed.html')

# y enviar datos así:
def list_posts(request):  
    # List existing posts  
  return render(request, 'feed.html', {'posts': posts})
```
## Base de datos

En settings se pueden configurar diversas bases de datos en las que puede trabajar Django. 

### Migrar datos
```python
python manage.py migrate
```

Para crear Modelos, se usa el archivo **models.py** dentro de las app. Aquí:

```python
class User(models.Model):  
    """User model"""  
  email = models.EmailField(max_length=100, unique=True)  
    password = models.CharField(max_length=100)  
    first_name = models.CharField(max_length=100)  
    last_name = models.CharField(max_length=100)  
    bio = models.TextField(blank=True)  
    birth_date = models.DateField(blank=True, null=True)  
    created_at = models.DateTimeField(auto_now_add=True)  
    modified_at = models.DateTimeField(auto_now=True)
```

Una vez modificada esto, es necesario hacer las migraciones, para ello en la terminal se debe ejecutar 

```
python manage.py makemigrations
#Y ejecutamos las migraciones
python manage.py migrate
```



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc0NjEwNzg4N119
-->