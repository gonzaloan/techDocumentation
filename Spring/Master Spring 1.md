# 1 - Validaciones

## Hibernate Validator

```xml
    <dependency> 
      <groupId>org.hibernate</groupId> 
      <artifactId>hibernate-validator</artifactId> 
      <version>5.0.2.Final</version> 
    </dependency>
```

### Validaciones Simples en el bean

La API de validación especifica un número de validaciones que pueden ser especificadas en los atributos de los beans:

```java
   @Size(min = 6, message = "Enter at least 6 characters") 
   private String name; 
   @Size(min = 6, message = "Enter at least 6 characters") 
   private String userId; 
   @Size(min = 8, message = "Enter at least 8 characters") 
   private String password; 
   @Size(min = 8, message = "Enter at least 8 characters") 
   private String password2;
```

Otras validaciones:

- `@NotNull`: No puede ser null
- `@Size(min =5, max = 50)`: Tamaño máximo de 50 caracteres y minimo 5 caracteres.
- `@Past`: Debe ser una fecha en el pasado
- `@Future` Debe ser una fecha en el futuro
- `@Pattern`: Debe seguir el patrón proveído por la expresión regultar.
- `@Max`: Valor máximo del campo
- `@Min`: Valor mínimo para el campo.

En el controller hay que validar esto:

```java
 @RequestMapping(value = "/create-user-with-validation",  
    method = RequestMethod.POST) 
    public String addTodo(@Valid User user, BindingResult result) { 
      if (result.hasErrors()) { 
        return "user"; 
       } 
      logger.info("user details " + user); 
      return "redirect:list-users"; 
    }
```

Con la anotación `@Valid`, Spring valida el bean, el resultado de la validación se almacena en `BindingResult`.

## Validaciones Custom

Validaciones más complejas pueden ser implementadas usando `@AssertTrue`. La siguiente lista es un ejemplo añadido a la clase `User`:

```java
    @AssertTrue(message = "Password fields don't match") 
    private boolean isValid() { 
      return this.password.equals(this.password2); 
    }
```

# 2- Model Attributes

Algunos formularios web contienen un número de valores dropdown, como lista de países, lista de estados, etc. Esta lista de valores necesitan estar disponibles en el modelo así la vista puede mostrar la lista. Este tipo de cosas están pobladas en el modelo usando métodos marcados con la anotación `@ModelAttribute`.

Hay dos variaciones posibles. En el ejemplo siguiente, el método retorna el objeto que necesita ser puesto en el modelo

```java
    @ModelAttribute 
    public List<State> populateStateList() { 
      return stateService.findStates(); 
     }
```
El objetivo en este ejemplo es usado para añadir múltiples atributos al modelo:

```java
    @ModelAttribute 
    public void populateStateAndCountryList() { 
      model.addAttribute(stateService.findStates()); 
      model.addAttribute(countryService.findCountries()); 
     }
```

# 3- Atributos de sesión

Hay valores como los de un usuario específico web, que podría no cambiar a través de las peticiones. Estos valores se almacenan típicamente en una sesión HTTP. Spring MVC proporciona una notación `@SessionAttributes` para especificar los atributos que podrían ser almacenados en la sesión.

Veamos el siguiente ejemplo:

```java
    @Controller 
    @SessionAttributes("exampleSessionAttribute") 
    public class LoginController {
```

## Poner un atributo en sesión

Una vez que definimos un atributo en la anotación `@SessionAttributes` está automáticamente añadido a la sesión si el mismo atributo es añadido al modelo.

En el ejemplo anterior, si ponemos un atributo con el nombre `exampleSessionAttribute` dentro del modelo, sería automáticamente almacenado en la sesión:

```java
model.put("exampleSessionAttribute", sessionValue);
```

## Leyendo un atributo desde la sesión

Este valor puede ser accedido en otros controladores especificando la anotación `@SessionAttributes`:

```java
    @Controller 
    @SessionAttributes("exampleSessionAttribute") 
    public class SomeOtherController {
```

El valor del atributo de la sesión será directamente disponible en todos los objetos del modelo. Así, puede ser accedido desde el modelo:

```java
Value sessionValue =(Value)model.get("exampleSessionAttribute");
```

## Removiendo un atributo desde la sesión

Es importante remover valores de la sesión cuando ya no son necesitados. Hay dos formas para ello. La primera está demostrada en el siguiente snippet. Usa el método `removeAttribute` disponible en la clase `WebRequest`:

```java
    @RequestMapping(value="/some-method",method = RequestMethod.GET) 
    public String someMethod(/*Other Parameters*/  
    WebRequest request, SessionStatus status) { 
      status.setComplete(); 
      request.removeAttribute("exampleSessionAttribute",
      WebRequest.SCOPE_SESSION); 
       //Other Logic
    }
```

# 4- InitBinders

Los formularios web clásicos, tienen datos, tipos de moneda y cantidades. Los valores en los formularios necesitan estar respaldados en los objetos del backend. La customización de cómo ocurre el binding se puede modificar con la anotación **@InitBinder**

La customización puede acerse en un controlador específico o en un set de controladores usando Handler Advice. Este ejemplo muestra como configurar el formato por defecto de una fech apara usar un binding de formulario

