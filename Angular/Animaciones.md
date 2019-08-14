## Animaciones

### Triggers y States
En el componente se debe agregar después de templateUrl:

```typescript
animations: [
	trigger('contenedor',[
		state('inicial', style({
			opacity:0,
			backgroundColor: 'green',
			transform: 'rotate3d(0,0,0,0deg)'
		})),
		state('final', style({
			opacity:1,
			backgroundColor: 'yellow',
			transform: 'rotate3d(5,10,20,30deg)'
		}))
	]
]
```
Ahora en la declaración de la clase agregamos una variable state = 'inicial' y en el html hay que asignarle al elemento que queremos animar. 

```html
<div id="cuadrado" [@contenedor]='state'>  ... </div>
```
Entonces ahora, cuando el valor state sea de 'final' cambiará según lo definimos

### Transitions

```typescript
animations: [
	trigger('contenedor',[
		state('inicial', style({
			opacity:0,
			backgroundColor: 'green',
			transform: 'rotate3d(0,0,0,0deg)'
		})),
		state('final', style({
			opacity:1,
			backgroundColor: 'yellow',
			transform: 'rotate3d(5,10,20,30deg)'
		})),
		transition('inicial=>final', animate(1000),
		transition('final=>inicial', animate(1000),
	]
]
```
Ahora para verlo funcionar, debemos tener alguna función que modifique la variable state que definimos

```typescript
animar(){
	this.state = (this.state == 'final')?'inicial':'final'
}
```

### Callbacks

Funciones que se ejecutan al terminal o iniciar cada transición. 

```html
<div id="cuadrado" 
	[@contenedor]='state' (@contenedorAnimable.start='animacionInicial($event)')
	(@contenedorAnimable.done='animacionTermina($event)')>  ... 
</div>

```
Luego de esto, se crean las funciones en el component. 



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUyMzcwNDM4NV19
-->