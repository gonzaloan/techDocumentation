## I18n

Código en la clase *Application.java*
```java
  @Bean
    public LocaleResolver localeResolver() {
      SessionLocaleResolver sessionLocaleResolver = 
      new SessionLocaleResolver();
      sessionLocaleResolver.setDefaultLocale(Locale.US); //Default en Inglés
      return sessionLocaleResolver;
    }

   @Bean
   public ResourceBundleMessageSource messageSource() {
     ResourceBundleMessageSource messageSource = 
     new ResourceBundleMessageSource();
     //Se sacarán datos desde messages. Si estamos en fr (francia), usaremos archivo desde message_fr.properties. Si el message no está disponible se usará el por defecto: message.properties
     messageSource.setBasenames("messages");
     messageSource.setUseCodeAsDefaultMessage(true);
     //Si no se encuentra el mensaje, se retorna por defecto el código. 
    return messageSource;
   }
```
Y en el controlador:

```java
@Autowired  
private ResourceBundleMessageSource messageSource;  
  
@GetMapping("/welcome-internationalized")  
public String msg(@RequestHeader(value = "Accept-Language",  
  required = false) Locale locale) {  
    return messageSource.getMessage("welcome.message", null,  
  locale);  
}
```


## Caching

Anotación en clase principal:

```java
	@EnableCaching
	@SpringBootApplication
    public class Application {

```
Con lo anterior, podemos añadir la anotación **@Cacheable** a los métodos que se quieran cachear: 

```java
	@Cacheable("todos")
    public List<Todo> retrieveTodos(String user) {
```
Con esto, la primera vez que se llame al método para un usuario específico, se traerá la información. En las siguientes llamadas del mismo usuario, se obtendrá la info del caché. 

También hay caching condicional, que exista sólo si alguna condición se satisface:

```java
	@Cacheable(cacheNames="todos", condition="#user.length < 10”)
    public List<Todo> retrieveTodos(String user) {
```
También hay anotaciones para añadir o quitar data de la caché:
@CachePut,  @CacheEvict, @Caching


## Configuration Properties

Si tenemos un valor que obtenemos desde el properties, podemos inyectarlo usando la anotación **@Value("${valor.properties}")**, pero si ese valor se usa en más de un lugar, lo mejor es utilizar un **ConfigurationProperties**. , así definimos un Bean que puede usarse en todos lados.

```java
@Component
    @ConfigurationProperties("application")
    public class ApplicationConfiguration {
      private boolean enableSwitchForService1;
      private String service1Url;
      private int service1Timeout;
      public boolean isEnableSwitchForService1() {
        return enableSwitchForService1;
      }
     public void setEnableSwitchForService1
     (boolean enableSwitchForService1) {
        this.enableSwitchForService1 = enableSwitchForService1;
      }
     public String getService1Url() {
       return service1Url;
     }
     public void setService1Url(String service1Url) {
       this.service1Url = service1Url;
     }
     public int getService1Timeout() {
       return service1Timeout;
     }
     public void setService1Timeout(int service1Timeout) {
       this.service1Timeout = service1Timeout;
    }
  }
```

En este código:
 - **@ConfigurationProperties("application")**: Es la configuración para la configuración externalizada. Podemos añadir esta anotación a cualquier clase para hacer un bind a un properties externo.
 - Se están definiendo múltiples valores en el bean.
 - Getters and Setters son necesitados a partir de que el binding ocurre a parte

Para que la clase obtenga estos valores, deben estar escritos en **application.properties** de esta manera:

```java
    application.enableSwitchForService1=true
    application.service1Url=http://abc-dev.service.com/somethingelse
    application.service1Timeout=250
```
Ahora se puede usar los valores así:

```java
  @Autowired
      private ApplicationConfiguration configuration;
      public String retrieveSomeData() {
        // Logic using the url and getting the data
        System.out.println(configuration.getService1Timeout());
        System.out.println(configuration.getService1Url());
        System.out.println(configuration.isEnableSwitchForService1());
        return "data from service";
      }
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbMzcxMTc4NjQ0XX0=
-->