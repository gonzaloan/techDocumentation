## Javascript


### DOM
El navegador recibe un documento .html, y lo procesa convirtiéndolo en una estructura de árbol llamada DOM. Luego de eso, el navegador indica **DOMContentLoaded** y eso significa que esta listo. 

### Scripts Loading
Los scripts pueden llamarse de diversas formas en el html:

```html
<script></script>

<!-- 
Carga asíncronamente. Útil para scripts de google y ese tipo de cosas
--> 
<script async></script>

<!-- Lanza la llamada de la ejecución para el final de la carga. -->
<script defer></script>
```


### Scope

```javascript
var message = "Hola" //Scope Global

let message = "Hola" //Valor cambia, pero opera sobre el block scope

const message = "Hola" //Valor constante

```

#### Module
```html
<!-- indica que el script es un módulo. Limita el alcance, yno salen del archivo donde están. -->
<script> </script>

```

### Closures

Son funcionan que regresan una función o un objeto con funciones que mantienen las variables que fueron declaradas fuera de su scope

```javascript
(function() {
let  color = 'green';
function  printColor() {
console.log(color);
}
printColor();
})();
```

Sirven para construir una especie de variables privadas, encapsulan variables que no pueden ser modificadas por otros objetos, sólo por funciones pertenecientes al mismo. 

```javascript
function  makeCounter(n) {
let  count = n;
return {
increase:  function () {
count++;
},
decrease:  function () {
count--;
},
getCount:  function () {
return  count;
   },
  };
}
let  counter = makeCounter(7);
console.log("The count is: ", counter.getCount());
counter.increase();
console.log("The count is: ", counter.getCount());
counter.increase();
```

### Call, Apply, Bind
Todas las funciones tienen call, apply, bind

#### Call
Acepta como parámetro de entrada un objeto que asocia como su *this*

```javascript
function saludar() {
	console.log(`Hola. Soy ${this.name} ${this.apellido}`);
}

const gonzalo = {
	name: 'Gonzalo',
	apellido: 'Munoz',
};

saludar.call(gonzalo);
```
Esto dará como resultado "Hola, Soy Gonzalo Munoz".


#### Apply
Similar al call, pero entrega un arreglo con los argumentos

```javascript
function caminar(metros, direccion){
	console.log(`${this.name} camina ${metros} metros hacia ${direccion}`
}

//Con call
caminar.call(gonzalo, 400, 'norte');

//Con apply
caminar.apply(gonzalo, [800, 'noreste']);
```

#### Bind
Haceu n bind de parámetros con algún elemento, pero no retorna nada

```javascript
const daniel = {name: 'Daniel', apellido: 'Sanchez'}
saludar.bind(daniel); //no retorna nada

//Ahora si lo imprime.
const danielSaluda = saludar.bind(daniel);
console.log(danielSaluda());

//Otro ejemplo con más parámetros
const danielCamina = caminar.bind(daniel, 2000);
danielCamina('sur');

console.log(danielCamina()); 
//Daniel camina 2000 metros hacia el sur.
```

## Prototype

```javascript
function Hero(name) {  
  this.name = name;  
}  
Hero.prototype.saludar = function() {  
  console.log(`Soy ${this.name}`);  
};   
const zelda = new Hero('Zelda');  
zelda.saludar();  
const link = new Hero("Link");  
link.saludar();
```

## Promises

Es importante al trabajar con javascript, usar async await, esto permite que sea más fluido y se va a leer mejor. 

Si tenemos por ejemplos llamadas fetch que usan *then*:

```javascript
function getPopularMovies() {
	const url = `https://api.themoviedb.org/blablabla?movies.json`;
	return fetch(url)
		.then(response => response.json());
}
```
Podemos volverlo asíncrono:

```javascript
async function getPopularMovies() {
	const url = `https://api.themoviedb.org/blablabla?movies.json`;
	const response = await fetch(url);
	const data = await response.json();
	return data.results;
}
```
### Promises en paralelo
Podemos trabajar con las llamadas paralelamente:

```javascript
async function getTopMoviesInParallel() {
	const ids = await getTopMoviesIds()
	const moviePromises = ids.map(id => getMovie(id))
	const movies = await Promise.all(moviePromises)
	return movies
}
```
Promise all esperará que se cumplan todas las promesas.

### Promises en Race
Podemos hacer una carrera y que sólo se obtenga el valor que se resuelve más rápido
```javascript
async function getTopMoviesInParallel() {
	const ids = await getTopMoviesIds()
	const moviePromises = ids.map(id => getMovie(id))
	const movie = await Promise.race(moviePromises)
	return movie
}
```

## Generators 

Son funciones especiales, pueden pausar su ejecución y luego volver al punto donde se quedaron recordando su scope

```javascript
function* simpleGenerator(){
	console.log('Generator');
}
//retorna un undefined, si se ve el valor de gen, estará suspendido
const gen = simpleGenerator();

