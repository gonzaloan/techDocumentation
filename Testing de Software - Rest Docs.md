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

- **Sprint Boot Starter Test**: repositorio: *spring-boot-starter-test*. En algunas versiones ya incluye a Mockito y a otras librerías útiles.
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

## Estructura de las pruebas

- Buscar siempre tener el mayor porcentaje de cobertura posible de las pruebas, en el mejor de los casos 70% u 80% está perfecto. Para esto, se puede utilizar el plugin **EclEmma** desde marketplace de Spring Tool Suite, que mostrará en una pestaña del IDE la cantidad de cobertura. También es posible crear un reporte que se puede subir a JIRA como prueba de la cobertura que están entregando, esto se hace con JaCoCo.  ([Instalación JaCoCo](https://www.codeproject.com/Articles/832744/Getting-Started-with-Code-Coverage-by-Jacoco))
- Para usuarios de **IntelliJ**, el IDE trae la opción de ejecutar las pruebas con covertura automáticamente.

## Cómo testear Microservicios

Para este ejemplo, usaremos el Microservicio **Cards**.

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

> La ruta es: cl.testproject.card_transactions.controller.CardTransactionController.java

- El primer paso es crear el archivo de prueba unitaria para esta prueba. Para ello creamos un archivo en el package de test: 
**test/java/cl/testproject.card_transactions/CardTransactionControllertTest.java**

- Una vez creada la clase, es necesario definir los mock. Un mock es un objeto falso, que crearemos para evitar que se llame a base de datos, repositorios, feigns, etc. Simulamos todo lo que vaya a buscar datos a otro lugar que no sea ese controller. En este caso, hay una llamada a **cardTransactionsService.findBeneficiarioCardBalance**, por lo que debemos crear un Mock de ese Service para simular una llamada a dicho servicio.

- Probaremos una llamada al método del controlador, cuando el balance está OK, simularemos un llamado al servicio y que este retorne **10000**. 

```java
@RunWith(SpringJUnit4ClassRunner.class)  
public class CardTransactionControllerUnitTest {  
  
    @Mock  
  private CardTransactionService cardTransactionService;  
   
  private CardTransactionController cardTransactionController;  
  
  private MockMvc mockMvc; 
  
  @Before  
  public void setUp() throws Exception {  
        MockitoAnnotations.initMocks(this);  
        cardTransactionController = new CardTransactionController(cardTransactionService);  
		mockMvc = MockMvcBuilders.standaloneSetup(cardTransactionController).build();  
		objectMapper = new ObjectMapper();
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


> Written by *Gonzalo Muñoz*.

<!--stackedit_data:
eyJoaXN0b3J5IjpbNzM5MzY1OTgwLDMyMzQyMjE3Nyw1NDA0ND
AxNjcsLTE5NDczNDY5NjgsMzE4NjU3MjE5LC0zOTgzNjE3MzEs
LTk1NTcwOTIyMywtMTg0MTg3NTA2OV19
-->