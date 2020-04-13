# Testing de Software 

## ¿Por qué es importante las testear el software?

- **Reducir el número de bugs**
- **Mejora la calidad del código, al identificar cada defecto que pueda surgir al modificar o refactorizar el código.**
- **Encontrar bugs temprano en el desarrollo.**
- **Facilita la integración y cambios.**
- **Entrega documentación**
- **Simplifica el proceso de debuggeo**

## Terminología 

### Unit Test / Pruebas Unitarias
- Diseñado para testear secciones específicas de código, métodos.
- Porcentaje de líneas de código testeado debe estar entre 70% u 80%.
- Debe ser rápido y pequeño.
- No debe tener dependencias externas. (No DB, no Spring Context, etc)

### Integration Test / Pruebas de Integración
- Diseñado para testear comportamientos entre objetos y partes de todo el sistema.
- Mucho más grande Scope.
- Puede incluir Spring Context, DB.
- Puede ser mucho más lento que las pruebas unitarias.

### Functional Test / Pruebas Funcionales
Testear la aplicación corriendo.
- Testear endpoints
- Pruebas de Caja Negra
- Pruebas de Caja Blanca

## Dependencias

- **Sprint Boot Starter Test**: repositorio: *spring-boot-starter-test*
- **Mockito**:  Librería para crear Mocks. Un mock es un objeto de prueba que imita el comportamiento de objetos reales de forma controlada. 
- **AssertJ**: Librería que permite comparar resultados de manera entendible.

## Anotaciones JUnit
Hay diversas anotaciones que ayudarán al proceso de hacer pruebas. 

- **@Test**: Marca un método como un Test.
- **@BeforeClass**: El método que posea esta anotación correrá **una vez** antes de cualquiera de los métodos test de la clase. Es útil para hacer un setup general.
- **@Before**: Método correrá antes que cada método de test. 
- **@AfterClass**: Método se ejecutará una vez después de que todos los test sean ejecutados.
- **@After**: Método se ejecutará una vez después de que cada test termine.

Las demás anotaciones serán explicadas a medida que vayan siendo mostradas en los ejemplos.

## Estructura de las pruebas para los proyectos de Sodexo

La nomenclatura a usar, tiene las siguientes consideraciones, esto es importante que se sigan para funcionar correctamente con Jenkins.

Es importante hacer una separación de lo que son las pruebas. Vamos a tener pruebas unitarias y de integración.



> Importante notar que habrá una carpeta **integration** dentro del paquete **test**. Esto es relevante porque las pruebas de integración ocupan datos reales, por lo tanto, no servirán en ambiente de QA y PRD.

- Se debe tener la misma estructura de packages en **test** que la que existe en **main**. Por ejemplo:
Si estamos haciendo tests de la clase **User.java** ubicada en esta ruta:
```
cl/sodexobeneficios/card_transactions/controller/CardTransactionController.java
```
Las pruebas **unitarias** de dicho elemento deberían estar en una ruta como esta:
```
test/java/cl/sodexobeneficios/card_transactions/controller/CardTransactionControllerUnitTest.java
```
Las pruebas de **integración** (si son endpoints) tendrían que estar en esta ruta:

`test/java/integration/CardTransactionControllerIntegrationTest.java`


