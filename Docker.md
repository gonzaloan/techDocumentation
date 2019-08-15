
## Docker

- **docker run {imagen/contenedor}**: Crea y lanza un contenedor. Puede ser un contenedor, o una imagen, las opciones útiles pueden ser ubuntu, mongo, node, nginx,etc

 - **docker run -d --name {containerName} {container}**: -d deja ejecutando el proceso, --name le asigna un nombre del container. Un ejemplo de este comando: docker run -d --name db mongo: Crea una instancia de linux con mongo corriendo llamada db

- **docker exec -it db bash**: Accede a container db en modo interactivo con bash.

- **docker ps -a**: Muestra todos los contenedores

 - **docker rm {SHA}**: Borra elemento


## Persistencia de Datos en Docker

### Bind mount: 
Podemos también crear un folder en nuestro sistema de archivos, y asignarlos a un contenedor para que pueda usarlos como parte de él:

**docker run --name db -d -v C:/Users/Gmunoz/Folder/mongodata:/data/db/ mongo**: Esto monta el directorio en la carpeta /data/db/

### Volumes

Se guardan todos dentro del filesystem, y pueden ser usados por multiples contenedores:

- **docker volume ls**: Un contenedor en su definición crean volumes. Con ls se listan todos.
- **docker volume prune**: Borra todos los volume
-  **docker volume create {nombreVolumen}**: Crea el volumen
- **docker run -d --name db --mount src=nombreVolumen, dst=/data/db mongo**: Crea un contenedor llamado db, y a su vez monta el *volume* que creamos anteriormente. El destino es donde se hará el mount, en este caso es una base de mongo.


## Imágenes

-**docker pull {nombreImg}**: Trae una imagen. 
-**docker image ls**: Ver todas las imagenes
-**docker pull ubuntu:18.04**: Trae una versión específica de una imagen.

### Crear imagen propia
Se necesita crear un archivo Dockerfile

Estructura del archivo:

~~~docker
FROM ubuntu

RUN touch /usr/src/hola-gonzo
~~~

- FROM: inicio del archivo, indica de qué imagen se usará como base
- RUN: Es lo que se ejecutará.	

Luego, para construir la imagen:
- **docker build -t ubuntu:gonzo**: Crea imagen, ubuntu:gonzo es el nombre

> Written with [Gonzalo Muñoz](https://github.com/gonzaloan/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTczMjI0NzgwNSwtODExNzA0MDAwXX0=
-->