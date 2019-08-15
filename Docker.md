
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


## Imagenes 
> Written with [Gonzalo Muñoz](https://github.com/gonzaloan/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI5MDcxMDIyMywtODExNzA0MDAwXX0=
-->