# Spring Rest Docs


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
Para empezar, y sabiendo qué controller queremos documentar, debemos crear una clase en la carpeta src/test con las siguientes anotaciones:

```java
@RunWith(SpringRunner.class)  
@WebMvcTest(CardTransactionController.class)  
@AutoConfigureRestDocs(value = "target/generated-snippets")  
public class CardTransactionControllerEndpoint {
	@Autowired  
	private MockMvc mvc;  
  
	@MockBean  
	private CardTransactionController cardTransactionController;
}
```
Primero definimos qué clase correrá nuestras pruebas (*@RunWith*), Definimos qué controller testeamos (*@WebMvcTest*) y configuramos los RestDocs (*@AutoConfigureRestDocs*). Esto hará que los snippets generados con las pruebas se almacenen en la carpeta **target/generated-snippets** dentro de una carpeta que definiremos en el método probado.


Una prueba de un controlador común y corriente tiene el siguiente formato:

```java
@Test  
public void findBeneficiaryCardBalance_when_is_ok() throws Throwable {  
  
    BDDMockito.given(cardTransactionController.findBeneficiaryCardBalance(6060730018862165L,"165775198","965569308", 16L )).willReturn(new ResponseEntity <>(10000L, HttpStatus.OK));  
  
  mockMvc.perform(RestDocumentationRequestBuilders.get("/transactions/beneficiary/cards/balance/{pan}?beneficiaryFiscalId=165775198&companyFiscalId=965569308&serviceId=16", 6060730018862165L)  
            .contentType(MediaType.APPLICATION_JSON))  
            .andExpect(MockMvcResultMatchers.status().isOk())  
            .andExpect(jsonPath("$").isNotEmpty())  
            .andExpect(content().json("10000"))  
            .andDo(MockMvcResultHandlers.print())  
			  //Spring Rest Docs desde aquí
            .andDo(document("findBeneficiaryCardBalance",  
				 pathParameters(
					 parameterWithName("pan").description("Corresponse al número de identificación de la tarjeta.")  
                    ),
                 requestParameters(
	                 parameterWithName("beneficiaryFiscalId").description("Corresponde al RUT del beneficiario"),  
					 parameterWithName("companyFiscalId").description("Corresponde al RUT de la compañía"),  
					 parameterWithName("serviceId").description("Corresponde al identificador del servicio, el tipo de servicio.")  
                    ))  
            );  
}
```
Si analizamos esta prueba, la sintaxis de **BDDMockito.given**, estamos forzando un comportamiento en el controller. Indicamos que si se llama al método *findBeneficiaryCardBalance* en el controlador con los parámetros ahí indicados, este retornará una ResponseEntity con una respuesta y status OK. 

Por otro lado, **mvc.perform** hará una petición get al endpoint, con los parámetros ahí indicados. Además, los *andExpect* indican que estamos esperando que dicha llamara entregue respuestas como el status OK, y que la respuesta que entrega el endpoint no esté vacía, y que el contenido sea *10000*.

#### Spring Rest Docs

Debajo del comentario que dice **Spring Rest Docs**, está la información necesaria para crear la documentación. En la primera línea luego de este marcado, se puede apreciar:

- **.andDo(document("findBeneficiaryCardBalance",**: Esta línea es la que creará el snippet con el nombre que declaremos acá. Este snippet luego va a contener toda la información que le indiquemos más abajo. 
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

Una vez se tiene los adocs listos, sólo se debe hacer un **mvn clean package**, esto creará un archivo html con nuestra documentación, dentro de la carpeta **src/docs/asciidocs** que definimos en el pom.


## Validar Constraints 



> Written by [Gonzalo Muñoz](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzNTU5MTY5ODIsLTIxMDM4NTc3MjVdfQ
==
-->