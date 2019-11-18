# RestTemplate

## Get

```java
@Component  
@ConfigurationProperties(value = "sfg.brewery", ignoreUnknownFields = false)  
public class BreweryClient {  
  
  private final String BEER_PATH_V1 = "/api/v1/beer/";  
 private String apihost;  
 private final RestTemplate restTemplate;  
  
 public BreweryClient(RestTemplateBuilder restTemplateBuilder) {  
  this.restTemplate = restTemplateBuilder.build();  
  }  
  
  public BeerDto getBeerById(UUID uuid) {  
  return restTemplate.getForObject(apihost + BEER_PATH_V1 + uuid.toString(), BeerDto.class);  
  }  
  
  public void setApihost(String apihost) {  
  this.apihost = apihost;  
  }
```

## Post 

```java
public URI saveNewBeer(BeerDto beerDto){  
  return restTemplate.postForLocation(apihost+BEER_PATH_V1, beerDto);  
}
```
## Put

```java
public void updateBeer(UUID uuid, BeerDto beerDto){  
  restTemplate.put(apihost+BEER_PATH_V1 + "/", uuid.toString(), beerDto);  
}
```
## Delete

```java
  
public void deleteBeer(UUID uuid) {  
  restTemplate.delete(apihost + BEER_PATH_V1 + uuid.toString());  
}
```
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4OTgyNTM1NjNdfQ==
-->