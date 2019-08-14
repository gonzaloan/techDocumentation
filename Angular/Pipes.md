## Pipes

Toman datos de entrada los transforman en una manera determinada y los retornan.

### Pipes por defecto
Por defecto es 
```
dato | {pipe}:'{param}'
```
- lowercase
- uppercase
- date:'medium' //Muestra fecha y hora
- date:'short' // Muestra fecha más compacta
- date:'fullDate' //Fecha completa
- uppercase
- json // representación Json del elemento
- number: '1.2-2'  //Muestra un número formateado, el .2-2 indica que habrán mínimo 2 parámetros y máximo 2.

### Pipe Customizado

Es buena práctica tener una carpeta en el proyecto para los pipes.

Con el angular-cli:
```
ng g p pipes/linkify
```
Y para transformar la información dentro del pipe se debe modificar el método transform que viene por defecto:

```typescript
@Pipe({name:'linkifystr'})
export class LinkifystrPipe implements PipeTransform{
    transform(str: string): string{
        return str ? linkifyStr(str, {target:'_system'}) : str;
    }
}
```
En este pipe, se transforma un texto a hipervínculo, el target indica que se inyectará un target de tipo _system.

Luego de eso, se puede usar con el nombre indicado en el Pipe.


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NDMyNTYyOThdfQ==
-->