//para continuar su ejecución. Ejecutará el código y cambiará su estado a 'done'. Imprimirá Generator
gen.next();

//LO interensante aquí es que se puede 'pausar' el generator:
function* simpleGenerator(){
	console.log('Generator');
	yield
	console.log('End Generator');
}
const gen = simpleGenerator();
//Ahora next imprimirá Generator y quedará en estado 'done': false
gen.next(); 
```

Un uso interesante:

```javascript
function* idMaker() {
	let id = 1;
	while(true){
		yield id;
    id = id + 1;
	}
}

//Retornará id y parará, retornando el id

//También podemos resetear el valor, enviando un valor boolean por ejemplo al generator

function* idMaker() {
	let id = 1;
	let reset;
	while(true){
		reset = yield id;
		if(reset) {
			id = 1;
		}else{
		    id = id + 1;
	    }
	}
}

//Esto hará que si le mandamos un True, retornará un 1, sino, aumentará el valor de id.
```

#### FIbonacci con Generator

```javascript
function* fibonacci() {
	let a = 1;
	let b = 1;
	while(true){
		const nextNumber = a + b;
		a = b;
		b = nextNumber;
		yield nextNumber;
	}
}
```

## Fetch

Podemos hacer un fetch asíncrono: 

```javascript
const response = await fetch(url);
//si cargamos una imagen queremos el blog (binario)
const blob = await response.blob();
```

Qué pasa si por ejemplo, está demorando mucho? Podemos cancelarlo

```javascript
let controller = new AbortController();

const response = await fetch(url, {signal: controller.signal });
//si cargamos una imagen queremos el blog (binario)
const blob = await response.blob();

//Si queremos detener, por ejemplo, con un botón de stop fetching, debería tener la función:
controller.abort();
```

## IntersectionObserver

Sirve para observar elementos y si cruzan un umbral que nosotros definimos, nos lo va a notificar para tomar acción. 
Se define por el porcentaje que tiene intersección con el viewport, la parte visible de la página. 

```javascript
class AutoPause {  
  run(player) {  
  const observer = new IntersectionObserver(this.handleIntersection, {  
  threshold: 0.25  
  });  
  
  observer.observe(player.media)  
 }  
  handleIntersection(entries) {  
  const entry = entries[0];  
  console.log(entry);  
  }  
}  
  
export default AutoPause;
```
Esto observará cada vez que el viewport se mueva 25%. De entrada y de salida.

## Visibility Change

Permite saber si el elemento es visible, puede ser usado para ejecutar una acción cuando cambiamos de pestaña.

```javascript
document.addEventListener('visibilitychange', this.handleVisibilityChange);

handleVisibilityChange(){  
  const isVisible = document.visibilityState === 'visible';  
 if (isVisible) {  
  this.player.play();  
  }else{  
  this.player.pause();  
  }  
}
```

## ServiceWorkers

Intercepa la petición para guardar el resultado en el caché. 

Se debe definir el uso de Service Workers en un js que se cargue con la página:

```javascript
if('serviceWorker' in navigator){  
  navigator.serviceWorker.register('/sw.js').catch(error => {  
  console.log(error.message);  
  });  
}
```

Podemos ahora, guardar la info en el caché de la siguiente forma:

```javascript
self.addEventListener('install', event => {  
  event.waitUntil(precache())  
});  
  
async function precache(){  
  const cache = await caches.open('v1')  
  cache.addAll([  
  '/',  
  '/index.html',  
  '/assets/MediaPlayer.js',  
  '/assets/plugins/AutoPause.js',  
  '/assets/plugins/AutoPlay.js',  
  '/assets/index.css',  
  '/assets/BigBuckBunny.mp4'  
  ])  
}
```
Aquí, se guardan todos los assets en memoria, en el apartado v1. Ahora se debe hacer que cuando ocurra una petición, revisar el caché para buscar la respuesta:

```javascript
self.addEventListener('fetch', event => {  
  const request = event.request;  
  //Sólo queremos cachear los GET  
  if(request.method !== 'GET'){  
  return;  
  }  
  //Si es un get:  
  event.respondWith(cachedResponse());  
  
});
```

Si no tenemos la respuesta cacheada, puede ir a buscarla con un fetch, la respuesta cacheada o con fetch está aquí:

```javascript
async function cachedResponse(request){  
  const cache = await caches.open('v1');  
  //Ver si tenemos la respuesta del request  
  const response = await cache.match(request);  
  //Retornamos la respuesta, sino que haga el fetch  
  return response || await fetch(request)   
}
```

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc0NTQzMDU0NV19
-->