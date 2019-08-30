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

Para crear un lambda ha



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUzOTQ0MTgzMyw1MzkxNTEzMTUsLTQ1ND
A0NTc4NywxNTY5NDc0MDU5LDQwODY1NTQ4LC0yMDgxNTU2MDE3
LC0xNzA3MzQyMzQsODQzODUyNzY5LDIwMzQ5NDMzOTYsLTE5OT
MzOTA1NzcsLTE2MTM1MjkxMzIsLTEwMzE2ODgwNDksLTExMTc5
OTg3NDUsMTcxNTAwOTY0NSwxNDk2NzI4MDgxXX0=
-->