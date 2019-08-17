
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
-**docker history {ubuntu:gonzo}**: muestra todas las capas de la imagen, y el tamaño de cada capa.

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
- **docker build -t ubuntu:gonzo .** .: Crea imagen, ubuntu:gonzo es el nombre- . El punto final es en dónde se trabajará con el componente. Le da un espacio. para que trabaje ahí y no salga de allí.

Con esto listo, una vez se haga **docker image ls** aparecerá nuestra imagen.
En este caso, si creamos un container con nuestra imagen, lo que se hizo fue levantar una máquina ubuntu y crear un archivo llamado hola-gonzo en la carpeta /usr/src/

#### Publicar la imagen
Ahora podríamos publicar esta imagen. Para ello debo tener un repositorio.
Docker usa el TAG para ello, podemos agregarle un Tag a la imagen así:

**docker tag ubuntu:gonzo gmunoz/gonzo**: Crea un tag gmunoz/gonzo. Es como en github, github/repositorio.

Ahora podemos pushearlo a nuestro repo
**docker push gmunoz:gonzo**


## Docker en Aplicaciones

- Usamos un repositorio tipo Github y Gitlab.
- Dentro del proyecto creamos un Dockerfile

~~~
FROM node:8

COPY [".", "/usr/src/"]

WORKDIR /usr/src

RUN npm install

EXPOSE 3000

CMD ["node", "index.js"]
~~~
-- La imagen base es Node
-- Copy: Partiendo del contexto de build '.' va a copiar todo lo que está en el contexto a usr/src/
-- Workdir: es como un cd, se irá a esa ruta (usr/src)
-- Correr npm install
-- Expose: Contenedor expone el puerto 3000
-- CMD: Cuál es el comando por defecto que correrá el contenedor al ejecutar la imagen.

- Contruimos la imagen con **docker build -t gonzoapp .** (El punto es el contexto): En este punto instalará la imagen de node, y el repositorio, a su vez, correrá la aplicación con npm install y todo lo que especificamos en el Dockerfile
- Corremos la imagen: **docker run --rm -p 3000:3000 gonzoapp**: Esto ejecuta la imagen, desde el puerto 3000 de mi localhost hasta el 3000 del contenedor. A su vez, rm indica que una vez termina de ejercutarse el contenedor lo borre.

### Podemos dejar el servidor escuchando cambios así:

- **docker run --rm -p 3000:3000 -v /Users/gmunoz/Downloads/Project/:/usr/src gonzoapp**: Esto quedará escuchando los cambios de los archivos dentro de la carpeta Project. Algo se cambia en el disco, y al detectar cambios, se van para usr/src


## Conectar Contenedores entre ellos

Para conectar un contenedor con otro, podemos usar **docker network**:
- **docker network ls**: Muestra todas las redes disponibles
-- Bridge: No se usa
-- Host: Forma de representar a la red de mi máquina. Es decirle al contenedor que use la misma red del equipo host. Es peligroso. 
-- 

> Written with [Gonzalo Muñoz](https://github.com/gonzaloan/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1MjQ3OTIwNDMsNDkyODY4MDgzLC0xOD
k0MDk5MjE2LC0xOTk5NjczOTYwLDQzNDE3OTU2NCwtMTQzOTM1
MjM4LDE1NTE0MTI1MTgsLTgxMTcwNDAwMF19
-->