- Todas las **Pruebas Unitarias** deben tener el sufijo **UnitTest**.
- Todas las **Pruebas de Integración** deben tener el sufijo **IntegrationTest**
- Buscar siempre tener el mayor porcentaje de cobertura posible de las pruebas, en el mejor de los casos 70% u 80% está perfecto. Para esto, se puede utilizar el plugin **EclEmma** desde marketplace de Spring Tool Suite, que mostrará en una pestaña del IDE la cantidad de cobertura. También es posible crear un reporte que se puede subir a JIRA como prueba de la cobertura que están entregando, esto se hace con JaCoCo.  ([Instalación JaCoCo](https://www.codeproject.com/Articles/832744/Getting-Started-with-Code-Coverage-by-Jacoco))
- Para usuarios de **IntelliJ**, el IDE trae la opción de ejecutar las pruebas con covertura automáticamente.

## Cómo testear Microservicios

Para este ejemplo, usaremos el proyecto **Card Transactions**.

### Pruebas Unitarias Controlador

Para este ejemplo, probaremos el controlador **CardTransactionController.java**, el método que testearemos será:

```java
@GetMapping(value = "/beneficiary/cards/balance/{pan}", produces = MediaType.APPLICATION_JSON_VALUE)  
public ResponseEntity<Long> findBeneficiaryCardBalance(@PathVariable(value = "pan") Long pan,  
  @RequestParam(value = "beneficiaryFiscalId", required = true) String beneficiaryFiscalId,  
  @RequestParam(value = "companyFiscalId", required = true) String companyFiscalId,  
  @RequestParam(value = "serviceId", required = true) Long serviceId) throws Throwable {  
  
   LOG.info("CONTROLLER - CardTransactionService.findBeneficiaryCardBalance({},{},{},{})", beneficiaryFiscalId,  
  companyFiscalId, pan, serviceId);  
  Long balance = cardTransactionService.findBeneficiaryCardBalance(beneficiaryFiscalId, companyFiscalId, pan,  
  serviceId);  
  LOG.info("CONTROLLER - Saldo encontrado {}, Beneficario: {}, Empresa: {}, PAN: {}, Servicio {})", balance,  
  beneficiaryFiscalId, companyFiscalId, pan, serviceId);  
 return new ResponseEntity<>(balance, HttpStatus.OK);  
}
```

> La ruta es: cl.sodexobeneficios.card_transactions.controller.CardTransactionController.java

- El primer paso es crear el archivo de prueba unitaria para esta prueba. Para ello creamos un archivo en el package de test: 
**test/java/cl/sodexobeneficios.card_transactions/CardTransactionControllerUnitTest.java**

- Una vez creada la clase, es necesario definir los mock. Un mock es un objeto falso, que crearemos para evitar que se llame a base de datos, repositorios, feigns, etc. Simulamos todo lo que vaya a buscar datos a otro lugar que no sea ese controller. En este caso, hay una llamada a **cardTransactionsService.findBeneficiarioCardBalance**, por lo que debemos crear un Mock de ese Service para simular una llamada a dicho servicio.

- Probaremos una llamada al método del controlador, cuando el balance está OK, simularemos un llamado al servicio y que este retorne **10000**. 

```java
@RunWith(SpringJUnit4ClassRunner.class)  
public class CardTransactionControllerUnitTest {  
  
    @Mock  
  private CardTransactionService cardTransactionService;  
  
  @InjectMocks  
  private CardTransactionController cardTransactionController;  
  
  
  @Before  
  public void setUp() throws Exception {  
        MockitoAnnotations.initMocks(this);  
  }  
  
    @Test  
  public void findBeneficiaryCardBalance_when_beneficiary_has_balance_ok() throws Throwable {  
  
        Mockito.when(cardTransactionService.findBeneficiaryCardBalance("176454040","9882182",123123123L,16L)).thenReturn(10000L);  
  
  ResponseEntity <Long> result = cardTransactionController.findBeneficiaryCardBalance(123123123L, "176454040", "9882182", 16L);  
  
  //Condiciones de Prueba  
  assertThat(result).isNotNull();  
  assertThat(result.getBody()).isInstanceOf(Long.class);  
  assertThat(result.getBody()).isEqualTo(10000L);  
  }  
}
```
En esta clase de Test podemos ver lo siguiente:

- ` @Mock  private CardTransactionService cardTransactionService; `: Este es el servicio del cual haremos un Mock. Así evitaremos realizar **realmente** todas las llamadas al servicio y podremos simular una respuesta. 

- `@InjectMocks private CardTransactionController cardTransactionController;`: El mock que creamos en el punto anterior debe ser inyectado en algún lugar, en este caso es en el controlador, que es del lugar de donde se llamada al Mock.

- `public void setUp()`: Este método tiene la anotación **@Before** que indica que se ejecutará antes de iniciar las pruebas. Este método es típicamente creado para insertar todas las configuraciones u objetos que serán usados a lo largo de las pruebas.
- `MockitoAnnotations.initMocks(this);`: Inicializamos los mocks.

- `@Test`: Cada una de las pruebas debe tener la anotación *@Test* para indicar que es una prueba. 

- `Mockito.when(cardTransactionService.findBeneficiaryCardBalance("176454040","9882182",123123123L,16L)).thenReturn(10000L);`: Acá creamos una llamada falsa al servicio **findBeneficiaryCardBalance**. Lo que estamos haciendo es indicar que dentro de esta prueba, cuando se llame al método con los datos indicados (*"176454040","9882182",123123123L,16L*), este retornará desde el servicio **10000L**. Así nos evitamos hacer una llamada real al servicio con Mocks. 
- `ResponseEntity <Long> result = cardTransactionController.findBeneficiaryCardBalance(123123123L, "176454040", "9882182", 16L);`: Acá estamos haciendo la prueba al controlador, estamos testeando los datos y almacenando la respuesta del controlador. Esta es la prueba como tal. 
- `assertThat(result).isNotNull();`: Finalmente aquí se escriben las condiciones que queremos probar, analizamos el valor de **result**, indicando que este no debe ser nulo, debe ser de tipo Long.class y que debe ser igual a **10000L**.


### Pruebas Unitarias Service

Además, se deben hacer pruebas unitarias de los servicios, lo cual es bastante similar al caso de arriba. 

Para el método del controlador testeado anteriormente, se hacía una llamada a **CardtransactionServiceImpl**, específicamente en *findBeneficiaryCardBalance*.

El método del servicio es el siguiente:

```java
@Override  
public Long findBeneficiaryCardBalance(String beneficiaryFiscalId, String companyFiscalId, Long pan,  
  Long serviceId) {  
  
   LOG.info("SERVICE - MulticajaService.getAvailableBalance({},{},{},{})", beneficiaryFiscalId, serviceId,  
  companyFiscalId, pan);  
  
 return multicajaService.getAvailableBalance(beneficiaryFiscalId, serviceId, companyFiscalId, pan);  
}
```

> Si bien en este caso, no hay lógica dentro de este método, es posible que más adelante algún desarrollador modifique este método, y eso podría causar que los resultados no sean los esperados en el controlador. Por eso es importante hacer pruebas, porque en el caso de que se modifique en el futuro este servicio, las pruebas fallarán y será posible detectar a tiempo que hay inconsistencias. Esta es la importancia de las pruebas.

Para las pruebas, seguimos el ejemplo anterior y creamos un archivo **CardTransactionServiceImplUnitTest.java** en la ruta homologada en src: **test/java/cl.sodexobeneficios.card_transactions/service/impl/CardTransactionServiceImplUnitTest**

Dentro de ese archivo de pruebas podríamos definir las siguientes pruebas:

```java
@RunWith(SpringJUnit4ClassRunner.class)  
public class CardTransactionServiceImplUnitTest {  
  
  @Mock  
  private MulticajaServiceImpl multicajaService;  
  
  @InjectMocks  
  private CardTransactionServiceImpl cardTransactionService;  
  
 private Long balanceResult;  
  
  @Before  
  public void setUp() throws Exception {  
      balanceResult = 10000L;  
	  MockitoAnnotations.initMocks(this);  
  }  
  
    @Test  
  public void findBeneficiaryCardBalance_when_client_has_balance() {  

     Mockito.when(cardTransactionService.findBeneficiaryCardBalance("98221719", "645124151", 66666666666L, 16L)).thenReturn(balanceResult);  
  
	 Long result = multicajaService.getAvailableBalance("98221719", 16L, "645124151", 66666666666L);  
  
	  assertThat(result).isNotNull();  
	  assertThat(result).isInstanceOf(Long.class);  
	  assertThat(result).isEqualTo(balanceResult);  
  }  
  
  
    @Test  
  public void findBeneficiaryCardBalance_when_client_has_no_balance() {  
        Mockito.when(cardTransactionService.findBeneficiaryCardBalance("98221719", "645124151", 66666666666L, 16L)).thenReturn(balanceResult);  
  
  Long result = multicajaService.getAvailableBalance("982217199", 16L, "645124151", 66666666666L);  
  
  assertThat(result).isNotNull();  
  assertThat(result).isInstanceOf(Long.class);  
  assertThat(result).isEqualTo(0);  
  }  
  
    @Test  
  public void findBeneficiaryCardBalance_when_card_transaction_fails() {  
  
        Mockito.when(cardTransactionService.findBeneficiaryCardBalance("98221719", "645124151", 66666666666L, 16L)).thenThrow(new RuntimeException());  
  Long result = multicajaService.getAvailableBalance("982217199", 16L, "645124151", 66666666666L);  
  
  assertThat(result).isNotNull();  
  assertThat(result).isInstanceOf(Long.class);  
  assertThat(result).isEqualTo(0);  
  }  
      
}
```

Analicemos los puntos importantes de las pruebas:

- `@Mock  private MulticajaServiceImpl multicajaService;`: Dentro de la implementación del servicio, se llama al método *getAvailableBalance*  de MulticajaService. Por lo que ese sería nuestro objeto a **falsear**, nuestro mock.

- `@InjectMocks  private CardTransactionServiceImpl cardTransactionService;`: Los mocks creados, debemos inyectarlos en el servicio que estamos probando **CardTransactionServiceImpl**.

- `balanceResult = 10000L;`: Dentro del *setup* de nuestra clase de pruebas (recordemos que lo que está dentro de la anotación *before*, se ejecuta antes de todas las demás pruebas). balanceResult es una variable que usaremos como valor de prueba más adelante.
- `MockitoAnnotations.initMocks(this);`: Inicializamos los valores que seteamos como mocks. 
- `Mockito.when(cardTransactionService.findBeneficiaryCardBalance("98221719", "645124151", 66666666666L, 16L)).thenReturn(balanceResult);`: Creamos una respuesta falsa. Le estamos diciendo a Mockito que cuando se ejecute el servicio **findBeneficiaryCardBalance** con los valores entregados en los parámetros, va a retornar nuestro **balanceResult** (que definimos en nuestro *Before*). Es decir, estamos forzando la respuesta.
- `Long result = multicajaService.getAvailableBalance("98221719", 16L, "645124151", 66666666666L);` : Acá se realiza la llamada del servicio que queremos probar. Usando los mismos parámetros que usamos en Mockito.when, debe traernos el resultado que le indiquemos. Almacenamos el resultado en una variable. 
- `assertThat`: Con estas palabras reservadas, verificamos que el resultado no sea null, que sea de tipo Long, y que sea igual a resultBalance. 


> Hemos realizado pruebas correctas a nuestro Service. Si bien ahora no sirve de mucho, ya que el servicio retorna sólo el balance result, y no hay más lógica de negocio metida entre medio. Más adelante cuando se modifique algo, si que servirá para evitar que creemos bugs, o problemas no deseados.

## Spring Rest Docs


### Requisitos
Para utilizar Spring Rest Docs, primero debemos actualizar las dependencias en el POM, se utilizará la versión 2.0.0
```xml
<dependency>  
	 <groupId>org.springframework.restdocs</groupId>  
	 <artifactId>spring-restdocs-mockmvc</artifactId>  
	 <version>2.0.0.RELEASE</version>  
</dependency>  
<dependency>  
	 <groupId>org.springframework.restdocs</groupId>  
	 <artifactId>spring-restdocs-core</artifactId>  
	 <version>2.0.0.RELEASE</version>  
</dependency>
```
Además, el plugin para generar los html a partir de los snippets:
```xml
<plugin>  
	 <groupId>org.asciidoctor</groupId>  
	 <artifactId>asciidoctor-maven-plugin</artifactId>  
	 <version>1.5.6</version>  
	 <executions> 
		 <execution> 
			 <id>generate-docs</id>  
			 <phase>package</phase>  
			 <goals> 
				 <goal>process-asciidoc</goal>  
			 </goals> 
			 <configuration> 
				 <backend>html</backend>  
				 <doctype>book</doctype>  
				 <attributes> 
					 <snippets>${project.build.directory}/generated-snippets</snippets>  
				 </attributes> 
				 <sourceDirectory>src/docs/asciidocs</sourceDirectory>  
				 <outputDirectory>target/generated-docs</outputDirectory>  
			 </configuration> 
			</execution> 
		</executions>
</plugin>
```
También se recomiendan herramientas de previsualizado, que permiten tener una vista previa de la documentación mientras se escribe:
**INTELLIJ** : https://plugins.jetbrains.com/plugin/7391-asciidoc
**STS**: https://marketplace.eclipse.org/content/asciidoctor-editor

### Generación de Snippets
Para generar la documentación, primero se deben crear los snippets a partir de las pruebas en JUNIT.

Los *Snippets* son pequeños trozos de documentación que según lo programemos en las pruebas generarán información como curls requests, información de las respuestas, los parámetros del servicio, los campos de respuesta, etc. 

Finalmente, es nuestra tarea ordenar estos snippets en un archivo que luego se transformará en la documentación. 

#### Pruebas 
Para empezar, es necesario tomar la prueba a algún controlador, y se debe setear las siguientes anotaciones:
```java
private MockMvc mockMvc;  
  
@Autowired  
private WebApplicationContext wac;  
  
@Rule  
public JUnitRestDocumentation restDocumentation = new JUnitRestDocumentation("target/generated-snippets");  
  
@Before  
public void setUp() {  
    this.mockMvc = MockMvcBuilders.webAppContextSetup(wac)  
            .apply(documentationConfiguration(restDocumentation))  
            .build();  
}
```

Esto hará que los snippets generados con las pruebas se almacenen en la carpeta **target/generated-snippets** estructurados según se indique en cada test que se desee documente un endpoint.

Una prueba de un controlador común y corriente tiene el siguiente formato:

```java
@Test  
public void isBannerCheckedByUser_should_return_ok_when_called_without_errors() throws Exception{  

mvc.perform(MockMvcRequestBuilders.get("/banner/323031393032303431363040/audit/fiscalId/165775198?platformId=3")  
            .contentType(MediaType.APPLICATION_JSON))  
            .andExpect(MockMvcResultMatchers.status().isOk())  
            .andExpect(MockMvcResultMatchers.jsonPath("success").isBoolean())  
            .andExpect(MockMvcResultMatchers.jsonPath("success").value(true))  
            .andDo(MockMvcResultHandlers.print());  
}
```
Si analizamos esta prueba, la sintaxis de **BDDMockito.given** indica que si se llama al método *isBannerCheckedByUser* en el controlador con los parámetros ahí indicados, este retornará una ResponsEntity con una respuesta y status OK.

Por otro lado, **mvc.perform** hará una petición get al endpoint, con los parámetros ahí indicados y esperará que la respuesta tenga status OK, que sea un json, y que ese json tenga un campo **success** quesea boolean y sea true.

Para poder generar los snippets es necesario agregar la llamada a un par de métodos:
```java
@Test  
public void isBannerCheckedByUser_should_return_ok_when_called_without_errors() throws Exception{  

  mvc.perform(RestDocumentationRequestBuilders.get("/banner/323031393032303431363040/audit/fiscalId/165775198?platformId=3")  
     .contentType(MediaType.APPLICATION_JSON))  
     .andExpect(MockMvcResultMatchers.status().isOk())  
     .andExpect(MockMvcResultMatchers.jsonPath("success").isBoolean())  
     .andExpect(MockMvcResultMatchers.jsonPath("success").value(true))  
     .andDo(MockMvcResultHandlers.print())  
     //Spring Rest Docs  
	 .andDo(document("banner-is-checked",  
		  pathParameters(  	
			  parameterWithName("bannerId").description("Identificador del Banner en 			la base de datos que se verificará."),  
			  parameterWithName("fiscalId").description("Rut del usuario que se verificará haya visualizado el banner del campo BannerId.")  
	        ),  
		  requestParameters(  
              parameterWithName("platformId").description("Identificador de la plataforma. ")),  
		  responseFields(  
              fieldWithPath("success").description("Retorna un true si es que el Banner ya ha sido visto por el usuario, o en su defecto un false"),  
			  fieldWithPath("result").description("Campo no utilizado, siempre retorna un null"))));
}
```
Debajo del comentario que dice **Spring Rest Docs**, está la información necesaria para crear la documentación. En la primera línea luego de este marcado, se puede apreciar:

- **.andDo(document("banner-is-checked-by-user")**: Esta línea es la que creará el snippet con el nombre que declaremos acá. Este snippet luego va a contener toda la información que le indiquemos más abajo. 
- **pathParameters/requestParameters**: Indican los campos del servicio:
			- **PathParams** son los parámetros de ruta, como cuando llamamos a alguna URL particular, ejemplo *https://swapi.co/api/people/10*. 
			 - **Request Params**: Son los parámetros del tipo ? &: http://swapi.co/api/planets/1/?format=wookiee 
- **responseFields**: Describe la respuesta que tendrá el servicio. Es importante destacar que si es utilizado, se debe documentar todos los campos de la respuesta, o el test dará error.
- **Request Fields**: Para datos enviados a través de un post

Una vez documentado los métodos que se deseen probar, al correr los tests, se generará la carpeta **target/generated-snippets** con unacarpeta con el nombre que hayamos puesto en el test.

### Creación de la documentación de la API a partir de Snippets
Ahora se debe crear un archivo .adoc en la ruta **src/docs/asciidocs**,  con el nombre que tendrá el archivo html generado.  
Se debe tener un archivo base de la documentación para cada microservicio, llamado **index.adoc** que tendrá inyectado dentro cada controlador del proyecto. 

El archivo index.adoc tiene la siguiente estructura:

```ascii
= Documentación de Microservicio USERS  
Gonzalo Muñoz<gonzalo.munoz.ext@sodexo.com>  
22/03/2019  
:appversion: 1.0.0  
:toc: left  
:sectnums:  
  
== Introducción  
  
Microservicio para todo el manejo de cuentas de usuarios externas e internas  
Esta es la documentación de los endpoints  
  
=== URL Base  
La *URL Base* es la raíz de todo el microservicio, si se realiza una petición y retorna un error de tipo 404, siempre es necesario verificar la URL Base, según los distintos ambientes:  
  
*Localhost*:  
-----------------  
localhost:10000/  
-----------------  
  
*Desarrollo*:  
-----------------  
ceclsvc-awsap48.ce.sdxcorp.net:10001/  
-----------------  
  
*Testing*:  
-----------------  
http://ceclsvc-dacap09.ce.sdxcorp.net:10000/user/  
-----------------  
  
*Producción*:  
-----------------  
http://ceclsvc-dacap41.ce.sdxcorp.net:10000/  
-----------------  
  
=== Encodings  
  
JSON es el formato estándar de datos que provee el microservicio Users.  
  
== /banner  
Servicios referentes a banners.  
include::banner.adoc[]  
== /card  
  
== /menu  
  
== /user
```
Es importante destacar que *include::banner.adoc[]* marca que se hará una inyección de otro archivo adoc dentro del documento, por lo que debe estar bien documentado, incluyendo la identación que aquí se define:


```
=== Endpoints  
* /banner/:bannerId/audit/fiscalId/:fiscalId -- Verificar que banner haya sido visto por usuario  
* /banner/changeSmartBannerToViewed -- Cambiar el estado de un banner a visto  
  
=== Verificar que banner ha sido visto por usuario  
Servicio que verifica si el usuario enviado por parámetro ha sido visto por el usuario indicado.  
  
==== Request  
include::{snippets}/banner-is-checked/http-request.adoc[]  
  
==== Curl Request  
include::{snippets}/banner-is-checked/curl-request.adoc[]  
  
==== Httpie Request  
include::{snippets}/banner-is-checked/httpie-request.adoc[]  
  
==== Path Parameters  
include::{snippets}/banner-is-checked/path-parameters.adoc[]  
  
==== Request fields  
include::{snippets}/banner-is-checked/response-fields.adoc[]  
  
==== Response fields  
include::{snippets}/banner-is-checked/response-fields.adoc[]  
  
==== Response Body  
include::{snippets}/banner-is-checked/response-body.adoc[]  
  
==== Http Response  
include::{snippets}/banner-is-checked/http-response.adoc[]  
  
=== Cambiar el estado de un banner a visto  
Servicio que cambia el estado de un banner a visto. Genera una entrada en la base de datos con el banner y el usuario que lo vio.  
  
==== Request  
include::{snippets}/banner-change-smartbanner-to-viewed/http-request.adoc[]  
  
==== Request Body  
include::{snippets}/banner-change-smartbanner-to-viewed/request-body.adoc[]  
  
==== Curl Request  
include::{snippets}/banner-change-smartbanner-to-viewed/curl-request.adoc[]  
  
==== Httpie Request  
include::{snippets}/banner-change-smartbanner-to-viewed/httpie-request.adoc[]  
  
==== Path Parameters  
include::{snippets}/banner-change-smartbanner-to-viewed/path-parameters.adoc[]  
  
==== Response fields  
include::{snippets}/banner-change-smartbanner-to-viewed/response-fields.adoc[]  
  
==== Response Body  
include::{snippets}/banner-change-smartbanner-to-viewed/response-body.adoc[]  
  
==== Http Response  
include::{snippets}/banner-change-smartbanner-to-viewed/http-response.adoc[]
```
Aquí se define todos los snippets creados en la carpeta target y los inyectamos en el documento. 


                                        
Hola que tal soy Gonzalo, Este es mi teclado, ba

> Written with *Gonzalo Muñoz*.

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5NDczNDY5NjgsMzE4NjU3MjE5LC0zOT
gzNjE3MzEsLTk1NTcwOTIyMywtMTg0MTg3NTA2OV19
-->