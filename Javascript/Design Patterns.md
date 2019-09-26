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
```




> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU2OTI5NDc1NCwtMTUyMDA2OTU3NSwxOT
YxMTc0NTkyXX0=
-->