# UseCase Annotation (Clean Architecture)

## Context

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
import org.springframework.stereotype.Service;

@Service
public class AuthenticationUseCaseService {
    public Boolean authenticate() {
        ...
    }
}
```

But @Service comes from Spring, so we have a dependency from domain layer to external layer. Not good.

## Proposal

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

So we shall turn @UseCase into a @Bean component.
We will do this in the application layer, where spring framework resides.

### Solution 1:

In a @Configuration class implements BeanDefinitionRegistryPostProcessor and override postProcessBeanDefinitionRegistry method.

```java
package application;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanDefinition;
import org.springframework.beans.factory.support.BeanDefinitionRegistry;
import org.springframework.beans.factory.support.BeanDefinitionRegistryPostProcessor;
import org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.type.filter.AnnotationTypeFilter;

@Configuration
public class BeanDefinitionRegistryConfig implements BeanDefinitionRegistryPostProcessor {

    @Override
    public void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry registry) throws BeansException {
        ClassPathScanningCandidateComponentProvider scanner = new ClassPathScanningCandidateComponentProvider(false);
        scanner.addIncludeFilter(new AnnotationTypeFilter(UseCase.class));

        for (BeanDefinition bd : scanner.findCandidateComponents("domain")) {
            registry.registerBeanDefinition(bd.getBeanClassName(), bd);
        }
    }

}
```

### Solution 2:

In a Configuration class add the ComponentScan annotation defining a Filter by annotation.

```java
package application;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.FilterType;

@Configuration
@ComponentScan(includeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = {UseCase.class}))
public class FilterTypeComponentScan {
}
```

### Summary

Those two classes will look for all classes annotated with @UseCase and register them as Bean components.

*By doing this we take advantage of the Spring framework in order to use dependency injection for @UseCase classes making the domain layer totally independent of outer layers.*

> Note :
> We can also create a Repository annotation in the domain layer (do not confound with Spring @Repository) to identify domain Repository classes.
> The same for domain Entity classes.
> 
> Maybe call them as @DomainRepository and @DomainEntity to differentiate from Spring annotations.
