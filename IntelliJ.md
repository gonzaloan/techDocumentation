##Guía para desarrollo con Intellij IDEA

#### Navegación
Para buscar en todos los archivos algún texto
**Ctrol+Shift+F**
Para navegar a alguna clase se puede escribir **Ctrl + N** y buscar la clase. Además puedes poner sólo las camelCase del nombre de la clase y encontrará resultados de igual manera.

Para navegar a algún archivo o directorio **Ctrl + shift + N**. 
Podemos también indicar a qué número de línea de la clase queremos ir: 
**NombreClase:NumLinea**

Para buscar un método o attribute, se utiliza **Ctrl + Shift + Alt + N**

Archivos recientes: **Ctrl + E**

Navegar entre los archivos abiertos: **Ctrl + Alt + Izq** y **Ctrl + Alt + Der**

Navegar entre métodos en una clase: **Alt + arriba** y **Alt + Abajo**

#### Live Templates
Una plantilla personalizable es un molde para hacer alguna clase. 
**Settings -> Editor -> Live Templates**
Acá podemos crear o ver las plantillas disponibles. Crearé una para hacer un test de pruebas por default:
```java
@Test
public void should$METHOD_NAME$(){
   //given
   $END$
   //when
   
   //verify
}
```
#### Generar Código

Para abrir menú y poder crear getter, setter, etc: **Alt + Insert**

Refactorizar es muy amplio, el menú para ello aparece con: **Ctrl + Shift + Alt + T**, es necesario destacar que este menú y las opciones dependen de dónde es ejecutado. b


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTczMzUwMjYwM119
-->