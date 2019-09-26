## Design Patterns

### Singleton

```javascript
class Singleton {
	//Una instancia privada 
	private static instance: Singleton;

	private constructor(){
		//init
	}

	static getInstance() {
		if(!Singleton.instance){
			Singleton.instance = new Singleton();
		}
		return Singleton.instance;
	}
}

export default Singleton;
```

### Observer

Contiene dos participantes:
- Sujeto: Permite suscribirse a él. Cuando hay algo envía información a todos los observadores.
- Observadores

```typescript
interface Observer{
	update(data: any) => void
}
interface Subject{
	subscribe: (observer: Observer) => void
	unsubscribe: (observer: Observer) => void
}
//Clase va a recibir los cambios de precio y los
//Avisará a sus suscriptores 
class BitcoinPrice implements Subject{
	observers: Observer[] = [];
	constructor(){
		//Si hay un cambio en este elemento, debería notificar a todos.
		const el = document.querySelector("#value");	
		el.addEventListener('input', () => {
			this.notify(el.value);
		});
	}
	subscribe(observer: Observer){
		this.observers.push(observer);
	}
	unsubscribe(observer: Observer){
		const index = this.observers.findIndex(obs => {
		return obs === observer 
		});
		//Sacamos de la lista observers
		this.observers.splice(index, 1);
	}
	//Cuando hay algún cambio notificamos
	notify(){
		this.observers.forEach(observer => 	observer.update(data));
	}
}

class PriceDisplay implements Observer {
	private el: HTMLElement;
	
	constructor(){
		this.el = document.querySelector("#price");
	}
	//Actualizamos
	update(data: any) {
		this.el.innerText = data;
	}
}

const value = new BitcoinPrice();
const display = new PriceDisplay();
value.subscribe(display);
```
Este ejemplo está pensado usando un Label y un Textbox.
Cada vez que el textbox se modifica, el Label se actualiza automáticamente.

### Decorator

- Añade nuevas responsabilidades a un objeto de forma dinámica.
- Nos permiten extender funcionalidad sin tener que usar subclases.



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI5MTI2NjU0NCwtNDkzNjEwMzY4LC0xMj
M4NjY4MzQ2LC05Mjc1MzQxMzUsLTQ0Mjk5MzI1NywtNTY5Mjk0
NzU0LC0xNTIwMDY5NTc1LDE5NjExNzQ1OTJdfQ==
-->