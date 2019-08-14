# Spring Security

Una buena práctica es forzar la autenticación y autorización en cada página de la aplicación. Las credenciales y la autorización deberían ser veriicadas antes de ejecutar cualquier petición a una aplicación web.

Pasos para usar Spring Security:

- Añadir dependencia Spring Security
- Configurar la intercepción de todas las peticiones
- Configurar Spring Security
- Añadir la funcionalidad de logout

## Configurar Filtro para interceptar requests

La mejor práctica es validar todos los request que vienen. Se quiere que el framework vea una petición que viene, autentique al usuario y le permita realizar la acción sólo si el usuario tiene acceso a la operación. 

Se puede configurar Spring Security para que intercepte todas las peticiones a través de un filtro

```java
 @Configuration 
    @EnableWebSecurity 
    public class SecurityConfiguration extends  
    WebSecurityConfigurerAdapter { 
      @Autowired 
      public void configureGlobalSecurity 
      (AuthenticationManagerBuilder auth) throws Exception { 
      auth 
      .inMemoryAuthentication() 
      .withUser("firstuser").password("password1") 
      .roles("USER", "ADMIN"); 
     } 
     @Override 
     protected void configure(HttpSecurity http)  
     throws Exception { 
       http 
      .authorizeRequests() 
      .antMatchers("/login").permitAll() 
      .antMatchers("/*secure*/**") 
      .access("hasRole('USER')") 
      .and().formLogin(); 
      } 
    }
```

Cosas importantes:

- `@EnableWebSecurity`: Permite a la clase configuración contener la definición de la misma. 
- `protected void configure(httpSecurity http)`: Método proporciona las necesidades de la seguridad para diferentes URLS
- `antMatchers("/*secure*/**").access("hasRole('USER')")`: Se necesita el role USER para acceder a cualquier URL que contiene el substring `secure`.
- `antMatchers("/login").permitAll()`: Permite el acceso a la página de login a todos los usuarios.
- `public void configureGlobalSecurity(AuthenticationManagerBuilder auth)`: En este ejemplo, estamos usando autenticación in-memory. Esto puede ser conectandose a la base de datos (`auth.jdbcAutehntication()`, o un LDAP ( `auth.ldapAuthentication()`), o un provider customizado (creado extendiendo `AuthenticationProvider`).
- `withUser("firstuser").password("password1")`: Configura una combinación user/pass en memoria
- `.roles("USER", "ADMIN")`: Asigna roles al usuario.

Cuando se intenta acceder a cualquier URL secura, serás redirigido a una página de login. Se puede personalizar la página y la redirección.





> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2ODUxNTQzMTNdfQ==
-->