# Using Aspect (relieve passing argument through layers)

Let's say that backend needs to access an external api to get a specific complex data structure (like complex json object).
And this json object contains sections that can be avoided in order to reduce response size. So a parameter is added in the api call in order to avoid adding sections in the json object.

- Rest Api Call:
> /data/<id>?withSubSection=<boolean>

Let's assume that layered architecture is used in the backend Controller -> Service -> Repository.
We use Repository in order to call the rest api.

- A common way to access data is doing like this :
> Controller.getData(idData) -> Service.getData(idData) -> Repository.getData(idData)
Or more layers are added
> Controller.getData(idData) -> Factory.getData(idData) -> Service.getData(idData) -> Mapper.getData(idData) -> Repository.getData(idData)
> Controller.getLightData(idData) -> Factory.getData(idData) -> Service.getData(idData) -> Mapper.getData(idData) -> Repository.getData(idData)

If we want to use the boolean in order to condition the response content having sub-sections, we'd need to add the boolean as a parameter through all layers.
> Controller.getLightData(idData) -> Factory.getData(idData, boolean) -> Service.getData(idData, boolean) -> Mapper.getData(idData, boolean) -> Repository.getData(idData, boolean)

In order to avoid passing the booelan parameter we can use Aspects.
Let's assume we have a clear separation between Domain and Infrastructure.

- Create an annotation in the domain :

```java
package domain;

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface WithSubsection {
}
```

- In Factory we will copy getData method, rename the new method and add anotation.

```java
package domain;

@DomainService
public class Factory {
  ...
  @WithSubsection
  public Data getData(idData) {
    ...
  }
  ...
}
```

- Create a bean that will be request scope one :

```java
package infrastructure;

@Aspect
@Component
@RequestScope
public class RequestContext {
  public static final String withSubsectionKey = "withSubsection";
  private final ConcurrentHashMap<String, String> context = new ConcurrentHashMap();

  public String getByKey(String key) {
    return context.getOrDefault(key, true); // true is the default value
  }

  public void setByKey(String key, String value) {
    return context.put(key, value); // true is the default value
  }
}
```

- Create an Aspect in the infrastructure :

```java
package infrastructure;

@Aspect
@Component
public class WithSubsectionAspect {
  private final RequestContex requestContext;

  @Before("@annotation(domain.WithSubsection)")
  void setWithSubsection() {
    requestContext.setByKey(RequestContext.withSubsectionKey, false");
  }
}
```

- In repository adapter :

```java
package infrastructure;

@Repository
public class DataRepository {
  private final RequestContext requestContext;
  private final RestTemplate restTemplate

  @Before("@annotation(domain.WithSubsection)")
  public Data getData(idData) {
    var withSubsection = requestContext.getByKey(RequestContext.withSubsectionKey)
    // Use restTemplate to call external api    
    ...
  }
}
```

Note : we are doing this in a request scope, as we needed to condition by request. We can change scope if necessary.
