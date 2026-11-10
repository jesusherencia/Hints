# Spring Hints:

## Execute something after application startup:

Two ways to execute something after application startup:

### Directly at SpringBootApplication:

```java
import org.springframework.boot.context.event.ApplicationReadyEvent;
import org.springframework.context.event.EventListener;
...
  @EventListener(ApplicationReadyEvent.class)
  public void doSomethingAfterStartup() {
    System.out.println("Hello world, I have just started up");
  }
...
```

### Implementing ApplicationListener:

```java
import org.springframework.boot.context.event.ApplicationReadyEvent;
import org.springframework.context.ApplicationListener;
import org.springframework.stereotype.Component;

@Component
public class ApplicationStartup implements ApplicationListener<ApplicationReadyEvent> {
    @Override
    public void onApplicationEvent(ApplicationReadyEvent event) {
        System.out.println("Hello world, I have just started up");
    }
}
```

## Using virtual threads in spring mvc:

In order to profit of non-blocking request threads use this config in application configuration file :
```
spring.threads.virtual.enabled=true
```
Note : It's needed at least Java 21 and SpringBoot 3.2

## Using interface-based projections:

While using Spring Data, if we want to obtain a projection of an Entity we can use interfaces.

Let's say I have an entity Person:
```java
class Person {
  @Id UUID id;
  String firstname, lastname;
  Address address;
}
```

And its repository class:
```java
interface PersonRepository extends Repository<Person, UUID> {
  Collection<Person> findByLastname(String lastname);
}
```

If we want to obtain only a projection containing the names, we can create an interface like this:
```java
interface NamesOnly {
  String getFirstname();
  String getLastname();
}
```

And add a method inside the repository class:
```java
interface PersonRepository extends Repository<Person, UUID> {
  Collection<Person> findByLastname(String lastname);
  Collection<NamesOnly> findByLastname(String lastname); // <--
}
```

For more information about this, refer to this [link](https://docs.spring.io/spring-data/jpa/reference/repositories/projections.html)