```java
    @InitBinder 
    protected void initBinder(WebDataBinder binder) { 
      SimpleDateFormat dateFormat = new SimpleDateFormat("dd/MM/yyyy"); 
      binder.registerCustomEditor(Date.class, new CustomDateEditor( 
      dateFormat, false)); 
    }
```

# 5- La anotación @ControllerAdvice

Alguna funcionalidad que definimos en el nivel del controlador puede ser común a través de la aplicación. Por ejemplo algún formato de fecha. Así, el `@InitBinder` que definimos antes puede ser aplicable en toda la aplicación. Para esto se utiliza **@ControllerAdvice**, que hace la funcionalidad común a través de todos los Request Mappings.

En el siguiente ejemplo usamos un **@ControllerAdvice** en la clase y definimos el método con **@InitBinder**. Por defecto, el binding definido en este método es aplicable en todos los request mappings:

```java
    @ControllerAdvice 
    public class DateBindingControllerAdvice { 
      @InitBinder 
      protected void initBinder(WebDataBinder binder) { 
        SimpleDateFormat dateFormat = new  
        SimpleDateFormat("dd/MM/yyyy"); 
        binder.registerCustomEditor(Date.class,  
        new CustomDateEditor( 
          dateFormat, false)); 
        } 
     }
```
El controller advice puede ser también usado para definir atributos de modelo comúnes (**@ModelAttribute**) y manejo común de excepciones ( **@ExceptionHandler**). Todo lo que se neesita es crear métodos marcados con anotaciones apropiadas. 


# 6- Manejo de Excepciones

## Manejo de excepciones a través de controllers

```java
    @ControllerAdvice 
    public class ExceptionController { 
      private Log logger =  
      LogFactory.getLog(ExceptionController.class); 
      @ExceptionHandler(value = Exception.class) 
      public ModelAndView handleException 
      (HttpServletRequest request, Exception ex) { 
         logger.error("Request " + request.getRequestURL() 
         + " Threw an Exception", ex); 
         ModelAndView mav = new ModelAndView(); 
         mav.addObject("exception", ex); 
         mav.addObject("url", request.getRequestURL()); 
         mav.setViewName("common/spring-mvc-error"); 
         return mav; 
        } 
     }
```
Algunas cosas importantes a notar:

- `@ControllerAdvice`: Indica que es aplicable a todos los controladores
- `@ExceptionHandler(value = Exception.class)` : Todo método con esta anotación, será llamada cuando una excepción del tipo o subtipo de la clase específica ( `Exception.class`) sea lanzadas en los controladores.
- `public ModelAndView handleException (HttpServlerRequest request, Exception ex)`: La excepción que es lanzada es inyectada en la variable Exception. El método es declarado con un ModelAndView para poder retornar un modelo con los detalles de la excepción y la vista.
- `mav.addObject("exception", ex)`: Añade la excepción al modelo para que se puedan mostrar detalles en la vista.
- `mav.setViewName("common/spring-mvc-error")`: La vista de la excepción.

## La vista Error

Cuando una excepción se gatille, `ExceptionController` redirige al usuario al la vista spring-mvc-error de `ExceptionController` después de popular el modelo con detalles de la excepción. La vista es algo así:
```html
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%> 
    <%@page isErrorPage="true"%> 
    <h1>Error Page</h1> 
     URL: ${url} 
    <BR /> 
    Exception: ${exception.message} 
   <c:forEach items="${exception.stackTrace}"  
      var="exceptionStackTrace">     
      ${exceptionStackTrace}  
   </c:forEach>
```
### Manejo de excepciones específicas en un controlador.

En algunas situaciones se necesita un manejo específico de errores en un controlador. Esta situación se puede manejar fácilmente implementando un método anotado con `@ExceptionHandler(value = Exception.class)`. 

## Manejo de excepciones Microservicios


Se crea una Excepcion que extienda de otra del mismo tipo:

Custom Exception es:

```java
    public class CustomException extends RuntimeException {
      public CustomException(String msg) {
        super(msg);
      }
    }
    
Podemos definir la estructura de un mensaje de excepción:


```java
    public class ExceptionResponse {
      private Date timestamp = new Date();
      private String message;
      private String details;

      public ExceptionResponse(String message, String details) {
        super();
        this.message = message;
        this.details = details;
       }

      public Date getTimestamp() {
        return timestamp;
      }

      public String getMessage() {
        return message;
      }

      public String getDetails() {
        return details;
      }
     }
```
Cuando ocurra *CustomException* queremos que retorna una respuesta usando la *ExceptionResponse* que creamos arriba. 

Podemos crear un manejo global para CustomException.class:

```java
    @ControllerAdvice
    @RestController
    public class RestResponseEntityExceptionHandler 
      extends  ResponseEntityExceptionHandler 
      {
        @ExceptionHandler(CustomException.class)
        public final ResponseEntity<ExceptionResponse> 
        todoNotFound(CustomException ex) {
           ExceptionResponse exceptionResponse = new ExceptionResponse(ex.getMessage(), 
           "Any details you would want to add");
           return new ResponseEntity<ExceptionResponse>
           (exceptionResponse, new HttpHeaders(), 
           HttpStatus.NOT_FOUND);
         }
     }
```



```
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkxOTk5NjM5XX0=
-->