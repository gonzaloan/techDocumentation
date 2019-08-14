## RxJs

Operador utilizado para trabajar con datos de forma asíncrona.

Se debe importar:
```typescript
import {Observable} from 'rxjs';
import 'rxjs/Rx';
```

### Diferencias de Observables (RxJs) y Promesas

- Promesas retornar una promise; Observable trabaja con observable.
- Promesas regresan un sólo objeto; Observable retorna un stream de objetos.
- Promesas avisan cuando termina; Observable avisa que hay algo nuevo.
- Promesas no pueden ser canceladas; Observables si
- Promesas tienen operadores muy limitados; Observables tienen una variedad de operadores.

## TypeAhead
Para crear un stream de observables.

```typescript
private searchField: FormControl;
results$: Observable<any>;
constructor(private http:Http){
	URL = 'https://maps.google.com/maps/api/geocode/json';
	this.searchField = new FormControl();
	//Esto estará escuchando los cambios de searchField
	this.results$ = this.searchField.valueChanges;
			.switchMap(query => this.http.get(`${URL}?address=${query}`))
	//Cada vez que el usuario teclee estará obteniendo informacion
			.map(response=>response.json())
			.map(response => response.results);		
}
```
En código anterior estamos escuchando searchFields, cada vez que el usuario cambia su valor, estamos agregando un objeto al string. En switchmap, cada vez que llega el cambio, llega con un query y por cada cambio hacemos una llamada http que va a la URL con la información. AL obtener la información, con el map estamos tratándola y obteniedno el json, y finalmente obtenemos la propiedad final que se llama results. 

En el html hay que agregar un input de tipo FormControl y así funciona.


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDk0NTU4MjkzXX0=
-->