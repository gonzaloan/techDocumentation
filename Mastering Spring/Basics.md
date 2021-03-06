## Crud

### Get Request

```java
@GetMapping("/{customerId}")  
@ResponseBody  
@ResponseStatus(HttpStatus.OK)  
public CustomerDTO getCustomer(@PathVariable UUID customerId) {  
  return customerService.getCustomerById(customerId);  
}
```

### Post Request

```java
@PostMapping  
public ResponseEntity handlerPost(@RequestBody BeerDTO beerDTO) {  
  BeerDTO savedDto = beerService.saveNewBeer(beerDTO);  
  HttpHeaders headers = new HttpHeaders();  
  //Estándar de REST TODO: add hostname to URL  
  headers.add("Location", "/api/v1/beer/" + savedDto.getId().toString());  
 return new ResponseEntity(headers, HttpStatus.CREATED);  
}
```

### Put

```java

@PutMapping("/{beerId}")  
public ResponseEntity handleUpdate(@PathVariable UUID beerId, @RequestBody BeerDTO beerDTO) {  
  beerService.updateBeer(beerId, beerDTO);  
  
 return new ResponseEntity(HttpStatus.NO_CONTENT);  
}
```

### DELETE

```java
@DeleteMapping("/{beerId}")  
@ResponseStatus(HttpStatus.NO_CONTENT)  
public void deleteBeer(@PathVariable("beerId") UUID beerId) {  
  beerService.deleteById(beerId);  
}
```


### API Versioning

- Versionar es considerado una buena práctica
- Ejemplo ```/api/v1/beer```
- Permite evolucionar la API sin romper a los consumidores actuales.

### Instalar Junit 5

Para hacer esto, se debe primero excluir JUNIT 4 de la librería de spring boot starter test.

```xml
<dependency>  
 <groupId>org.springframework.boot</groupId>  
 <artifactId>spring-boot-starter-test</artifactId>  
 <scope>test</scope>  
 <exclusions>  
 <exclusion>  
 <groupId>junit</groupId>  
 <artifactId>junit</artifactId>  
 </exclusion>  
 </exclusions>  
</dependency>
```

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbODM1NTcxMDQwLC01Njg1OTM1OTQsMTMzMz
UxNDk1NSwxMDc1NjQ3ODBdfQ==
-->