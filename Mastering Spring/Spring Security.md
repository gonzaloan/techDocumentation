## Spring Security

Cada request a una REST API debería ser controlada con autenticación y autorización antes de ser ejecutada. Los Filters proporcionan esta característica. Podemos ejecutarlos antes de que la API sea ejecutada. 


Algunos de los filtros importantes son:
- **UsernamePasswordAuthenticationFilter**: Realiza autenticación usando las credenciales del usuario. Ejecutado si la solicitud es POST y tiene user credentials. 
- **BasicAuthenticationFilter**: Realiza autenticación básica. Ejecutada si hay un header de solicitud de autenticación básica.
- **AnonymousAuthenticationFilter**: Si no hay credenciales de autenticación en la solicitud, esta creará un usuario anónimo. Generalmente, los usuarios anónimos tienen permitido ejecutar solicitud a APIs públicas.
- **ExceptionTranslationFilter**: No proporciona seguridad adicional. Traduce excepciones de autenticación a una HTTP response permitida.
- **FilterSecurityInterceptor**: Responsable de las decisiones de autorización. 

Estos filtros proporcionan características auxiliares:

- **HeaderWriterFilter**: Escribe headers de seguridad a la respuesta. *X-Frame-Options*, *X-XSS-Protection* y *X-Content-Type-Options*. 
- **CrsfFilter**:  Chequea por protección **CRSF**.



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc2NTEwMzE2MV19
-->