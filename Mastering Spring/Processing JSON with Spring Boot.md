## Processing JSON with Spring Boot


### Javascript Object Notation

- Json es un formato de intercambio de datos ligero.
- Basado en el lenguaje de programación Javascript ECMA-262.
- Hoy en día es independiente de Javascript
- El estándar de hoy en día es ECMA-404
- JSON puede ser leído y creado por la mayoría de los lenguajes de programación
- JSON es un formato ideal de intercambio de información por su amplia adopción.

### Procesamiento de JSON

- **Serialization**: Es el proceso de convertir objetos Java en un objeto JSON.
- **De-Serialization**: Proceso de convertir un JSON en un objeto Java. 
-- Típicamente es un String, algunas veces un buffer, o un archivo.
- En una acción HTTP GET, Spring leerá la información en POJOS, serializará el POJO en un String JSON y entregará el payload JSON al cliente. 
- En una acción HTTP POST, el cliente usará HTTP para hacer el post, Spring leerá el cuerpo de la petición y deserializará el payload JSON en un POJO de Java.
- También puede ser usado para mensajear. 

### Librerías JSON en Spring Boot

Spring soporta 3 librerías de mapeos JSON:

- **Jackson**: El default de Spring Boot
- **GSON**
- **JSON-B**.

Spring configurará automáticamente cualquiera de estas 3 librerías.

### Anotaciones comunes de Jackson

- **@JsonProperty**: Permite setear el nombre de la propiedad.
- **@JsonFormat**: Da control sobre cómo es serializada la propiedad.
- **@JsonUnwrapped**: Permite 'suavizar' una propiedad. Por ejemplo si tienes un objeto embedido permite suavizarlo en un bajo nivel en vez de múltiples niveles.
- **@JsonView**: Permite configurar vistas virtuales de objetos. Esto permite por ejemplo transformar un objeto de x cantidad de elementos, y ver qué propiedades se muestran o no.
- **@JsonManagedReference, @JsonBackReference**: Para mapear objetos embebidos.
- **@JsonIdentityInfo**: Permite especificar la propiedad para determinar la identidad de un objeto. 
- **@JsonFilter**: Esada para especificar una propiedad de filtro programática. 

##### Anotaciones Serialización

- **@JsonAnyGetter**:  Toma la propiedad de un Map y las keys se convertirán en nombres de property y los values se convertirán en realmente values. 
- **@JsonGetter**: Identifica a un método como Getter.
- **@JsonPropertyOrder**: Te permite establecer el order de propiedades en el output serializado
- **@JsonRawValue**: Va a serializar el valor String de la propiedad como está. 
- **@JsonValue**: Usado para marcar un método para la salida serializada. 
- **@JsonRootName**: Crea un root element para el objeto
- **@JsonSerialize**: Permite especificar un serializador custom. 

##### Anotaciones Deserialización

- **@JsonCreator**: Permite identificar un objeto constructor para usar. 
- **@JacksonInject**: Permite inyectar valores dentro de propiedades usando deserialización. 
- **@JsonAnySetter**: Convierte un objeto JSON en un objeto Java Map, donde los nombres de las propiedades son las keys del map.
- **@JsonSetter**: Identifica un método para ser usado como setter para el JSON identificado.
- **@JsonDeserialize**: Permite especificar un deserializador custom.

#### Otras anotaciones Jackson

- **@JsonIgnoreProperties**: Anotación de nivel de clase que le dice a Jackson que ignore una o más propiedades.
- **@JsonIgnore**: Anotación de nivel de campo que permite ignorar una propiedad.
- **@JsonIgnoreType**: Anotación de nivel de clase que permite ignorar una clase. Típicamente es usado para ignorar una clase, donde la clase es la propiedad en otra clase.
- **@JsonInclude**: Anotación de nivel de clase usada para controlar cómo los valores null son presentados en la serialización.
- **@JsonAutoDetect**: Jackson usará reflección en la serialización. Mirará la clase y determinará cómo mostrar esas propiedades/métodos. 

### Ejemplos de Uso

El Json Generado ya no tendrá el campo 'id', sino que 'beerId':
```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class BeerDTO{
	@JsonProperty("beerId") //El Json generado va a cambiar a BeerId
	@Null
	private UUID id;

	@NotBlank
	private String beerName;

	@NotBlank
	private String beerStyle;

	@Positive
	private Long upc;

	@JsonFormat(shape = JsonFormat.Shape.STRING)
	private BigDecimal price;
	@JsonFormat(pattern = "yyyy-MM-dd", shape = JsonFormat.Shape:STRING)
	private OffsetDateTime createdDate;
	private OffsetDateTime lastUpdatedDate;

	
}
```
- price: En el caso de price, lo que se hace es formatear como un String el campo precio. 
- createdDate: En el caso de la fecha, se está cambiando la forma de mostrar la fecha según el pattern, y como un String.


#### Custom Serializer

También podemos crear serializadores personalizados. Para ello creamos una clase así:

Supongamos que queremos serializar un LocalDate: 


```java
public class LocalDateSerializer extends JsonSerializer<LocalDate> {
	
	@Override
	public void serialize(LocalDate value, JsonGenerator gen, SerializerProvider serializers) throws IOEXception{
		gen.writeObject(value.format(DateTimeFormatter.BASIC_ISO_DATE));
		
	}
}

```

Esto lo que hace, es tomar el **value**, y luego el JsonGenerator y escribir ese objeto. 
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk1OTY3OTgyNSwxNTUwNzQ4NDA2LC04NT
g2MDQxNTEsLTE0NTYyOTY0MzldfQ==
-->