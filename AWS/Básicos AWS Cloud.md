# Básicos

## Servicios
Son modelos y estrategias de implementación.

### Infraestructura como servicio (IaaS)
Bloques de creación fundamentales para la TI en la nube.
Acceso a características de red, los equipos y espacio de almacenamiento de datos. 

### Plattform as a Service (PaaS)
Elimina a los negocios la necesidad de administrar infraestructura subyacente(hardware y SO) 

### Software as a Service (SaaS)
Proporciona un producto completo que el proveedor del servicio ejecuta y administra.


## Servicios de Informática

- Amazon EC2 : Elastic Cloud server
- LightSail : VPS 
- ECS: Elastic Container Service: Registro de Docker Container. 
- Lambda: Serverles Compute. Gestiona backend code al hacer cosas como subir documentos, o en app actividad


# EC2

- En la creación de una máquina EC2, lo más importante es la configuración del security group:
-- Acá se puede indicar qué puertos están abiertos, y de dónde podremos conectarnos. Por ejemplo desde un SSH y desde la IP que se desee. 

##  Conectarse a EC2 por MobaXterm

- En Amazon, puede extraerse la IPv4 pública:
**3.83.217.146**
- En MobaXTerm creamos una nueva sesión SSH
agregamos la IP. 
- El usuario por defecto de una máquina de amazon es ec2-user. En el caso de Ubuntu el usuario es *ubuntu*. 
- En la pestaña de avanzado, se agrega la key generada por amazon. 
- Es importante saber que se debe controlar lo siguiente:
-- Hacer los update del sistema operativo
-- Hacer respaldos, pueden hacerse periódicamente o según se necesite. También se puede hacer un snapshot y copiarlos en otra máquina. En el menú Actions de Amazon, se puede Crear una imagen. 

## Implementando un proyecto en EC2

- Mediante MobaXterm podemos conectarnos a nuestra máquina.
- Hacemos un git clone del proyecto que queramos instalar: git clone https://github.com/mauropm/aws-platzi-python.git
- Dentro de la máquina, se instalan todas las dependencias, y en este caso se inicia el proyecto con python app.py
- Esto debe levantar la máquina en el puerto 5000.
- Debemos abrir nuestra máquina y conexión 5000 al mundo, para ello: 
-- Es necesario entrar a la consola de AWS, y presionar en security group. Esto abrirá un menú y se debe ver los permisos en **Inbound** (Cómo nos conectaremos a esa máquina).
-- Presionamos editar, y agregamos un CUSTOM TCP RULE. Colocamos el puerto 5000. Y Anywhere (para acceder desde cualquier ip)

## Lambdas Y Serverless

- Lugar donde se ejecutan funciones de código. Viven de manera independiente.
- No hay un servidor como EC2, AWS se encarga de ejecutarlo cuando se necesite. 
- Se pueden programar funciones lambda en NodeJS, Python, Java, C# y Go. 
- Está limitada a 1000 ejecuciones concurrentes. 
- AWS maneja la seguridad. Es aislado.

Para crear un lambda hay cosas útiles, como variables de ambiente:

```python
import os
def lambda_handler(event, context):
    what_to_print = os.environ.get("what_to_print")
    how_many_times = int(os.environ.get("how_many_times"))
    if what_to_print and how_many_times > 0:
        for i in range(0, how_many_times):
            print(f"what_to_print: {what_to_print}.")
        return what_to_print
    return None 
```
Estas variables de ambiente se pueden definir en AWS. En el mismo menú de creación de function.

- Una utilidad para los lambdas sería por ejemplo, apagar instancias a ciertas horas. 

# Elastic Beanstalk

- Administrador de otros recursos, permite obtener balanceador de cargas y tantas instancias EC2 como se indiquen.
- Al crear un un Elastic Beanstalk permite crear una instancia bajo ambientes Python, DOcker, etc.

- Ambiente escala de manera dinámica de acuerdo a varios factores como memoria, latencia, etc.


## Creación de ambiente

Se debe agregar al proyecto a subir unos cambios:
- Crear directorio **.ebextensions**. Dentro del folder van configuraciones:

~~~python
option_settings:

"aws:elasticbeanstalk:container:python":

WSGIPath: application.py
~~~
- Se puede subir zip con el proyecto.
- Se puede configurar


# S3
- Permite guardar archivos en la nube, hacer buckets, y respaldar información.
- Se almacenan en **Buckets** unidades de almacenamiento.
- Una vez creado un bucket, podemos subir muchos archivos.
- En la pestaña **properties** podemos elegir las propiedades del Bucket, podemos subir una página estática, o versionada. 
- Y se puede respaldar los buckets, en otras máquinas o espacios de AWS (**management**)

### Glacier

- Al igual que S3 permite hacer almacenamiento de tipo histórico. 
- Es más económico, pero más lento.

# RDS Aurora

Optimiza el funcionamiento de un motor de base de datos.
Tiene varias opciones de bases de datos: 
Mysql, PostgresQL, Oracle, SQL Server

Crear base de datos:
- Click en RDS
- Create Dabase
- Se puede modificar clave, puertos usados, fechas de respaldo. 

### Migración a RDSPG

En nuestra máquina amzn, por ssh, debemos tener:

- Instalación de postgresql: *yum install postgresql*
- hacemos un wget del archivo con el script: *wget www.blablabla.com/script.sql*
- **psql -h [hostdelabasededatos]  -U [nombreUsuario] [NombreBD]**
- Host de la base de datos se obtiene de la consola de AWS: database-1.cogypn2qn15r.us-east-1.rds.amazonaws.com

# Route 53

- Funcionalidad de Redes. 
- Permite tener un DNS con el que podrás hacer subdominios asignados a instancias y verlos reflejados en segundos.
- Puedes comprar 












> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NzEzMjQxNjMsLTQzMzg5MDYwMCw1MT
EzNDY5MDgsLTIxMDU5NzI3MSwyMDY2NzQ3NTI0LDEzMzUxMTc1
NTYsLTMzMTkyNDY0Niw0MDA1NTg2MTUsOTU2MjMzNjM3LC0yMD
MwNjczNDkwLC0xOTQ0ODMyMzMzLC0xNzMxMjAzMDQzLC0xNTU0
MTQ3MTY0LDUzOTE1MTMxNSwtNDU0MDQ1Nzg3LDE1Njk0NzQwNT
ksNDA4NjU1NDgsLTIwODE1NTYwMTcsLTE3MDczNDIzNCw4NDM4
NTI3NjldfQ==
-->