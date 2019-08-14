# Test Driven Development

Para hacer un TDD utilizamos lo que se llama método Red-Green-Refactor.
Esto quiere decir que para probar una funcionalidad hacemos:

 1. Creamos un archivo de Test a la clase que deseemos probar. Una buena opción es definir arriba de la clase, como un comentario todas las funcionalidades que debe cumplir la prueba.
Por ejemplo:
```java
/* 
	 * 1) Si el número es divisible por 3, retorna Fizz
	 * 2) si el número es divisible por 5, retorna Buzz
	 * 3) Si el número es divisible por 3 y 5 retorna FizzBuzz
	 * 4) En otro caso, retorna el mismo número
	 * */
```
Esto nos ayudará a tener un orden y saber qué es lo que debemos completar.
 3. Crear un test que de como resultado un error (ya que no hay código que respalde la prueba.
 4. Escribir el código necesario para que la prueba pase.
 5. Refactorizar el código para que quede lo mejor posible.
 6. Seguir con las demás pruebas.

Para aplicar un TDD en un proyecto tipo API o por capas, lo recomendado es:

 1. Crear un test de la clase Service a probar. Ahí colocar todas las funcionalidades que esperamos hace ese servicio. Para esto no es necesario tener algún repositorio definido, ya que con Mockito podemos simular lo que queremos que respondan los métodos.
 2. Luego probar los Repository. 
 3. 



## Consejos

 - Nombres de la clase test deben terminar en **Should**, ejemplo: **UsersServiceShould.java**
 - Escribir como comentario al principio de la clase a probar qué pruebas haremos, esto nos servirá de guía para saber qué testear.
 - El nombre de los métodos de prueba debe ser descriptivo, ejemplo: **public void return_all_users_when_findAll_method_is_called()**
 - 

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg2NjMxOTA5N119
-->