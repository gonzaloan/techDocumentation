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
public ResponseEntity handlerPost(BeerDTO beerDTO) {  
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
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA3NTY0NzgwXX0=
-->