## Guards

Son scripts que implementan una estrategia de seguridad para accesos no autorizados a las rutas de la aplicación. 

```
ng g guard services/authGuard
```

Donde definimos las rutas, debemos declarar el guard de la siguiente manera:

```
{path: 'home', component: HomeComponent, canActivate: [AuthenticationGuard]},
```

Luego de eso, debemos modificar el archivo guard, que creamos con el ng cli. 


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTAyMjc4NTk0Nl19
-->