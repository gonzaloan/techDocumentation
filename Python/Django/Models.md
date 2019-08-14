## Models En Django



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

## Admins en Django

En urls.py puedes agregar el path para ver la pantalla de los admin:
```python
from django.contrib import admin

urlpatterns = [  
    path('admin/', admin.site.urls),   
]
```

Y podemos crear un superuser desde la consola de Django:

**python manage.py createsuperuser**

## Usuarios en Django

Django trae usuarios normales por defecto. se encuentran en *django.contrib.auth.models*.
Podríamos crearlos de la siguiente manera:

```python
from django.contrib.auth.models import User

u = User.objects.create_user(username='Pedro', password='admin123')
```
Esto devuelve el usuario creado con usuario encriptado y varios campos más. Si ese modelo no es lo que buscas puedes extenderlo para agregarle campos necesarios

Creamos la app users, que se encargará de la gestión de usuarios, luego de eso, en esa app agregamos nuestro model en models.py:

```python
class Profile(models.Model):  
    """Profile Model  
  
 Proxy model that extends the base data with other information. """  
  #Seteamos una relación de uno a uno entre hacia User, on_delete puede ser CASCADE, PROTECT, SET_NULL, SET_DEFAULT, entre otros  
  user = models.OneToOneField(User, on_delete=models.CASCADE)  
  
    website = models.URLField(max_length=200, blank=True)  
    biography = models.TextField(blank=True)  
    phone_number = models.CharField(max_length=20, blank=True)  
    picture = models.ImageField(upload_to='users/pictures', blank=True, null=True)  
    created = models.DateTimeField(auto_now_add=True)  
    modified = models.DateTimeField(auto_now=True)  
  
    def __str__(self):  
        return self.user.username
```

Profile extenderá de User por defecto de Django y agregará nuevos campos. Ahora, para acceder a este Profile desde el admin de django debemos registrar el modelo en admin.py de users:

```python
""" User Admin Classes"""  
  
# Django  
from django.contrib import admin  
# Models  
from users.models import Profile  
  
# Register your models here.  
admin.site.register(Profile)
```

Ahora si vamos al admin (localhost:8000/admin) podemos modificar el Profile y agregar nuevos usuarios. 

### Funcionalidad de Login

```python
from django.shortcuts import render, redirect  
from django.contrib.auth import authenticate, login, logout  
from django.contrib.auth.decorators import login_required  
  
# Models  
from django.contrib.auth.models import User  
from users.models import Profile  
  
# Exceptions  
from django.db.utils import IntegrityError  
def login_view(request):  
    """Login view"""  
  if request.method == 'POST':  
        username = request.POST['username']  
        password = request.POST['password']  
        user = authenticate(request, username=username, password=password)  
        if user:  
            login(request, user)  
            #Redirección  
  return redirect('feed')  
        else:  
            return render(request, 'users/login.html', {'error': 'Invalid Username and password'})  
  
    return render(request, 'users/login.html')

```

### Funcionalidad de Sign Up

```python
def signup_view(request):  
    """Signup View"""  
  if request.method == 'POST':  
        username=request.POST['username']  
        password = request.POST['password']  
        password_confirmation = request.POST['password_confirmation']  
        first_name = request.POST['first_name']  
        last_name = request.POST['last_name']  
        email = request.POST['email']  
        if password != password_confirmation:  
            return render(request, 'users/signup.html', {'error': 'Password confirmation doesnt match'})  
  
        try:  
            user = User.objects.create_user(username=username, password=password)  
        except IntegrityError:  
            return render(request, 'users/signup.html', {'error': 'User already taken'})  
        user.first_name = first_name  
        user.last_name = last_name  
        user.email = email  
        user.save()  
        # Crear un profile  
  profile = Profile(user= user)  
        profile.save()  
  
        return redirect('login')  
    return render(request, "users/signup.html")
```
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ0MDQwOTI1Ml19
-->