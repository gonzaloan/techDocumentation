
## Java



### Clases Abstractas
A veces no se necesita implementar todos los métodos, ni heredad todos los métodos. Para ello está las clases abstractas. 
Es decir, no implementa todos los métodos, debemos definir cuáles deben ser implementadas obligatoriamente.
ni crearemos instancias.


```java
public abstract class Figura{
	abstract void dibujate();
}
```
Las clases abstractas siempre se heredan. Para definir que un método es obligatorio de implementar tendrá la palabra **abstract**. La clase hija, que extienda *Figura*, puede implementar o no la clase abstract siempre y cuando el método sea abstract también:
```java
class Triangulo extends Figura {
	abstract void dibujate();
}
```
O en caso de querer implementar el método:
```java
class Triangulo extends Figura{
	@Override
	public void dibujate(){
	}
}
```
Las clases abstractas no se pueden instanciar con **new**.


### Java Docs
Para hacer comentarios de documentación:
Estos están dentro de : /**    **/ 

Y pueden tener la siguiente notación:
-   **@author**: Nombre del desarrollador.
-   **@version**: Versión del método o clase.
-   **@param**: Definición de un parámetro de un método, es requerido para todos los parámetros del método.
-   **@return**: Informa de lo que devuelve el método, no se puede usar en constructores o métodos “void”.
-   **@exception**: Excepción lanzada por el método, posse un sinonimo de nombre @throws
-   **@see**: Enlaza con otro método o clase.
-  **@deprecated** : Indica que el método o clase es antigua y que no se recomienda su uso porque posiblemente desaparecerá en versiones posteriores.
-  **@link**: se puede colocar de qué clases hereda. Esto generará un link a esa clase que ponga como objetivo. Ej: @Link ArrayList.
- **@inheritDoc**: Esto traerá de todo lo heredado.
- 

La estructura básica es:
```java
/**
* [descripción corta]
* <p>
* [descripción larga]
* 
* [author, version, params, returns, otros tags
* [see also]
*/
```
Ejemplo de comentario de clase:
```java

/**
 * <h1>AmazonViewer</h1>
 * AmazonViewer es un programa que permite visualizar Movies, Series con sus respectivos Chapters, Books y Magazines.
 * Te permite generar reportes generales y con fecha del día.
 * <p>
 * Existen algunas reglas como todos los elementos pueden ser visualizados o leídos a excepción 
 * de las Magazines, estas sólo pueden ser vistas a modo de exposición sin ser leídas.
 * 
 * @author njmunoz
 * @version 1.1
 * @since 2019
 * */
```
Y para comentar un método:

```java
/**
* Este método captura el tiempo exacto de inicio y final de visualización.
* 
* @param dateI Es un objeto {@code Date} con el tiempo de inicio exacto.
* @param dateF Es un objeto {@code Date} con el tiempo final exacto.
* */
```
### Interfaces avanzadas
Desde java 8 se puede añadir métodos en una interfaz, el método **default**, podemos implementar código dentro de la interfaz.
Desde Java 9, podemos tener métodos *private*

```java
public interface MyInterface {
	default void defaultMethod() {
		privateMethod("Hola!!!");
	}
	private void privateMethod(final String string) {
		System.out.println(string);
	}
	void normalMethod();
}
```
El método private no será accesible para ninguna clase que la implemente, sólo es accesible por el método default.

**¿Cuándo usar Clases Abstractas o Interfaces ?**

La clase abstractas se utilizarán para definir subclases. No se pueden usar instancias, sólo heredar. Sólo servirá para redefinir sin crear nuevos objetos. La clase abstracta está pensada en objetos.
Para las interfaces, se utiliza para implementar métodos que se comparten entre familias. Las interfaces están pensadas en comportamiento.

### Map

```java
Map<Integer, String> map = new HashMap<Integer, String>();
Map<Integer, String> treeMap = new TreeMap<Integer, String>();
Map<Integer, String> linkedHashMap = new LinkedHashMap<Integer, String>();
```
Usos:
-   **HashMap:** Los elementos no se ordenan. No aceptan claves duplicadas ni valores nulos.
-   **LinkedHashMap** Ordena los elementos conforme se van insertando; provocando que las búsquedas sean más lentas que las demás clases.
-   **TreeMap** El Mapa lo ordena de forma “natural”. Por ejemplo, si la clave son valores enteros (como luego veremos), los ordena de menos a mayor.


###  Interfaz Funcional
Tienen un sólo método, que debe ser **abstracto**.
Debe tener la anotación *@FunctionalInterface*
```java
@FunctionalInterface
public interface Greeting {
	public void perform();
}
```
Al tener la anotación, el compilador automáticamente lo setea como abstracto.


### Lambdas

**( parámetros )** -> **{ cuerpo-lambda }**


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUzMTM1Mzc4Nl19
-->