## Basics


### Interfaces de Typescript
Es como crear un nuevo tipo que se va a personalizar. 

```ng g i interfaces/user```

En el cuerpo de la interfaz se ponen las propiedades que deben tener el objeto usuario:

```typescript
export interface User {  
  nick: string;  
  subNick?: string;  
  age: number;  
  email: string;  
  friend: boolean;  
  uid: any;  
}
```
Agregar el signo de pregunta indica que un campo es *opcional*

### Navegación con parámetros

Para envíar los parámetros debemos especificarlos en el routerLink

```typescript
<div>  
 <b>Friends</b>  
 <ng-container *ngFor="let user of friends; let i = index">  
 <div *ngIf="user.friend">  
 <a routerLink="/conversation/{{user.uid}}">  
  {{i}}: {{user.nick}}  - {{user.email}}  
  </a>  
 </div> </ng-container></div>
```

Y para que conversation tome estos parámetros, debemos modificar donde tenemos definidos los path:

```typescript
const appRoutes: Routes = [  
  {path: '', component: HomeComponent},  
  {path: 'home', component: HomeComponent},  
  {path: 'login', component: LoginComponent},  
  {path: 'conversation/:uid', component: ConversationComponent},  
  {path: 'profile', component: ProfileComponent}  
];
```

Con esto, ahora debemos desde conversation tomar estos datos:
Para ello, vamos al component de conversation, y en el constructor debemos definir lo siguiente:

```typescript
friendId: any;  
constructor(private activatedRoute: ActivatedRoute) {  
  this.friendId = this.activatedRoute.snapshot.params['uid'];  
  console.log(this.friendId);  
}
```



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ4MjYzMTk3NF19
-->