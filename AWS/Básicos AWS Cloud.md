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




> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbODQzODUyNzY5LDIwMzQ5NDMzOTYsLTE5OT
MzOTA1NzcsLTE2MTM1MjkxMzIsLTEwMzE2ODgwNDksLTExMTc5
OTg3NDUsMTcxNTAwOTY0NSwxNDk2NzI4MDgxXX0=
-->