## Debug Logging

Apache trae un logging por default muy bueno para ver toda la información, sólo hay que agregar al archivo properties:

```
logging.level.org.apache.http=debug
```

## Java Bean Validation

Nos permite hacer validaciones, por ejemplo en un DTO:

Hay muchas
- @Null
- @NotBlank
- @NotNull
- @Max
- @Currency
- @Email
- @Length
- @SafeHtml
- @UniqueElements
- @Url
- @Digits
- @AssertTrue/@AssertFalse
- @DecimalMax
- @Negative
- @Positive
- @Past
- @PastOrPresent
- @Pattern
- @Size

Entre muchas otras

```java
public class BeerDTO {  
  
  @Null //NO permitir al cliente setear el ID  
  private UUID id;  
  @NotBlank  
  private String beerName;  
  @NotBlank  
  private String beerStyle;  
  @Positive  
  private Long upc;  
  
}
```

Una vez tenemos esto, debemos irnos al Controller, donde tomamos este objeto y añadimos la etiqueta `@Valid`



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NDA2OTU3NTgsMTg3OTUzNjIwOSwxOD
gxMjYyMTg4LC01NzEyNDc0OTMsNzgzNTg3MjEzXX0=
-->