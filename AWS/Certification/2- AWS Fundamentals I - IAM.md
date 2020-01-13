
## IAM

### AWS Regions

- AWS tiene regiones por todo el mundo.
- Cada región tiene zonas de disponibilidad. (us-east-1a, us-east-1b, etc.) estas son de la A a la F. 
- Representan data centers físicos en la región, pero separados unos de otros. (para que estén isolados en cualquier caso).

- Las consolas de AWS tienen un alcance regional (con excepción de IAM y S3), esto quiere decir que cada vez que se hagan cambios o modificaciones, estas se harán en la región específica.

### Introducción IAM

- **IAM**: Identity and Access Management
- Toda la seguridad de nuestro AWS irá en IAM:
-- Usuarios
-- Grupos
-- Roles

- Existe una cuenta Root, que no debe ser nunca usada, esta es para crear cuentas AWS.
- Los usuarios deben crearse con permisos apropiados
- IAM es el centro de AWS.
- Hay pólizas dentro de IAM, y están escritas en JSON.

#### ¿Cómo visualizamos IAM en un nivel superior?

- Usuarios:  Generalmente es una persona física. Alguien que posea una cuenta en IAM. 

- Grupos: Los usuarios están agrupados. Pueden ser de cualquier tipo, pero usualmente existen por funciones, por ejemplo, admins, devops, etc. La idea de tener grupos es que hayan personas en él, y de esta forma los permisos asociados a un grupo serán heredados por los usuarios en él. 

- Roles: Son sólo para uso interno dentro de los recursos y servicios de AWS, por lo que los roles son dados a las máquinas. 

- Pólizas: Finalmente, las pólizas definen qué cosas pueden o no pueden hacer los usuarios, grupos y roles. 

- Como se dijo anteriormente, IAM tiene una vista global, es decir, cuando se crea un usuario, grupo o rol, esto será para todas las regiones. 

- IAM tiene pólizas predefinidas que se pueden utilizar. 

- La idea es darle a los usuarios los permisos mínimos para desarrollar su tarea. 

- Para empresas grandes, existe algo llamado **IAM Federation**, lo que permite esto es integrar su propio repositorio de usuarios como Active Directory, con IAM, permitiendo que se pueda acceder a AWS con las credenciales corporativas. 
 
##### Cosas importantes

- Sólo se debe asignar un usuario IAM por cada persona física.
- Sólo se debe asignar un Rol IAM por aplicación. Cada app tiene su ciclo de vida, por lo que debe tener un rol separado. 
- Nunca usar la cuenta ROOT para algo que no sea la configuración inicial.
- Nunca usar las credenciales ROOT IAM.



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxMjUzODYyMjldfQ==
-->