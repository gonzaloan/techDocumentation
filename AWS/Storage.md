## Storage

### Características de S3

- Es almacenamiento de objetos (Es como un Dropbox). Hay tipos S3, S3 IA(Acceso poco frecuente), S3 One Zone(Poco frecuente de una Zona) y Glacier.
- Características Alta durabilidad y disponibilidad, dependiendo del tipo.
- **Bucket**: Unidad en la que se almacena información en S3. *htts://us-west-1.amazonaws.com/[nombre_bucket]*
-- *us-west-1*: Región.

- **Objeto**: Se almacenan dentro del bucket: *https;//us-west-1.amazonaws.com/[nombre_bucket]/doc1.pdf*.
- **Web estática**: *https://[nombre_bucket].s3-website-us-west-1.amazonaws.com*

### Object Level Logging 

Permite llevar registro de todas las acciones que se realicen en este. 

- Es una opción del Bucket de S3
- Conecta a servicio CloudTrail. Por lo que debe primero crearse un Trail en AWS CloudTrail, esto es básicamente una cola o registro en donde se almacenarán las llamadas de nuestro Bucket.
- Se crea un Trail y se puede crear el Bucket que almacenará los Logs
Esto guardará un log de todas las acciones que se realizaron en el s3.

### Transfer Acceleration

Se debe activar en el bucket que necesitamos. AWS identificará el CDN que tenga mejor tiempo de respuesta y aprovechando el caché, será más rápido. 

### Eventos en S3

- Se debe activar la sección Events. Ahí se pueden agregar todos los eventos que se quieran sean notificados. 
- Se puede crear notificaciones para ciertas acciones y en qué carpetas o archivos que queremos tener dichas notificaciones.
- Estas notificaciones pueden enviarse a un lambda, SQS, SNS. Este último por ejemplo, si tenemos un evento de eliminar archivos, el SNS haría que llegara un correo electrónico a todos los observadores cuando se vaya a eliminar un objeto. 

> Written by [Gonzalo Muñoz]().
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkyODQ2OTY4NSwxODQ4NDU5MTEzLC0zOT
I3MjIzNzcsMTAyODQ4NjEyXX0=
-->