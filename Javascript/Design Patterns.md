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
	subscribe(observer: Observer){
		this.observers.push(observer);
	}
	unsubscribe(observer: Observer){
		const index = this.observers.findIndex()
	}
}
```




> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk2MTE3NDU5Ml19
-->