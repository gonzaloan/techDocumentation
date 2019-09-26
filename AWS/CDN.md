# CDN

Red de servidores con copias de información para que cargue más rápido en el dispositivo de cómputo del usuario. Puede ser video, audio, imágenes, etc.

## VPC
Es Virtual Private Cloud, permite crear una red virtual que contiene todas las instancias totalmente fuera de internet. 

Funciona así: Se tiene por un lado cualquier dispositivo conectado a internet, el cual se conecta a la red de AWS y a su vez dentro de esta red, viven diferentes VPC. 

- Se pueden asignar IPs estáticas 'internas'. 
- Se puede asignar una dirección IPv6 o IPv4
- Se puede asignar múltiples IPs a una instancia.
- Se puede cambiar los grupos de seguridad en vivo.
- Control fino sobre el tráfico saliente.
- Controles de seguridad extras a nivel de red


Una vez tenemos una VPC creada (desde la consola AWS), podemos relacionar una EC2 a esa VPC, aparece en las opciones de la instancia. 

## Jumpbox

Permite tener acceso a los VPCs que se hayan creado, ya que éste tiene una IP pública que se puede usar para conectarse desde afuera.

Para crear una Jumpbox, se crea una instancia EC2, con el VPC que estemos usando. En las configuraciones de la instancia, indicamos que tenga acceso público (Enabled). Esta instancia se creará con una ip pública, permitiendo que accedamos a la máquina por SSH.

- Una vez entramos a la máquina, podemos obtener la IP privada de la máquina privada que creamos, tipo: *10.0.0.27*
- Copiamos el testkey.pem en la máquina y le cambiamos los permisos: *ssh -i testkey.pem ubuntu@10.0.0.27*

## CloudFront

Es la implementación de CDN de AWS. Esto es una red de distribución de contenido que crea réplicas del archivo en diferentes partes del mundo. CloudFront tiene diferentes centros de replicación intentando cubrir gran parte del globo. 

Por ejemplo para un archivo de video, sería así: El archivo es enviado a un Storage S3 y pasaría a un conversor de archivos AWS Elemental MediaConvert que crearía copias del archivo con diferentes tamaños y calidades. Después de esto, se notifica a CloudWatch que redirecciona de nuevo a los Amazon S3 en las diferentes locaciones. 

### Preparando CloudFront

Para ello podríamos primero crear un bucket de S3 para posteriormente subirlo a los servicios de CloudFront

- Creamos un Bucket S3 público
- Subimos un Archivo que queremos compartir.
- Es necesario que el archivo sea público, que pueda ser leído por cualquier persona. 
- Tomamos el link de nuestro contenido: *https://testaws-cf.s3.amazonaws.com/tumblr_nvdi97mmM91sr7u1yo1_r2_250.gif*
- Abrimos el módulo **CloudFront** de AWS
- Creamos una distribución. Se debe poner el origen de la URL que queremos compartir, por ejemplo nuestro S3 creado y el nombre del archivo. Desde este menú también se puede configurar los niveles de acceso.
- En el apartado de Distribution Settings, podemos seleccionar en qué redes se publicará el contenido. 

## Route 53 

Es el servicio de Amazon de nombres de dominios donde puedes no sólo configurar los dominios que ya tengas, sino comprar, crear subdominios, cambiar dominio a distintas implementaciones y más.

Cuando se tiene un nombre de dominio o subdominio, éste apunta a algún endpoint de Amazon como un balanceador de carga y de ahí irá a algún proyecto específico como EC2 o un contenido HTTP.

## API Gateway 

Es un endpoint público abierto a internet que recibe peticiones de tipo HTTP. Si la petición está en caché se regresará, sino se conectará con algún sistema como EC2, lambda, etc. 
Cuando algo no está en caché el API Gateway lo notará y redireccionará a través de Route 53 llevando a diferentes servicios como funciones lambda, servicores EC2 o Elastic Beanstalk.

### Arquitecturas de API Gateway:

- Cliente Web -> Amazon API Gateway -> Elastic Load Balancer -> Amazon ECS Cluster


- Usuario -> API Gateway -> Lambda -> Dynamo DB
- Usuario -> Amazon S3 (Contenido Estático) -> (Si es dinámico) Amazon API Gateway -> AWS Lambda -> Amazon DynamoDB

Por ejemplo, para crear una API Gateway con Lambda:

- Creamos una función Lambda desde cero con permisos.
- Una vez creada, debemos dar en **add trigger**. Aquí seleccionamos **API Gateway**.
- En las configuraciones de la API Gateway, seleccionamos *Create a new API*.
- Esto creará un Endpoint de la API del tipo *https://j4z2c6fgrg.execute-api.us-east-1.amazonaws.com/default/testAPIGateway*

### API Gateway + Servidor EC2

Si necesitamos tener nuestro server EC2 con API Gateway, se puede de la siguiente manera:

- Abrir en la consola de AWS: API Gateway
- Crear una API nueva
- En la nueva API, creamos un *new Child Resource*, de tipo **proxy**. 
- Al abrir el nuevo Proxy en el menú, seleccionar **HTTP Proxy**. Y copiamos el endpoint de nuestra instancia EC2. *Ej:http://54.149.101.146/{proxy}*
- Actions/Deploy API: Esto dará como resultado una dirección a través de API Gateway. 


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjY5NjQ3XX0=
-->