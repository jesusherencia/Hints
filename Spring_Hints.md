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
