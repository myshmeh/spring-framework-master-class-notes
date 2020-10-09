# References
- https://www.udemy.com/course/spring-tutorial-for-beginners/
# 2020/10/08
## What is the core of Spring Framework?
- a dependency injection framwork
## What is dependency?
- whatever a thing is dependent on
```java
public class ComplexBusinessService { // this class is dependent on sortAlgorithm
    SortAlgorithm sortAlgorithm = new BubbleSortAlgorithm(); // this is dependency of ComplexBusinessService
}
public class BubbleSortAlgorithm implements SortAlgorithm {}
```
- potential problem here is that sortAlgorithm is directly instantiated inside the ComplexBusinessService class thus tightly coupled with the class.
- tight coupling is considered a bad code. loose coulpling is recommended - pass the sort instance to the ComplexBusinessService contructor
## What is dependency injection?
```java
public class ComplexBusinessService { // this class is dependent on sortAlgorithm
    SortAlgorithm sortAlgorithm; // = new BubbleSortAlgorithm(); // this is dependency of ComplexBusinessService
    public ComplexBusinessService (SortAlgorithm sortAlgorithm) {
        this.sortAlgorithm = sortAlgorithm; 
    }
}
public class BubbleSortAlgorithm implements SortAlgorithm {}

SortAlgorith sort = new BubblesortAlgorithm();
ComplexBusinessService service = new ComplexBusinessService(sort); // this is dependency injection
```
- Are we gonna do this every sinlge time? - Spring Framework!
```java
@Component
public class ComplexBusinessService { 
    @Autowired
    SortAlgorithm sortAlgorithm;
    String[] employees = ["jack", "amy", "naomi"];
    public ComplexBusinessService (SortAlgorithm sortAlgorithm) {
        this.sortAlgorithm = sortAlgorithm;
    }

    public getStaffs() {
        return this.sortAlgorithm.sort(employees);
    }
}
@Component
public class BubbleSortAlgorithm implements SortAlgorithm {}

@SpringBootApplication
public class SpringIn5StepsApplication {
	// How to tell Spring:
	//   What are the beans? - @Component
	//   What are the dependencies of a bean? - @Autowired
	//   Where to search for beans? - @ComponentScan (@SpringBootApplication automatically scans the package and the sub-package)
	public static void main(String[] args) {
		ApplicationContext applicationContext = SpringApplication.run(SpringIn5StepsApplication.class, args);
		ComplexBusinessService complexBusinessService = applicationContext.getBean(ComplexBusinessService.class);
		int result = complexBusinessService.getStaffs();
		System.out.println(result);
	}

}
```
## Frequent Terminologies
- Component
  - Spring starts managing the annotated class
- Autowiring
  - Identify what's dependency of the Component
- Beans
  - Objects managed by Spring Framework
- Inversion of Control
  - giving the control of instantiation to another
- IOC Container
  - the thing given the control of instantiation
- Application Context
  - where all beans are created and managed by Spring Framwork
  - one of IOC Containers

# 2020/10/09
## Contructor injection vs Setter injection
- for mandatory dependencies, constructor injection; and
- for optional dependencies, setter injection are recommended
- but nowadays, annotate @Autowired and let the Spring do the rest is common
- Spring consists of multiple modules, including Core, Beans and Test
- Spring prijects are small modules to tackle specific problems, including Spring Boot, Spring Security and Spring Batch
## Autowired in depth
- use @Primary to priortize the Component to inject
- or you could use @Qualifier if you often switch between multiple components
## Bean scope
- a bean managed by Spring context is singleton by default
- other scope types include:
  - prototype: instantiated per getBean call
  - request: instantiated per HTTP request
  - session: instantiated per HTTP session
- use @Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE) to explicitly set a bean scope
- use @Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE, proxyMode = ScopedProxyMode.TARGET_CLASS) for a component wired from a singleton scope component to make sure the dependency component is instantiated per getBean call.
