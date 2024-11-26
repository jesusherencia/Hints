# UseCase Annotation (Clean Architecture)

When developing a Java Application the common chosen framework is SpringBoot.
A common structure of classes is used in this case : 
    Controller -> Service -> Repository

We annotate those classes with the corresponding Spring annotations :
- RestController
- Service
- Repository

What happens if we try to apply the Clean Architecture to our project ?
Well, in that case we isolate our domain layer with should be mainly composed of :
- Use Case
- Entity
- Repository
And no depedencies to external layers should exist.

We can say that Service classes are our Use Case classes in our domain layer.
So we place Service classes in a domain package. So we will have the following Class:

```java
package domain; 

@Service
public class AuthenticationUseCaseService {
    public Boolean authenticate() {
        ...
    }
}
```

But @Service comes from Spring, so we have a dependency from domain layer to external layer.
To overcome this we replace @Service by the UseCase annotation that we create in the domain package.

```java
package domain;
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface UseCase {
}
```

Now we have the following class:

```java
package domain; 

@UseCase
public class AuthenticationUseCaseService {
    public Boolean authenticate() {
        ...
    }
}
```

Great! No dependencecies from domain to outer layers.
But AuthenticationUseCaseService won't be treated as a Bean in the Spring framework and no dependency injection could be done over this class.

So we shall turn @UseCase a @Bean component.
We will do this in the application layer.

```java
@Configuration
public class ApplicationConfiguration {

    @Autowired
    private GenericApplicationContext context;

    @PostConstruct
    public void registerAnnotatedClassesAsBeans() {
        ClassPathScanningCandidateComponentProvider scanner =
                new ClassPathScanningCandidateComponentProvider(false);

        scanner.addIncludeFilter(new AnnotationTypeFilter(UseCase.class));

        for (BeanDefinition bd : scanner.findCandidateComponents("domain")) {
            System.out.println(bd.getBeanClassName());
            context.registerBeanDefinition(bd.getBeanClassName(), bd);
        }
    }

}
```

This class will look for all classes annotated with @UseCase and register them as Bean component.

By doing this we take advantage of the Spring framework in order to use dependency injection for @UseCase classes making the domain layer totally independent of outer layers.