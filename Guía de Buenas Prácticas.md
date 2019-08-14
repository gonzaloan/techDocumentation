# Guía de buenas prácticas de programación y código limpio
Esta guía marca una manera de escribir código que sea mucho más fácil de mantener, testear y desarrollar. 

## 1) Convención de nombres

- Las variables, métodos y clases deben usar el estilo de **CamelCase**, La única excepción son las clases, cuya primera letra debe ser siempre Mayúscula.

```java
class UserService{
	private String name;
	public String getName(){
		return name;
	}
}
```
- Las constantes deben estar escritas en mayúsculas, divididas de guión bajo:
```java
final String CARACTER_GUION = "-";
```
- Evitar caracteres especiales y números en nombres:

```java
//Mala práctica
User user1 = new User();
User user2$ = new User();
```
- No usar muchas palabras en los nombres de variables/métodos. Deben ser lo más simple posible e informativos.
```java
//Mala práctica
public void saveUserIntoMyDatabaseMongoDB(User user);

//Esto es mejor
public void saveUser(User user);
```
## 2) Escribir variables descriptivas

Al crear variables nunca usar sólo una letra en el nombre. Las variables deben representar el valor por el cual fueron creadas, esto servirá para cuando otros desarrolladores vean tu código, puedan entenderlo.
```java
public void check(){
	if (a < x){
		return true;
	}else{
		return false;
	}
}
```
Esto es poco claro.

## 3) Declaración de variables/métodos
Todas las variables de la clase deben ser declaradas al inicio de la clase, de esta manera siempre es claro saber dónde encontrar la declaración de la misma. 

Si esta variable sólo se utiliza en un método, es mejor declararla como una variable local del método.

Los métodos deben ser declarados en el mismo orden en que se utilizan. Por ejemplo, si tenemos un método **checkPerson()** que llama al método **verifyPersonId()**, este último debe estar declarado sobre checkPerson(). Además los métodos más importantes deben ser declarados al inicio de la clase, y los menos importantes abajo.  

## 4) Simple Responsibility (De SOLID) 
Un método debe ser responsable sólo de una acción. Si un método hace dos o tres cosas distintas, es mejor dividir la funcionalidad en otros métodos, esto hace que los métodos sean más fáciles de entender, escalar y reutilizar. 

Por ejemplo, si tenemos un método **saveUserAndCheckBalance()**, este método por su nombre entendemos que hace dos cosas. Hay que evitar utilizar palabras como **and** o **or**, es mejor separar la función en **saveUser()** y **checkBalance()**

## 5) Métodos pequeños
No hay un estándar para definir cuántas líneas debe tener un método, pero hay que intentar que sean lo más cortos posibles. Hay que intentar que sean siempre entendibles y lo menos complejos posible, aunque nuevamente, no hay un estándar definido.

## 6) Minimizar el código
Evitar escribir tres líneas de código si se puede hacer en una sola, siempre y cuando el código sea entendible. 

```java
boolean hasMoneyAvailable(User user){
	if(user.hasBalance){
		return true;
	}else{
		return false;
	}
}
```
Podría ser simplificado a:
```java
boolean hasMoneyAvailable(User user){
	return user.hasBalance;
}
```

## 7) Nunca duplicar código
Evitar lo más posible tener código duplicado en el proyecto. Si no es posible reusar un método en otro lugar, entonces probablemente el método podría construirse de mejor manera, estos deben ser lo más universales posible.

## 8) Usar comentarios
Antes era normal usar comentarios en todas partes del código. Hoy en día tener que escribir muchos comentarios es visto como una señal de que el código no es autodescriptivo. 
Está bien dejar comentarios, siempre y cuando sea en algún lugar importante, y siempre y cuando pueda ayudar a los demás miembros a entender mejor la funcionalidad. 

## 9) Usar Checker Tools
Utilizar herramientas que prueben la calidad del código. Por ejemplo SonarQube. Para Angular una opción es TSLint.

## 10) Mejores prácticas
Tener un código limpio no es suficiente, se necesitan buenas prácticas de programación:

### Java

-	Preferir retornar vacíos en vez de **null**, esto ahorrará muchos if / else al testear elementos nulos.
```java
public String getName(){
	return (null==name ? "" : name);
}
```
- Usar **length** en vez de **equals** para chequear si un valor está vacío.

-	Usar los String con cuidado, evitar concatenar String en algún loop, ya que esto genera cada vez un nuevo objeto:
```java
//Mala práctica
String output = "";
for (String s : largeArray) {
    output = output  + s;
}
```
En estos casos es mejor utilizar un StringBuilder:

```java
//Esto está OK
StringBuilder sb = new StringBuilder();
for (String s : largeArray) {
    sb.append(s);
}
String output = sb.toString();
```

-	Evitar ***objetos*** innecesarios. Es recomendado que los objetos sean sólo creados o inicializados en caso de ser necesario. Ejemplo:
```java	
public class Person {
	private List persons;
	
	public List getPersons(){
		//Se inicializa sólo si es necesario
		if(null == persons){
			persons = new ArrayList();
		}
		return persons;
	}
}
```
- Evitar memory leaks:
-- Cerrar conexiones con bases de datos cuando las queries terminen.
-- Intentar usar bloques Finally siempre que se pueda.

- Manejar las posibles NullPointerException prematuramente para que puedan ser eliminados,  antes de que se llame a algún método sobre ellos:
```java	
int numberOfClients = store.listClients().count;  
//Si esto es null hará un nullpointer
```
Es mejor manejarlos antes:
```java
private int getListOfClients(File[] files){
	if ( null == files){
		throw new NullPointerException("File can't be null");
	}
}
```

- Preferir **Double** sobre **Float**, estas son más precisas. 
- Asignar valores **null** a variables que ya no serán utilizadas.
- Declarar todas las constantes como **static final**.
- Dentro de los métodos, declarar los argumentos **final** si no serán modificados después de su declaración.


### Angular
Es importante definir:

- Definir una estructura que permita una gestión simple, mantenimiento del código base y que permita sea escalable fácilmente. 
- Utilizar nombres consistentes y descriptivos seguidos de puntos
```angular
|-- my-feature.component.ts  
|-- my-service.service.ts
```
- Separación de Concerns: Dividir la aplicación en múltiples módulos, el código es más organizado, mantenible, legible y reutilizable, y permite hacer un lazy-load según se requiera.
- Crear y utilizar un common frame, en donde se declare todos los módulos reutilizables. 
- Utilizar TSLint para analizar el código y chequear si el código de Typescript sigue las reglas básicas.
- Limpiar los imports con alias de path: 

Evitar esto: 
```typescript
import 'reusableComponent' from '../../../shared/components/reusable/reusable.component.ts';
```

Utilizando un alias que se declara en el archivo **tsconfig.json**:
```typescript
{  
  "compilerOptions": {  
    ...  
    "baseUrl": 'src',  
    "paths": {  
      "@app:": ['@app/*']  
    }  
  }  
}
```
De esta manera, podemos ahora referencias un archivo así:
```typescript
import 'ReusableComponent' from '@app/shared/';
``` 

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTE0MTU2MTkzNF19
-->