

Spring

-  spring的依赖注入是怎么实现的?
-  在一个方法中改变了注入对象的属性, 另一个方法调用该属性是否改变? 
-  spring中bean的生命周期，作用域



# 一，20道面试题

> 基本概念，
>
> ioc，
>
> aop：
>
> bean： 生命周期，作用域，配置
>
> 注解，Autowired，Qualifier
>
> 事务，
>
> 事件，
>
> 异步，
>
> 跨模块管理；
>
> 关键接口：

- [ ] **Spring框架是什么？它的核心模块有哪些？**

- [ ] **Spring的IOC（控制反转）是什么？请解释它的工作原理。**

- [ ] **什么是Spring的依赖注入（DI）？有哪几种方式实现DI？**

- [ ] **什么是Spring的AOP（面向切面编程）？**

- [ ] **Spring AOP的代理模式有哪些？它们之间有何异同？**

- **Spring AOP 如何选择 JDK 动态代理和 CGLIB？**

  ```
  CGLIB 代理 基于子类，的底层确实是通过 字节码生成代理 来实现的。
   JDK 动态代理： 接口，通过反射创建实列对象
  ```

  

- **Spring AOP的内部实现原理是什么？**

- **Spring中的代理模式是如何实现的？JDK动态代理与CGLIB代理有什么区别？**

- [ ] **Spring Bean的生命周期是什么？从实例化到销毁的过程如何？**

- [ ] **Spring Bean的作用域有哪几种？解释它们的区别。**

- [ ] 如何在Spring中配置Bean？可以通过哪些方式进行配置？

- [ ] **Spring中的@Autowired注解和@Qualifier注解有什么区别？**

- [ ] **什么是Spring的容器？它有哪些类型？**

- [ ] **解释一下Spring事务的概念和常见的事务管理方法。**

### 进阶难度

1. **Spring如何实现事务的管理？如何实现声明式事务？**
2. **什么是Spring的Event机制？如何发布和监听事件？**
3. **Spring Boot和传统Spring的区别是什么？**
4. **Spring Security的核心概念有哪些？如何实现基于角色的访问控制？**
5. **Spring的@Value注解和@ConfigurationProperties注解有什么区别？**
6. **Spring的条件注解（@Conditional）是如何工作的？如何根据条件加载Bean？**
7. **什么是Spring的Profiles？如何使用Profiles进行环境配置？**
8. **Spring的JdbcTemplate和MyBatis的区别是什么？**
9. **Spring Data JPA是什么？它如何简化数据库操作？**

### 深度难度

1. **Spring的循环依赖问题如何解决？如何使用三级缓存机制？**
2. **解释 BeanPostProcessor 的作用。**
3. **Spring的BeanFactory与ApplicationContext有什么区别？**
4. **Spring Boot启动过程的底层实现是怎样的？**
5. **如何在Spring中实现自定义的注解？请举例说明。**
6. **Spring如何支持异步编程？如何使用@Async注解实现异步任务？**
7. **Spring中的代理模式是如何实现的？JDK动态代理与CGLIB代理有什么区别？**
8. **Spring的缓存抽象是如何实现的？如何使用Spring Cache进行缓存管理？**
9. **Spring WebFlux与传统Spring MVC的区别？**
   **答**：WebFlux基于Reactor库实现响应式编程，支持非阻塞IO（如Netty），适合高并发场景；MVC基于Servlet API，同步阻塞模型。
10. **Spring源码中使用了哪些设计模式？举例说明。**
    **答**：工厂模式（BeanFactory）、代理模式（AOP）、模板方法（JdbcTemplate）、观察者模式（ApplicationEvent）、单例模式（Bean作用域）。





# 二，答案

## 2.1,Spring框架是什么？它的核心模块有哪些？

1. **Spring Core Container（核心容器）**
   - **模块**：`spring-core`、`spring-beans`、`spring-context`、`spring-expression`（SpEL）。
   - **作用**：
     - **依赖注入（DI）**：通过`BeanFactory`和`ApplicationContext`管理对象（Bean）的创建与依赖关系，实现松耦合。
     - **控制反转（IoC）**：将对象的创建与生命周期交给容器管理，开发者专注于业务逻辑。
     - **SpEL（Spring表达式语言）**：支持在运行时动态操作对象属性。
2. **Spring AOP（面向切面编程）**
   - **模块**：`spring-aop`、`spring-aspects`。
   - **作用**：
     - 通过代理机制实现横切关注点（如日志、事务、安全）的模块化，避免代码重复。
     - 与AspectJ集成，支持声明式事务管理（如`@Transactional`注解）。
3. **Spring Data Access/Integration（数据访问与集成）**
   - **模块**：`spring-jdbc`、`spring-orm`、`spring-tx`。
   - **作用**：
     - **JDBC**：简化数据库操作，减少样板代码（如`JdbcTemplate`）。
     - **ORM**：集成Hibernate、MyBatis等框架，统一数据访问接口。
     - **事务管理**：通过声明式或编程式事务保证数据一致性。
4. **Spring Web（Web模块）**
   - **模块**：`spring-web`、`spring-webmvc`、`spring-websocket`。
   - **作用**：
     - **MVC架构**：提供`DispatcherServlet`处理HTTP请求，支持RESTful开发。
     - **WebSocket**：实现双向实时通信。
     - **Spring WebFlux**：支持响应式编程的Web框架，适用于高并发场景。
     - 整合Servlet API，简化Web层开发。
5. **Spring Test（测试模块）**
   - **模块**：`spring-test`。
   - **作用**：
     - 支持单元测试与集成测试，提供`Mock`对象和注解（如`@SpringBootTest`）。
     - 集成JUnit、TestNG等测试框架
6. **Security**：
   - 提供全面的安全框架，用于认证、授权、攻击防护等功能。



## 2.2,Spring的IOC（控制反转）是什么？请解释它的工作原理/ ioc 过程。

> 概念，具体实现，与传统的对比，好处

### 一，概念

 **IOC（控制反转，Inversion of Control）** 是一种设计思想，其核心是将对象的创建、依赖管理和生命周期**控制权**转移到第三方。从而降低代码的耦合度，提高灵活性和可维护性。



### 二，**IOC 的实现：依赖注入（DI）**

- 依赖注入（Dependency Injection）是 IOC 的具体实现方式，Spring 容器通过以下方式注入依赖：
  - **构造函数注入**：通过构造器参数传递依赖。
  - **Setter 方法注入**：通过 setter 方法赋值。
  - **字段注入**：通过 `@Autowired` 注解直接注入字段（需谨慎使用）。

### 三，IOC的好处：

- **松耦合**：通过IOC，类之间的耦合性大大降低，类不再依赖于具体的实现，而是依赖于接口或抽象类。
- **易于扩展和维护**：对象的依赖关系由容器管理，因此不需要修改对象代码即可更改依赖的实现，增加了系统的灵活性。
- **提高可测试性**：通过依赖注入，开发者可以轻松地替换依赖，使用模拟对象进行单元测试。
- **集中化配置**：Spring容器通常通过XML或注解配置对象的创建和依赖关系，便于集中管理和修改。

### 四，**Spring 容器的作用**

1. **管理 Bean**：
   通过配置（XML、注解或 Java Config）定义 Bean（即对象），容器负责实例化、初始化和销毁 Bean。
2. **依赖解析**：
   自动处理 Bean 之间的依赖关系（如 `@Autowired`）。
3. **生命周期管理**：
   支持 `@PostConstruct`、`@PreDestroy` 等回调方法。

### **关键注解**

- **`@Component`**：标记类为 Spring 管理的 Bean。
- **`@Autowired`**：自动注入依赖（可省略显式配置）。
- **`@Configuration`** + **`@Bean`**：通过 Java 代码定义 Bean。（cglb 默认单列）
- **`@ComponentScan`**：启用组件扫描，自动发现 Bean。



## 2.3,什么是Spring的依赖注入（DI）？有哪几种方式实现DI？

> 已经回答

## 2.4,什么是Spring的AOP（面向切面编程）？

### 1. 是什么

Spring的AOP（面向切面编程，Aspect-Oriented Programming）是一种编程范式，它是面向对象编程（OOP）的一种补充。AOP的主要目标是将程序中的横切关注点（cross-cutting concerns）与业务逻辑分离开来。例如日志记录、事务管理、安全检查等。在不改变义务代码情况下扩展新的功能。

### 2. 核心的概念

AOP的主要概念包括：

- **切面（Aspect）**：是横切关注点的模块化，包含了一个或多个通知（Advice）和切入点（Pointcut）。切面可以看作是一个关注点的抽象，如日志、事务等。
- **连接点（Joinpoint）**：程序中可以插入切面的点，通常是方法的执行。Spring AOP中的连接点一般是方法执行时。
- **通知（Advice）**：在切面中定义的操作，指定何时执行。通知定义了切面逻辑的具体实现，可以在目标方法执行之前、之后，甚至在发生异常时执行。
  - **前置通知（Before）**：目标方法执行之前执行。
  - **后置通知（After）**：目标方法执行之后执行，无论方法是否抛出异常。
  - **返回通知（AfterReturning）**：目标方法执行成功后执行。
  - **异常通知（AfterThrowing）**：目标方法抛出异常后执行。
  - **环绕通知（Around）**：包裹目标方法的执行，可以控制目标方法是否执行，并且在执行前后添加自定义操作，是最强大的通知类型。
- **切入点（Pointcut）**：切入点定义了通知应用到哪些方法，通常通过表达式来指定哪些方法是目标方法。切入点表达式可以基于方法名、参数类型、修饰符等条件来选择。
- **代理（Proxy）**：AOP通过代理模式实现。在Spring中，AOP通常通过生成目标对象的代理对象来实现切面功能。代理对象是目标对象的一个代理，切面的通知会在代理方法执行时被织入。

```java
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

@Aspect
public class LoggingAspect {
    
    @Before("execution(* com.example.UserService.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        String methodName = joinPoint.getSignature().getName();
        System.out.println("Before method: " + methodName);
    }
    
    @After("execution(* com.example.UserService.*(..))")
    public void logAfter(JoinPoint joinPoint) {
        String methodName = joinPoint.getSignature().getName();
        System.out.println("After method: " + methodName);
    }
}
```

> 1. LoggingAspect 切面：功能
> 2. logBefore，logAfter 通知
> 3. JoinPoint ：连接点 ，包含原对象中的方法，参数信息等
> 4. 切点：具体要执行的方法 ，execution(* com.example.UserService.*(..)) 
> 5. 代理对象：被代理的原始对象

### 3. Spring AOP的实现

Spring AOP是基于代理模式实现的。它通过动态代理（JDK动态代理或CGLIB字节码生成库）来为目标对象创建代理，从而在目标方法执行之前或之后插入额外的功能。Spring AOP主要支持两种类型的代理：

- **JDK动态代理**：适用于接口类型的目标对象，代理类会实现目标对象的接口，并通过接口调用目标方法。（@Conponent。有接口的） public 方法
- **CGLIB代理**：适用于没有实现接口的目标对象，代理类是通过字节码技术在运行时创建的目标对象的子类。 （@Configuration+ @Bean 没有实现接口）不能是final



### 4. AOP的应用场景

AOP主要用于处理横切关注点（cross-cutting concerns），常见的应用场景包括：

- **日志记录**：在方法执行之前或之后记录日志。
- **性能监控**：记录方法执行的时间，分析性能瓶颈。
- **事务管理**：在方法执行时自动管理事务，如开启、提交或回滚事务。
- **安全控制**：在方法执行之前进行权限检查。
- **缓存管理**：在方法执行前检查缓存，避免重复计算。



## 2.5,Spring AOP的代理模式有哪些？它们之间有何异同？

## 2.6,Spring AOP 如何选择 JDK 动态代理和 CGLIB？



Spring框架会根据目标对象是否实现接口来自动选择代理方式：

- **如果目标类实现了接口**，Spring会使用**JDK动态代理**。

  > 实列：@Componet 组件，实现接口
  >
  > 实现：JDK动态代理是通过Java的**反射机制**实现的
  >
  > 体现：*Proxy的对象

- **如果目标类没有实现接口**，Spring会使用**CGLIB代理**。

  > 实列：@configration + @bean ,么有接口的 bean
  >
  > 实现：CGLIB通过字节码生成技术，动态地创建目标类的子类，并覆盖其中的方法
  >
  > 体现：*cglb 的对象



### 手动选择代理方式 @EnableAspectJAutoProxy 

```java
@EnableAspectJAutoProxy(proxyTargetClass = true) // 强制使用CGLIB代理
@EnableAspectJAutoProxy(proxyTargetClass = false) // 使用JDK动态代理
```

注意事项：

- 使用 CGLIB 代理时，目标类不能是 `final` 的，因为 CGLIB 通过继承生成子类。
- 被代理的方法也不能是 `final` 或 `static` 的，因为这些方法无法被覆盖。包括非 `public` 方法



## 2.7,Spring Bean的生命周期是什么？从实例化到销毁的过程如何？

> 主要有：实列化，属性赋值，初始化，使用，销毁，可以再细分

Spring Bean 的生命周期是由 Spring 容器管理的，其完整的过程从实例化到销毁大致可以分为以下几个阶段：

1. **实例化（Instantiation）：**
   容器通过反射创建 Bean 实例，调用构造方法（默认构造器或通过依赖注入指定的构造器）。
2. **属性填充（Populate Properties）：**
   实例化后，Spring 根据配置进行依赖注入，将所有必要的属性赋值给 Bean。
3. **感知阶段（Aware Interfaces）：**
   如果 Bean 实现了特定的 Aware 接口（如 `BeanNameAware`、`BeanFactoryAware`、`ApplicationContextAware` 等），Spring 会注入相应的上下文信息或名称。
4. **BeanPostProcessor【前置处理】（postProcessBeforeInitialization）：**
   在初始化前，所有注册的 `BeanPostProcessor` 会调用其 `postProcessBeforeInitialization` 方法，对 Bean 进行进一步处理。
5. **初始化（Initialization）：**
   - 如果 Bean 实现了 `InitializingBean` 接口，则调用其 `afterPropertiesSet()` 方法。
   - 同时，如果在配置中指定了自定义初始化方法（`init-method`），该方法也会被调用。
     这一步是进行一些初始化逻辑的执行，确保 Bean 已经准备好被使用。
6. **BeanPostProcessor【后置处理】（postProcessAfterInitialization）：**
   在初始化完成后，`BeanPostProcessor` 的 `postProcessAfterInitialization` 方法被调用，允许对 Bean 进行二次加工或包装。
7. **Bean 可用阶段（Ready for Use）：**
   此时，Bean 已经完全初始化，可以在应用中被正常使用。
8. **销毁（Destruction）：**
   当容器关闭时，Spring 会对单例 Bean 进行销毁处理。
   - 如果 Bean 实现了 `DisposableBean` 接口，则调用其 `destroy()` 方法。
   - 同时，如果在配置中指定了自定义销毁方法（`destroy-method`），该方法也会被调用。
     这一步用于释放资源和进行清理工作。



## 2.8,Spring Bean的作用域有哪几种？解释它们的区别。

> 单列，property，request，sesstion, global,websocke

在Spring框架中，**Bean的作用域（Scope）**决定了Bean实例在Spring容器中的生命周期和可见性。不同的作用域适用于不同的应用场景，合理选择作用域可以提高应用的性能和可维护性。

### Spring Bean的主要作用域

Spring提供了以下几种主要的Bean作用域：

1. **单例（Singleton）**
2. **原型（Prototype）**
3. **请求（Request）**
4. **会话（Session）**
5. **应用上下文（Application）**
6. **WebSocket（WebSocket）**

下面详细介绍每种作用域及其区别，并解释其实现机制。

------

#### 1. 单例（Singleton）

- **定义**：在整个Spring容器中，只存在一个Bean实例。无论多少次请求，都会返回同一个实例。

- **适用场景**：适用于无状态的Bean，如服务层（Service）、数据访问层（DAO）等。

- **实现机制**：

  - 默认的作用域。
  - Spring容器在**启动时创建单例Bean**，或在第一次请求时懒加载（取决于配置）。
  - 使用`ConcurrentHashMap`等线程安全的集合来存储和管理单例Bean。

- **示例**：

  ```xml
  <bean id="mySingletonBean" class="com.example.MySingletonBean" scope="singleton"/>
  ```

#### 2. 原型（Prototype）

- **定义**：每次请求都会创建一个新的Bean实例。

- **适用场景**：适用于有状态的Bean，如用户会话相关的对象。

- **实现机制**：

  - **每次通过`getBean()`方法请求时**，Spring容器都会创建一个新的实例。
  - 不会在容器启动时预先创建，只有在请求时才创建。

- **示例**：

  ```xml
  <bean id="myPrototypeBean" class="com.example.MyPrototypeBean" scope="prototype"/>
  ```

#### 3. 请求（Request）

- **定义**：**每个HTTP**请求都会创建一个新的Bean实例，且该实例仅在当前请求内有效。

- **适用场景**：适用于Web应用中与单个HTTP请求相关的Bean，如请求作用域的服务。

- **实现机制**：

  - 仅在Web应用上下文中有效。
  - Spring使用`RequestContextListener`或`RequestContextFilter`来管理请求作用域的Bean。
  - 每个请求都有独立的Bean实例，生命周期与请求一致。

- **示例**：

  ```xml
  <bean id="myRequestBean" class="com.example.MyRequestBean" scope="request"/>
  ```

#### 4. 会话（Session）

- **定义**：每个HTTP会话都会创建一个新的Bean实例，且该实例在整个会话期间有效。

- **适用场景**：适用于Web应用中与用户会话相关的Bean，如用户认证信息。

- **实现机制**：

  - 仅在Web应用上下文中有效。
  - Spring使用`HttpSession`来管理会话作用域的Bean。
  - 每个会话都有独立的Bean实例，生命周期与会话一致。

- **示例**：

  ```xml
  <bean id="mySessionBean" class="com.example.MySessionBean" scope="session"/>
  ```

#### 5. 应用上下文（Application）

- **定义**：在整个Web应用的生命周期内，只存在一个Bean实例。类似于单例，但作用域限定在ServletContext内。

- **适用场景**：适用于需要在整个Web应用中共享的Bean，如应用配置。

- **实现机制**：

  - 仅在Web应用上下文中有效。
  - 使用`ServletContext`来存储和管理Bean实例。

- **示例**：

  ```xml
  <bean id="myApplicationBean" class="com.example.MyApplicationBean" scope="application"/>
  ```

#### 6. WebSocket（WebSocket）

- **定义**：每个WebSocket会话都会创建一个新的Bean实例。

- **适用场景**：适用于与WebSocket会话相关的Bean。

- **实现机制**：

  - 仅在支持WebSocket的Web应用上下文中有效。
  - 使用WebSocket会话来管理Bean实例。

- **示例**：

  ```xml
  <bean id="myWebSocketBean" class="com.example.MyWebSocketBean" scope="websocket"/>
  ```

------

### 作用域的区别总结

|   作用域    |           描述            |         适用场景          |             生命周期             |
| :---------: | :-----------------------: | :-----------------------: | :------------------------------: |
|  Singleton  |  整个容器中只有一个实例   |     无状态服务、DAO等     | 容器启动时创建或第一次请求时创建 |
|  Prototype  |   每次请求都创建新实例    | 有状态对象、用户特定对象  |          每次请求时创建          |
|   Request   |   每个HTTP请求一个实例    | Web应用中与请求相关的Bean |        请求开始到请求结束        |
|   Session   |   每个HTTP会话一个实例    | Web应用中与会话相关的Bean |        会话开始到会话结束        |
| Application |    整个Web应用一个实例    |    Web应用中共享的Bean    |        应用启动到应用关闭        |
|  WebSocket  | 每个WebSocket会话一个实例 |    WebSocket相关的Bean    |     WebSocket会话开始到结束      |

## 2.9,如何在Spring中配置Bean？可以通过哪些方式进行配置？

>  缺陷：主要是两种：隐世@componet，显示 @configation +@bean

1. **XML配置方式**：

   - 通过在XML文件中使用`<bean>`标签定义Bean，设置其类名、属性等。

     ```xml
     <bean id="user" class="com.example.User">
         <property name="id" value="1"/>
         <property name="userName" value="user_name_1"/>
         <property name="note" value="note_1"/>
     </bean>
     ```

   - 优点：直观，适合初学者和集中管理需求的项目。

   - 缺点：文件庞大，维护困难，缺乏类型安全检查。

     

2. **注解配置方式**：

   - 使用`@Component`、`@Service`、`@Repository`、`@Controller`等注解标记类为Bean。

   - 使用`@Autowired`进行依赖注入。或`@Resource`注入依赖。

     ```java
     @Service
     public class UserServiceImpl implements UserService {
         @Autowired
         private UserDao userDao;
     }
     ```

   - 优点：代码简洁，类型安全，符合面向对象编程习惯。

   - 缺点：组件扫描范围不合理可能导致性能问题。

     

3. **Java配置方式（java config  隐试）**：

   - 使用`@Configuration`注解的类，内部定义`@Bean`方法创建Bean。

     ```java
     @Configuration
     public class AppConfig {
         @Bean
         public User user() {
             return new User();
         }
     }
     ```

   - 优点：灵活，可以与依赖注入结合得更好，适合复杂配置。

   - 缺点：需要编写更多Java代码。

4. **手动编程式配置**

   **描述：** 在某些场景下，可以直接通过 API 动态注册 Bean，如使用 `BeanDefinitionRegistry` 或 `GenericApplicationContext`。

   **优点：** 灵活性高，适合在运行时动态生成或修改 Bean 定义；可以结合条件逻辑实现动态配置。

   ```java
   // 一
   GenericApplicationContext context = new GenericApplicationContext();
   context.registerBean(MyBean.class);
   context.refresh();
   
   //二
   // 创建一个默认的可配置的BeanFactory
   ConfigurableListableBeanFactory beanFactory = new DefaultListableBeanFactory();
   // 定义一个Bean
   RootBeanDefinition beanDefinition = new RootBeanDefinition(UserServiceImpl.class);
   beanFactory.registerBeanDefinition("userService", beanDefinition);
   // 获取并使用Bean
   UserService userService = (UserService) beanFactory.getBean("userService");
   userService.doSomething();
   ```

   ### 5. **外部化配置**

   外部化配置是指将应用程序的配置信息（如数据库连接、服务地址、端口号等）从代码中分离出来，集中管理在外部文件（如`application.properties`或`application.yml`）中。

   **特点：**

   - **集中管理**：所有配置信息集中在一个或多个外部文件中，便于维护和修改。
   - **环境适配**：可以通过不同的配置文件（如`application-dev.properties`、`application-prod.properties`）来适配不同的环境（开发、测试、生产）。
   - **动态更新**：某些配置可以在运行时动态更新（如Spring Cloud Config支持动态刷新配置）。

   **示例：**在`application.properties`中定义配置：

   ```yaml
   app.name=MyApplication
   app.version=1.0.0
   database.url=jdbc:mysql://localhost:3306/mydb
   database.username=root
   database.password=secret
   ```

   在代码中通过`@Value`或`@ConfigurationProperties`注入配置：

   ```java
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.stereotype.Component;
   
   @Component
   public class MyAppConfig {
       @Value("${app.name}")
       private String appName;
   
       @Value("${app.version}")
       private String appVersion;
   
       // Getter methods
   }
   ```

### 6, Spring Boot 自动装配

> 机制，流程，触发条件....

1. **条件化加载**：基于类路径、环境变量等动态决定配置类是否生效。
2. **约定优于配置**：提供默认实现，允许开发者通过属性文件覆盖。
3. **SPI 机制**：通过 `spring.factories` 发现和加载配置类。



## 2.10,Spring如何实现事务的管理？如何实现声明式事务？

在 Spring 框架中，事务管理主要通过 **声明式事务管理** 和 **编程式事务管理** 两种方式实现。以下是详细实现方案：

### 1. 配置事务管理器

Spring 事务的核心是 `PlatformTransactionManager` 接口，针对不同持久化技术有不同的实现：

```
@Configuration
@EnableTransactionManagement // 启用注解驱动的事务管理
public class AppConfig {
    @Bean
    public DataSource dataSource() {
        // 配置数据源（如 HikariCP、DBCP 等）
        return new DriverManagerDataSource(...);
    }
    @Bean
    public PlatformTransactionManager transactionManager() {
        // 根据持久化技术选择实现类（如 JPA、JDBC、Hibernate 等）
        return new DataSourceTransactionManager(dataSource());
    }
}
```

------

### **2. 声明式事务（推荐）**

通过 `@Transactional` 注解实现，基于 AOP 动态代理。

#### **基本用法**

```
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;

    @Transactional
    public void createUser(User user) {
        userRepository.save(user);
        // 其他数据库操作（自动在同一个事务中）
    }
}
```

#### **注解参数详解**

```
@Transactional(
    propagation = Propagation.REQUIRED,    // 事务传播行为（默认：当前事务存在则加入，否则新建）
    isolation = Isolation.DEFAULT,         // 隔离级别（默认使用数据库设置）
    timeout = 30,                          // 超时时间（秒）
    readOnly = false,                      // 是否只读事务（优化性能）
    rollbackFor = Exception.class,         // 触发回滚的异常类型（默认仅对RuntimeException回滚）
    noRollbackFor = NullPointerException.class // 不触发回滚的异常
)
public void transactionalMethod() { ... }
```

------

### **3. 编程式事务**

通过 `TransactionTemplate` 或直接使用 `PlatformTransactionManager` 手动控制事务。

#### **使用 TransactionTemplate**

```
@Service
public class OrderService {

    @Autowired
    private TransactionTemplate transactionTemplate;

    public void processOrder() {
        transactionTemplate.execute(status -> {
            try {
                // 业务逻辑（如多个DAO操作）
                return true; // 提交事务
            } catch (Exception e) {
                status.setRollbackOnly(); // 标记回滚
                return false;
            }
        });
    }
}
```

#### **直接使用 PlatformTransactionManager**

```
@Autowired
private PlatformTransactionManager transactionManager;

public void manualTransaction() {
    TransactionDefinition definition = new DefaultTransactionDefinition();
    TransactionStatus status = transactionManager.getTransaction(definition);

    try {
        // 执行业务操作
        transactionManager.commit(status);
    } catch (Exception e) {
        transactionManager.rollback(status);
        throw e;
    }
}
```

------

### **4. 关键概念**

- **传播行为（Propagation）**
  如 `REQUIRED`、`REQUIRES_NEW`、`NESTED`，控制事务边界。
- **隔离级别（Isolation）**
  如 `READ_COMMITTED`、`SERIALIZABLE`，解决并发问题。
- **回滚规则**
  默认对 `RuntimeException` 和 `Error` 回滚，可通过 `rollbackFor` 自定义。

------

### **5. 常见问题**

1. **自调用失效**
   同一类内部方法调用 `@Transactional` 方法时，代理失效（需通过AOP代理对象调用）。
2. **异常处理**
   确保异常未被捕获或正确配置 `rollbackFor`。
3. **多数据源事务**
   需配置多个 `TransactionManager` 并使用 `@Transactional(value = "txManager2")`。

------

### **总结**

- **声明式事务**：简洁高效，适合大多数场景。
- **编程式事务**：灵活控制，适合复杂事务边界需求。

通过合理配置传播行为和隔离级别，可以应对不同业务场景的事务需求。



## Spring 事务的传播行为（了解即可）

在Spring框架中，事务的传播行为（Propagation Behavior）定义了当一个事务方法被另一个事务方法调用时，应该如何处理事务。理解事务的传播行为对于设计健壮的事务管理机制至关重要。Spring提供了七种不同的事务传播行为，每种行为都有其特定的应用场景。

### 1. **REQUIRED（默认）**

- **描述**：如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。

- **适用场景**：大多数情况下的默认选择，适用于需要在一个事务中执行多个操作的情况。

- 示例：

  ```java
  @Transactional(propagation = Propagation.REQUIRED)
  public void methodA() {
      // 业务逻辑
      methodB();
  }
  
  @Transactional(propagation = Propagation.REQUIRED)
  public void methodB() {
      // 业务逻辑
  }
  ```

  在上述示例中，

  ```
  methodA 和methodB将在同一个事务中执行。
  ```

### 2. **SUPPORTS**

- **描述**：如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务方式执行。

- **适用场景**：适用于方法可以在有事务或无事务的情况下执行，且不强制要求事务的场景。

- 示例：

  ```java
  @Transactional(propagation = Propagation.SUPPORTS)
  public void methodC() {
      // 业务逻辑
  }
  ```

### 3. **MANDATORY**

- **描述**：如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。

- **适用场景**：适用于方法必须在事务中执行，不允许在无事务的情况下调用的场景。

- 示例：

  ```java
  @Transactional(propagation = Propagation.MANDATORY)
  public void methodD() {
      // 业务逻辑
  }
  ```

  如果methodD被一个非事务方法调用，将会抛出IllegalTransactionStateException。

### 4. **REQUIRES_NEW**

- **描述**：无论当前是否存在事务，都创建一个新的事务。如果当前存在事务，则将当前事务挂起。

- **适用场景**：适用于需要独立于外部事务执行的方法，如日志记录、发送通知等。

- 示例：

  ```java
  @Transactional(propagation = Propagation.REQUIRES_NEW)
  public void methodE() {
      // 业务逻辑
  }
  ```

  调用methodE时，会启动一个新的事务，而不会影响外部事务。

### 5. **NOT_SUPPORTED**

- **描述**：以非事务方式执行操作。如果当前存在事务，则将当前事务挂起。

- **适用场景**：适用于不需要事务支持的方法，如只读操作或某些性能优化场景。

- 示例：

  ```java
  @Transactional(propagation = Propagation.NOT_SUPPORTED)
  public void methodF() {
      // 业务逻辑
  }
  ```

### 6. **NEVER**

- **描述**：以非事务方式执行，如果当前存在事务，则抛出异常。

- **适用场景**：适用于绝对不允许在事务中执行的方法。

- 示例：

  ```java
  @Transactional(propagation = Propagation.NEVER)
  public void methodG() {
      // 业务逻辑
  }
  ```

  如果methodG被一个事务方法调用，将会抛出IllegalTransactionStateException。

### 7. **NESTED**

- **描述**：如果当前存在事务，则在嵌套事务中执行；如果当前没有事务，则创建一个新的事务。嵌套事务是外部事务的一部分，可以独立提交或回滚，而不会影响外部事务。

- **适用场景**：适用于需要在事务内部执行部分操作，且这些操作可以独立回滚的场景。

- 示例：

  ```java
  @Transactional(propagation = Propagation.NESTED)
  public void methodH() {
      // 业务逻辑
  }
  ```

  methodH将在一个嵌套事务中执行，允许其独立于外部事务进行回滚。

### 总结

|   传播行为    |                   描述                   |            适用场景            |
| :-----------: | :--------------------------------------: | :----------------------------: |
|   REQUIRED    |     如果存在事务则加入，否则新建事务     |         大多数通用场景         |
|   SUPPORTS    |    如果存在事务则加入，否则非事务执行    |         可选事务的方法         |
|   MANDATORY   |       必须在事务中执行，否则抛异常       |       强制要求事务的方法       |
| REQUIRES_NEW  |        总是新建事务，挂起当前事务        | 需要独立事务的方法，如日志记录 |
| NOT_SUPPORTED |         非事务执行，挂起当前事务         |        不需要事务的方法        |
|     NEVER     |       非事务执行，存在事务时抛异常       |  绝对不允许在事务中执行的方法  |
|    NESTED     | 存在事务时在嵌套事务中执行，否则新建事务 |   需要部分操作独立回滚的场景   |

### 使用建议

- **默认使用REQUIRED**：大多数情况下，默认的`REQUIRED`传播行为已经足够满足需求。
- **独立操作使用REQUIRES_NEW**：当某些操作需要独立于外部事务执行时，使用`REQUIRES_NEW`。
- **避免不必要的嵌套**：`NESTED`传播行为虽然灵活，但可能会增加复杂性，应根据实际需求谨慎使用。
- **明确事务边界**：通过合理的事务传播行为配置，确保事务边界清晰，避免事务过大或过小导致的数据一致性问题。

理解并正确应用事务传播行为，有助于构建健壮、可维护且高效的事务管理机制，从而提升应用程序的整体质量和性能。



## 2.11,什么是Spring的Event机制？如何发布和监听事件？

Spring 的 Event 机制主要基于观察者模式（Observer Pattern），其核心原理包括事件发布、事件监听和事件分发三个主要部分。下面详细介绍其实现原理：

### 1. **事件对象（Event Object）**

### 2.**事件发布者（Event Publisher）**

### 3. **事件监听器（Event Listener）**



## 2.12,Spring如何支持异步编程？如何使用@Async注解实现异步任务

Spring 通过多种方式支持异步编程，主要包括使用 `@Async` 注解、自定义线程池



## 2.13,==重点==Spring的循环依赖问题如何解决？如何使用三级缓存机制？

> 第一解决： 属性，setter 上的循环依赖： 三级缓存
>
> 第二解决：构造器上的循环依赖，@Lazy 懒加载
>
> 第三解决: 合理设计依赖关系
>
> 第四解决：把构造上的循环依赖，改成属性或者setter 依赖

Spring框架中的循环依赖问题通常发生在两个或多个Bean相互依赖的情况下。例如，Bean A依赖于Bean B，而Bean B又依赖于Bean A。这种情况下，Spring容器无法确定Bean的创建顺序，从而导致循环依赖问题。

Spring解决循环依赖问题的方法主要有以下几种：

1. 三级缓存机制：
   Spring容器在创建Bean的过程中，使用了三级缓存机制来解决循环依赖问题。这三级缓存分别是：

- 一级缓存（singletonObjects）：存储已经创建好的单例Bean。
- 二级缓存（earlySingletonObjects）：存储提前暴露的单例Bean的引用，用于解决循环依赖。
- 三级缓存（singletonFactories）：存储Bean的工厂对象（BeanWrapper），用于创建Bean的代理对象。

当Spring容器创建Bean时，首先会从一级缓存中查找是否已经存在该Bean。如果不存在，则创建一个新的Bean，并将其工厂对象放入三级缓存。接着，Spring容器会尝试注入该Bean的依赖。如果依赖的Bean也正在创建过程中，那么可以从三级缓存中获取该Bean的工厂对象，创建一个提前暴露的Bean引用，并将其放入二级缓存。这样，当依赖的Bean再次尝试注入当前Bean时，可以从二级缓存中获取到提前暴露的Bean引用，从而避免了循环依赖的问题。

1. 使用构造器注入：
   在某些情况下，可以通过使用构造器注入来解决循环依赖问题。构造器注入可以确保所有的依赖在Bean创建时就已注入，从而避免了循环依赖的问题。但是，这种方法可能会导致Bean的创建和依赖注入过程变得复杂，因此需要谨慎使用。
2. 使用`@Lazy`注解：
   在某些情况下，可以使用`@Lazy`注解来解决循环依赖问题。`@Lazy`注解可以让Spring容器延迟初始化Bean，直到该Bean被实际使用。这样，可以避免在创建Bean时就触发循环依赖的问题。你可以在依赖注入的地方（如构造器、setter方法或字段）使用`@Lazy`注解。
3. 重新设计类结构：
   如果循环依赖问题无法通过上述方法解决，那么可能需要重新设计类结构。你可以尝试将相互依赖的部分提取到一个新的类中，或者使用接口、抽象类等方式降低类之间的耦合度。

总之，解决Spring中的循环依赖问题需要根据具体情况选择合适的方法。在大多数情况下，可以通过调整依赖注入方式、使用`@Lazy`注解或重新设计类结构来解决循环依赖问题。



```java
@Component
public class BeanA {
    private final BeanB beanB;

    @Autowired
    public BeanA(@Lazy BeanB beanB) {
        this.beanB = beanB;
    }
}

@Component
public class BeanB {
    private final BeanA beanA;

    @Autowired
    public BeanB(BeanA beanA) {
        this.beanA = beanA;
    }
}
```



## 三级缓存机制解决循环依赖原理？

> 三级缓存机制是用于解决**循环依赖**问题的核心机制。循环依赖指的是两个或多个Bean之间相互依赖，形成一个闭环，导致Spring容器无法正确地创建这些Bean。为了解决这一问题，Spring采用了三级缓存机制，通过分阶段管理和缓存Bean的不同状态，确保Bean能够正确地创建和注入依赖。

Spring的三级缓存机制主要包括以下三个缓存：

1. **一级缓存（singletonObjects）**：
   - **作用**：存储已经完全初始化好的单例Bean实例。
   - **特点**：只有在Bean完全创建并初始化完成后，才会被放入一级缓存中。
2. **二级缓存（earlySingletonObjects）**：
   - **作用**：存储早期暴露的单例Bean的引用，这些Bean可能尚未完成全部属性的注入。
   - **特点**：用于解决循环依赖问题，允许其他Bean在当前Bean完全初始化之前引用它。
3. **三级缓存（singletonFactories）**：
   - **作用**：存储Bean的工厂对象（通常是`ObjectFactory`或`Supplier`），用于创建早期暴露的Bean引用/存储Bean的工厂对象（BeanWrapper），用于创建Bean的代理对象。。
   - **特点**：通过工厂对象，可以在需要时动态创建早期Bean引用，进一步解耦Bean的创建过程。

**工作流程**：

1. 创建Bean A：
   - Spring开始创建`BeanA`，并将其工厂对象存入三级缓存。
2. 注入依赖Bean B：
   - 在创建`BeanA`的过程中，发现需要注入`BeanB`。
3. 创建Bean B：
   - Spring开始创建`BeanB`，并将其工厂对象存入三级缓存。
4. 注入依赖Bean A：
   - 在创建`BeanB`的过程中，发现需要注入`BeanA`。
5. 通过工厂对象获取Bean A：
   - Spring检查一级缓存未发现`BeanA`，然后检查二级缓存也未发现，接着从三级缓存中获取`BeanA`的工厂对象，并通过该工厂创建`BeanA`的早期引用，将其存入二级缓存并注入到`BeanB`中。
6. 完成Bean B的创建：
   - `BeanB`创建完成后，被移入一级缓存。
7. 完成Bean A的创建：
   - `BeanA`完成全部属性注入后，被移入一级缓存。

```java
protected Object getSingleton(String beanName, boolean allowEarlyReference) {
    Object singletonObject = this.singletonObjects.get(beanName);
    if (singletonObject == null && isSingletonCurrentlyInCreation(beanName)) {
        synchronized (this.singletonObjects) {
            singletonObject = this.earlySingletonObjects.get(beanName);
            if (singletonObject == null && allowEarlyReference) {
                ObjectFactory<?> singletonFactory = this.singletonFactories.get(beanName);
                if (singletonFactory != null) {
                    singletonObject = singletonFactory.getObject();
                    this.earlySingletonObjects.put(beanName, singletonObject);
                    this.singletonFactories.remove(beanName);
                }
            }
        }
    }
    return singletonObject;
}
```



## 2.14,@Autowired和@Qualifier，@Value注解和@ConfigurationProperties，其他重要注解

> 根据类别可以分为：
>
> 1. 组件相关
> 2. 依赖注入
> 3. java配置
> 4. 属性注入
> 5. 生命周期和作用域
> 6. 事务相关
> 7. 异步相关
> 8. web相关

| 注解                       | 所属分类         | 作用说明                                                     | 示例代码                                                     |
| -------------------------- | ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **@Component**             | 组件扫描与管理   | 标识普通组件，被自动扫描并注册为 Bean                        | `@Component public class MyComponent { }`                    |
| **@Service**               | 组件扫描与管理   | 标识业务层组件，语义化更明确                                 | `@Service public class UserService { }`                      |
| **@Repository**            | 组件扫描与管理   | 标识持久层组件，同时支持数据访问异常转换                     | `@Repository public class UserRepository { }`                |
| **@Controller**            | 组件扫描与管理   | 标识控制器组件，用于处理 HTTP 请求                           | `@Controller public class HomeController { }`                |
| **@RestController**        | 组件扫描与管理   | 组合了 @Controller 与 @ResponseBody，专用于构建 RESTful API  | `@RestController public class ApiController { }`             |
| **@Autowired**             | 依赖注入         | 根据类型自动注入 Bean                                        | `@Autowired private OrderRepository orderRepository;`        |
| **@Qualifier**             | 依赖注入         | 当存在多个同类型 Bean 时指定具体要注入的 Bean                | `@Autowired @Qualifier("specificBean") private SomeService someService;` |
| **@Resource**              | 依赖注入         | 通过名称或类型进行注入，属于 JSR-250 提供的注解              | `@Resource private UserService userService;`                 |
| **@Configuration**         | Java 配置        | 声明配置类，替代 XML 配置，可用于定义 Bean                   | `@Configuration public class AppConfig { }`                  |
| **@Bean**                  | Java 配置        | 在配置类中定义一个 Bean 并交由 Spring 管理                   | `@Bean public DataSource dataSource() { /* ... */ }`         |
| **@ComponentScan**         | Java 配置        | 指定包路径，自动扫描并注册带有 @Component 及其派生注解的类   | `@ComponentScan("com.example.app")`                          |
| **@Value**                 | 属性注入         | 注入外部化配置的属性值（例如配置文件中的值）                 | `@Value("${app.name}") private String appName;`              |
| **@PropertySource**        | 属性注入         | 指定属性文件位置，加载外部配置                               | `@PropertySource("classpath:application.properties")`        |
| **@Scope**                 | 作用域与生命周期 | 指定 Bean 的作用域（如 singleton、prototype 等）             | `@Scope("prototype") public class PrototypeBean { }`         |
| **@Lazy**                  | 作用域与生命周期 | 延迟初始化 Bean，只有在首次使用时才创建                      | `@Lazy public class LazyBean { }`                            |
| **@PostConstruct**         | 生命周期         | Bean 初始化后执行的方法                                      | `@PostConstruct public void init() { /* ... */ }`            |
| **@PreDestroy**            | 生命周期         | Bean 销毁前执行的方法                                        | `@PreDestroy public void destroy() { /* ... */ }`            |
| **@Transactional**         | 事务管理         | 声明式事务管理，确保方法在事务中执行                         | `@Transactional public void processPayment() { /* ... */ }`  |
| **@SpringBootApplication** | Spring Boot      | 应用入口注解，组合了 @Configuration、@EnableAutoConfiguration 和 @ComponentScan | `@SpringBootApplication public class Application { public static void main(String[] args) { SpringApplication.run(Application.class, args); } }` |
| **@RequestMapping**        | Web 请求映射     | 将 HTTP 请求映射到控制器的方法                               | `@RequestMapping("/users")`                                  |
| **@GetMapping**            | Web 请求映射     | 映射 HTTP GET 请求                                           | `@GetMapping("/users")`                                      |
| **@PostMapping**           | Web 请求映射     | 映射 HTTP POST 请求                                          | `@PostMapping("/users")`                                     |

这个表格将 Spring 框架中常用的重要注解按类别整理，方便你快速了解每个注解的用途和使用示例。



## 2.15，`@Component`、`@Configuration` 和 `@Bean` 都用于定义 Bean，但它们的用途和适用场景有显著区别。

### **1. `@Component`：隐式 Bean 定义**

- **作用**：
  标记一个类为 Spring 管理的 Bean，通过**组件扫描**（`@ComponentScan`）自动注册到容器中。

- **适用场景**：
  直接定义自己编写的类为 Bean（如 Service、Repository 等）。

  ```java
  @Component // 自动注册为 Bean
  public class UserService {
      // ...
  }
  ```

------

### **2. `@Configuration` + `@Bean`：显式 Bean 定义**

- 作用：

  在 Java 配置类中显式定义 Bean，通常用于以下场景：

  - 需要**复杂初始化逻辑**（如配置数据源、第三方库的类）。
  - 需要**条件化创建 Bean**（如根据环境动态决定 Bean 的实现）。
  - 需要**手动控制 Bean 的创建过程**。

  ```java
  @Configuration // 标记为配置类
  public class AppConfig {
      @Bean // 显式定义一个 Bean
      public DataSource dataSource() {
          return new HikariDataSource(...); // 复杂初始化逻辑
      }
  }
  ```

------

### **核心区别**

|     **特性**      |           `@Component`           |      `@Configuration` + `@Bean`      |
| :---------------: | :------------------------------: | :----------------------------------: |
|   **定义方式**    |         隐式（自动扫描）         |         显式（手动编码定义）         |
|   **适用对象**    |           自己编写的类           |  第三方库的类、需要复杂逻辑的 Bean   |
|   **控制粒度**    |       类级别（直接标记类）       |    方法级别（通过方法返回 Bean）     |
|  **初始化逻辑**   | 简单（依赖默认构造器或自动注入） | 复杂（可在方法中编写任意初始化代码） |
|  **条件化配置**   |    需结合 `@Conditional` 注解    |      可直接在方法中编写条件逻辑      |
| **Bean 依赖管理** |   自动注入（如 `@Autowired`）    |   可通过方法调用直接引用其他 Bean    |

------

### **关键细节**

#### (1) `@Bean` 的灵活性

- **第三方库的类**：如果某个类不在你的代码库中（如 `RestTemplate`、`DataSource`），无法直接添加 `@Component` 注解，此时必须用 `@Bean` 显式定义。

  ```java
  @Configuration
  public class AppConfig {
      @Bean
      public RestTemplate restTemplate() {
          return new RestTemplate();
      }
  }
  ```

- **条件化 Bean**：结合 `@Profile` 或 `@Conditional` 实现动态 Bean 创建。

  ```java
  @Configuration
  public class AppConfig {
      @Bean
      @Profile("dev") // 仅在 dev 环境下生效
      public DataSource devDataSource() {
          return new EmbeddedDatabaseBuilder().build();
      }
  }
  ```

------

#### (2) `@Configuration` 的代理机制

- **CGLIB 增强**：`@Configuration` 类会被 Spring 代理，确保 `@Bean` 方法之间的调用返回的是**容器中的单例对象**（而非每次创建新对象）。
  例如：

  ```java
  @Configuration
  public class AppConfig {
      @Bean
      public A a() { return new A(); }
  
      @Bean
      public B b() { 
          return new B(a()); // 调用 a() 会返回容器中的单例 A
      }
  }
  ```

- **对比 `@Component`**：如果使用 `@Component` 替代 `@Configuration`，`@Bean` 方法之间的调用会直接创建新对象（不经过代理），导致单例失效。

------

### **总结：何时用哪种方式？**

- **用 `@Component`**：自己编写的类，且无需复杂初始化逻辑。
- 用 `@Configuration` + `@Bean`：
  - 第三方库的类（无法修改源码添加 `@Component`）。
  - 需要条件化或复杂初始化的 Bean。
  - 需要显式控制 Bean 的创建过程（如工厂方法）。

------

### **代码对比**

#### 场景：定义一个 `RestTemplate` Bean

- **使用 `@Component`（不可行）**：无法修改 `RestTemplate` 的源码添加 `@Component`。

- **使用 `@Bean`（正确方式）**：

  ```java
  @Configuration
  public class AppConfig {
      @Bean
      public RestTemplate restTemplate() {
          return new RestTemplate();
      }
  }
  ```

------

通过合理选择 `@Component` 和 `@Configuration` + `@Bean`，可以更灵活地管理 Spring 容器中的 Bean。



前置知识：

gradle，junit 测试框架

### 源代码

：https://github.com/spring-projects/spring-framework

使用 gradel 编译时，选对 jdk 版本

![image-20241211160533902](/Users/eric/Desktop/笔记/复习日记/img/image-20241211160533902.png)

测试代码

![image-20241211163716962](/Users/eric/Desktop/笔记/复习日记/img/image-20241211163716962.png)

调试技巧

![image-20241211213338923](/Users/eric/Desktop/笔记/复习日记/img/image-20241211213338923.png)

关键类：

```java
//定义了bean 获取相关，接口 
// getBean
BeanFactory
//定义了 createBean   创建
AbstractBeanFactory  
// 又在 其子类中实现，创建实列 ，属性赋值
AbstractAutowireCapableBeanFactory  
// 重要
DefaultListableBeanFactory  
//把各种 扫描到注解 的类
ConfigurationClassParser
```

![image-20241211220554883](/Users/eric/Desktop/笔记/复习日记/img/image-20241211220554883.png)

> 创建实列 ：createBeanInstance



![image-20241211224111056](/Users/eric/Desktop/笔记/复习日记/img/image-20241211224111056.png)

扫描



```java
ClassPathBeanDefinitionScanner
AnnotationConfigApplicationContext. scan, 
org. springframework. stereotype. Component, 
org. springframework. stereotype. Repository, 
org. springframework. stereotype. Service, 
org. springframework. stereotype. Controller
```



### bean 

```
AbstractAutowireCapableBeanFactory
```



```java
protected Object doCreateBean(String beanName, RootBeanDefinition mbd, @Nullable Object[] args)
			throws BeanCreationException {

		// Instantiate the bean.
		BeanWrapper instanceWrapper = null;
		if (mbd.isSingleton()) {
			instanceWrapper = this.factoryBeanInstanceCache.remove(beanName);
		}
		if (instanceWrapper == null) {
			instanceWrapper = createBeanInstance(beanName, mbd, args);
		}
		Object bean = instanceWrapper.getWrappedInstance();
		Class<?> beanType = instanceWrapper.getWrappedClass();
		if (beanType != NullBean.class) {
			mbd.resolvedTargetType = beanType;
		}

		// Allow post-processors to modify the merged bean definition.
		synchronized (mbd.postProcessingLock) {
			if (!mbd.postProcessed) {
				try {
					applyMergedBeanDefinitionPostProcessors(mbd, beanType, beanName);
				}
				catch (Throwable ex) {
					throw new BeanCreationException(mbd.getResourceDescription(), beanName,
							"Post-processing of merged bean definition failed", ex);
				}
				mbd.markAsPostProcessed();
			}
		}

		// Eagerly cache singletons to be able to resolve circular references
		// even when triggered by lifecycle interfaces like BeanFactoryAware.
		boolean earlySingletonExposure = (mbd.isSingleton() && this.allowCircularReferences &&
				isSingletonCurrentlyInCreation(beanName));
		if (earlySingletonExposure) {
			if (logger.isTraceEnabled()) {
				logger.trace("Eagerly caching bean '" + beanName +
						"' to allow for resolving potential circular references");
			}
			addSingletonFactory(beanName, () -> getEarlyBeanReference(beanName, mbd, bean));
		}

		// Initialize the bean instance.
		Object exposedObject = bean;
		try {
			populateBean(beanName, mbd, instanceWrapper);
			exposedObject = initializeBean(beanName, exposedObject, mbd);
		}
		catch (Throwable ex) {
			if (ex instanceof BeanCreationException bce && beanName.equals(bce.getBeanName())) {
				throw bce;
			}
			else {
				throw new BeanCreationException(mbd.getResourceDescription(), beanName, ex.getMessage(), ex);
			}
		}

		if (earlySingletonExposure) {
			Object earlySingletonReference = getSingleton(beanName, false);
			if (earlySingletonReference != null) {
				if (exposedObject == bean) {
					exposedObject = earlySingletonReference;
				}
				else if (!this.allowRawInjectionDespiteWrapping && hasDependentBean(beanName)) {
					String[] dependentBeans = getDependentBeans(beanName);
					Set<String> actualDependentBeans = new LinkedHashSet<>(dependentBeans.length);
					for (String dependentBean : dependentBeans) {
						if (!removeSingletonIfCreatedForTypeCheckOnly(dependentBean)) {
							actualDependentBeans.add(dependentBean);
						}
					}
					if (!actualDependentBeans.isEmpty()) {
						throw new BeanCurrentlyInCreationException(beanName,
								"Bean with name '" + beanName + "' has been injected into other beans [" +
								StringUtils.collectionToCommaDelimitedString(actualDependentBeans) +
								"] in its raw version as part of a circular reference, but has eventually been " +
								"wrapped. This means that said other beans do not use the final version of the " +
								"bean. This is often the result of over-eager type matching - consider using " +
								"'getBeanNamesForType' with the 'allowEagerInit' flag turned off, for example.");
					}
				}
			}
		}

		// Register bean as disposable.
		try {
			registerDisposableBeanIfNecessary(beanName, bean, mbd);
		}
		catch (BeanDefinitionValidationException ex) {
			throw new BeanCreationException(
					mbd.getResourceDescription(), beanName, "Invalid destruction signature", ex);
		}

		return exposedObject;
	}
```

```java
package com.example.demo;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.BeanFactory;
import org.springframework.beans.factory.BeanFactoryAware;
import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.config.BeanPostProcessor;
import org.springframework.stereotype.Component;

@Component
public class EricBean implements BeanPostProcessor , BeanFactoryAware, DisposableBean {

    public EricBean() {
        System.out.println("实列化----- ");
    }
    private String id;
    BeanFactory beanFactory;

    public void setEngine(String engine) {
        this.id = engine;
    }

    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        // 在初始化之前对 Bean 进行处理
        System.out.println(" 在初始化之前对 Bean 进行处理" + beanName);
        return bean;  // 必须返回 Bean 实例
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        //
        System.out.println("在初始化之后对 Bean 进行处理 " + beanName);
        return bean;  // 必须返回 Bean 实例
    }

    @Override
    public void setBeanFactory(BeanFactory beanFactory) throws BeansException {
        this.beanFactory=beanFactory;
        System.out.println("属性赋值");
    }

    @Override
    public void destroy() throws Exception {
        System.out.println("销毁");
    }
}

```

 实列化，属性赋值，初始前，初始化， 初始化后，销毁

```shell
实列化----- 
属性赋值
 在初始化之前对 Bean 进行处理org.springframework.boot.autoconfigure.web.servlet.ServletWebServerFactoryConfiguration$EmbeddedTomcat
在初始化之后对 Bean 进行处理 org.springframework.boot.autoconfigure.web.servlet.ServletWebServerFactoryConfiguration$EmbeddedTomcat
 在初始化之前对 Bean 进行处理tomcatServletWebServerFactory
........
销毁
```



1. 定义 

2. 扫描 定义

3. 创建实列 ：createBeanInstance

4. 实列的属性赋值； 自定义属性，aware

5. bean 的作用 scope

    **singleton**（默认作用域）：在整个Spring容器中只会创建一个Bean实例。

   **prototype**：每次请求都会创建一个新的Bean实例。即每次注入该Bean时，都会得到一个新的对象。

   **request**：对于每个HTTP请求都会创建一个新的Bean实例，通常用于Web应用中。

   **session**：对于每个HTTP会话会创建一个新的Bean实例，通常用于Web应用中。

   **application**：在整个ServletContext范围内共享一个Bean实例。

   **websocket**：在WebSocket连接的生命周期内共享一个Bean实例。

==自定义org.springframework.beans.factory.config.scope==





==request==

```java
@Component
@Scope("request")
public class MyRequestBean {

    private String requestId;

    public MyRequestBean() {
        // 每个请求实例化时会自动调用这个构造函数
        this.requestId = UUID.randomUUID().toString();  // 给每个请求生成唯一的ID
    }

    public String getRequestId() {
        return requestId;
    }

    public void setRequestId(String requestId) {
        this.requestId = requestId;
    }
}

```

**`@Scope("request")`** 使得每个HTTP请求都会生成一个新的Bean实例，这些实例是请求级别的，在请求处理期间有效，并且请求结束后被销毁。

Spring使用 **`ThreadLocal`** 来为每个线程（即每个HTTP请求线程）维护一个请求作用域的Bean实例。

**`RequestContextListener`** 或 **`RequestContextFilter`** 用于在Web应用中管理请求作用域的Bean实例，它们会确保在每次请求时创建新的实例，并在请求结束后销毁实例。



###   bean 的两个扩展  BeanPostProcessor； BeanFactoryPostProcessor

BeanPostProcessor 实现 aop

`BeanFactoryPostProcessor` 作为 Spring 容器的一个扩展点，可以用于多种高级功能，主要包括：

- 修改 Bean 的属性值、作用域、初始化方法等。
- 动态注册和修改 Bean 定义。
- 控制 Bean 的生命周期，修改初始化方法、销毁方法等。
- 改变自动装配方式，动态调整 Bean 的依赖关系。
- 启用懒加载等。



在 Spring 中，如果容器默认加载了一个 `Bean`，并且你希望替换这个 `Bean`，可以通过几种不同的方式来实现。这些方式涉及到 Spring 的 `BeanDefinition` 的修改，或者在容器初始化阶段动态地替换或修改 Bean 的定义。以下是几种常见的方法来替换一个默认加载的 `Bean`：

### 1. **使用 `BeanFactoryPostProcessor` 替换默认的 Bean**

`BeanFactoryPostProcessor` 是一个容器初始化后、所有 `Bean` 实例化之前的钩子方法。在这个阶段，你可以获取容器中的所有 `BeanDefinition`，并根据需要修改或替换 Bean 的定义。

#### 示例：使用 `BeanFactoryPostProcessor` 替换默认 Bean

假设你有一个 `MyBean` 类，并且容器已经默认加载了一个 `MyBean` Bean。你想要替换它的定义或更改它的配置。

```java
import org.springframework.beans.factory.config.BeanDefinition;
import org.springframework.beans.factory.config.BeanFactoryPostProcessor;
import org.springframework.beans.factory.support.BeanDefinitionRegistry;

public class BeanReplacementPostProcessor implements BeanFactoryPostProcessor {

    @Override
    public void postProcessBeanFactory(org.springframework.beans.factory.config.ConfigurableListableBeanFactory beanFactory) {
        if (beanFactory instanceof BeanDefinitionRegistry) {
            BeanDefinitionRegistry registry = (BeanDefinitionRegistry) beanFactory;

            String beanName = "myBean"; // 默认的 Bean 名称

            // 如果默认的 Bean 存在，进行替换
            if (registry.containsBeanDefinition(beanName)) {
                // 获取默认 Bean 定义
                BeanDefinition beanDefinition = registry.getBeanDefinition(beanName);

                // 创建新的 Bean 定义，并进行替换（可以更改类、属性等）
                BeanDefinition newBeanDefinition = new MyNewBeanDefinition();
                 
               // GenericBeanDefinition newBeanDefinition = new GenericBeanDefinition();
               // newBeanDefinition.setBeanClass(MyNewBean.class); // 新的 Bean 实现类
               // newBeanDefinition.setScope(beanDefinition.getScope()); 

                // 替换现有 Bean 定义
                registry.registerBeanDefinition(beanName, newBeanDefinition);
            }
        }
    }
}
```

然后，在你的 `@Configuration` 配置类中注册这个 `BeanFactoryPostProcessor`：

```java
@Configuration
public class AppConfig {

    @Bean
    public static BeanFactoryPostProcessor beanFactoryPostProcessor() {
        return new BeanReplacementPostProcessor();
    }

    @Bean(name = "myBean")
    public MyBean myBean() {
        return new MyBean();
    }
}
```

在这个示例中，我们通过 `BeanFactoryPostProcessor` 在容器启动时替换了默认的 `MyBean` Bean。你可以替换其类、属性、作用域等。

### 2. **通过条件化 Bean 注册来替换默认的 Bean**

另一种常见的做法是通过条件化注册 Bean 来替换默认的 Bean。可以通过使用 `@Conditional` 注解或者 `@Profile` 注解来根据条件动态决定是否加载默认的 Bean，或者在某些条件下注册自定义的 Bean。

#### 示例：使用 `@Profile` 替换默认的 Bean

```
@Configuration
public class AppConfig {

    @Bean(name = "myBean")
    @Profile("dev")  // 只在 "dev" profile 下注册此 Bean
    public MyBean myBean() {
        return new MyBean();
    }

    @Bean(name = "myBean")
    @Profile("prod")  // 只在 "prod" profile 下注册另一个 Bean
    public MyBean myNewBean() {
        return new MyNewBean();  // 替换为另一个实现
    }
}
```

在这个示例中，`@Profile` 注解用于在不同的环境（如开发环境 `dev` 和生产环境 `prod`）中加载不同的 Bean 实现。你可以通过切换不同的 `profile` 来替换默认的 `Bean`。

### 3. **通过 `@Primary` 和 `@Qualifier` 注解来实现替换**

如果有多个 `Bean` 具有相同类型，但你希望在某些场合替换默认的 `Bean`，可以使用 `@Primary` 注解标记一个 Bean 为首选 Bean，或者使用 `@Qualifier` 注解来明确指定注入哪个 Bean。

#### 示例：使用 `@Primary` 替换默认的 Bean

```
@Configuration
public class AppConfig {

    @Bean(name = "myBean")
    public MyBean myBean() {
        return new MyBean();
    }

    @Bean(name = "myNewBean")
    @Primary  // 设置为首选 Bean
    public MyBean myNewBean() {
        return new MyNewBean();  // 这个 Bean 将会被默认注入
    }
}
```

在这个示例中，`myNewBean` 被标记为首选的 `Bean`，因此如果有多个 `MyBean` 类型的 Bean，在没有明确指定 `@Qualifier` 的情况下，会默认注入 `myNewBean`。

### 4. **使用 `@Bean` 方法替换**

如果你能够访问到 `@Configuration` 类并且可以修改配置类，那么直接通过 `@Bean` 方法替换默认的 Bean 是最简单的方法。

#### 示例：直接替换 `@Bean` 定义

```
@Configuration
public class AppConfig {

    @Bean(name = "myBean")
    public MyBean myBean() {
        return new MyNewBean();  // 直接替换默认的 MyBean
    }
}
```

通过这种方式，你直接用 `MyNewBean` 替换了默认的 `MyBean`。

### 5. **使用 `@Import` 动态替换 Bean**

`@Import` 注解允许你动态导入其他的配置类。如果你想替换默认的 Bean，可以使用 `@Import` 将新的配置类导入到现有配置中。

#### 示例：使用 `@Import` 替换 Bean

```java
@Configuration
@Import(AnotherConfig.class)  // 导入替换 Bean 的配置类
public class AppConfig {

    @Bean(name = "myBean")
    public MyBean myBean() {
        return new MyBean();
    }
}

@Configuration
public class AnotherConfig {

    @Bean(name = "myBean")
    public MyNewBean myNewBean() {
        return new MyNewBean();  // 替换 myBean
    }
}
```

在这个示例中，`AppConfig` 中定义了一个默认的 `myBean`，但通过 `@Import` 导入了 `AnotherConfig` 配置类，`AnotherConfig` 中定义了一个同名的 `myBean`，从而实现了替换。

### 6. **使用 `ApplicationContext` 动态修改 Bean 定义**

如果你不想在容器初始化时替换默认的 Bean，还可以在容器启动之后通过 `ApplicationContext` 直接修改 Bean 定义，或者删除旧的 Bean 并注册新的 Bean。

#### 示例：动态修改 Bean 定义

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class BeanReplacer {

    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

        // 动态修改 Bean 定义
        if (context.containsBeanDefinition("myBean")) {
            context.getBeanFactory().removeBeanDefinition("myBean");

            // 注册新 Bean
            context.getBeanFactory().registerBeanDefinition("myBean", new RootBeanDefinition(MyNewBean.class));
        }

        // 获取新替换的 Bean
        MyBean bean = context.getBean("myBean", MyBean.class);
        System.out.println(bean.getClass().getName());

        context.close();
    }
}
```

在这个示例中，容器启动后，我们动态地从 `ApplicationContext` 中移除旧的 `myBean` 定义，并注册新的 `MyNewBean`。

### 如何替换一个默认的bean？

替换 Spring 容器中的默认 Bean 可以通过多种方式来实现，选择哪种方法取决于你的需求和容器的初始化方式：

- **`BeanFactoryPostProcessor`**：在容器启动后、Bean 实例化前修改 Bean 定义。
- **`@Profile`**：基于环境的配置替换。
- **`@Primary` 和 `@Qualifier`**：在多个候选 Bean 中选择默认 Bean。
- **直接修改 `@Bean` 定义**：直接在配置类中替换 Bean。
- **`@Import`**：通过导入其他配置类来替换 Bean。
- **动态修改**：在容器初始化后，通过 `ApplicationContext` 动态修改 Bean 定义。



### ioc

1. 定义

   Inversion of Control  控制反转，是spring 框架核心概率，翻译成控制反转；所谓控制：就是对对象的管理，如创建，销毁等，所谓反转：就是本应该有对象自己管理内部的子对象这些动作交给了spring；

   ==容器==：内部是 concrentHashMap 的一个 key value结构存储；vlaue：  把配置的 bean 对象：如 xml 配置的bean 节点，或者通过 @componet 标注的 等标注的对象等；key： 对象的名称（id）；

   ==容器==： BeanFactory 顶层接口，它的各种实现类

   ==如何实现反转==：通过依赖注入，就是“赋值” 把对象内部 属性赋值，也是通过配置：如 xml 中 ref，或者通过 注解 @autowired 或者 @resource 等； ==注意== 注解虽然是代码 但也可以理解为配置；

   注入方式： 构造器，字段，setter；

2. 控制反转

   

3.  依赖注入

   

4. ioc 的作用和优点

   ==解偶== 通过统一的入口也就是容器来管理对象/或者组件实现 他们之间的耦合，比如： 通过配置优先级来替换一些对象；或者手动通过 名称来替换容器内部的某个对象； 具体操作： @Primary 配置默认，@Qualifier 优先级； 或者根据不同环境指定不同的bean；

   

### aop

https://docs.spring.io/spring-framework/reference/core/aop-api/advice.html

#### 什么是aop

AOP又名Aspect Oriented Programming 意为 ‘[面向切面编程](https://so.csdn.net/so/search?q=面向切面编程&spm=1001.2101.3001.7020)’通过预编译和运行期间动态代理来实现程序功能增强，在不改变原有的代码时。

#### 他有哪些应用场景

1. 缓存，权限，日志，事务，方法的功能增强；

# spring 中的aop 

### Spring AOP 与 AspectJ 的结合

在 Spring AOP 中，`@EnableAspectJAutoProxy` 启用的实际上是 **AspectJ 注解支持** 和 **Spring 基于代理的 AOP 实现**。因此，当你使用 `@Before("execution(...)")` 等 AspectJ 风格的切点表达式时，Spring AOP 会创建一个代理对象，在代理对象的生命周期内应用切面逻辑。

但 Spring AOP 本身不具备 AspectJ 提供的 **编译时增强** 或 **字节码增强** 功能。只有通过 **AspectJ** 提供的完整工具（比如 AspectJ 编译器）才能实现字节码级别的织入。

### 什么时候使用 `@EnableAspectJAutoProxy`？

- **如果你仅需要方法级别的切面**，Spring AOP 提供的基于代理的功能足以满足需求，这时你可以使用 `@EnableAspectJAutoProxy` 来启用 AOP 功能，同时使用 AspectJ 风格的注解（例如 `@Before`、`@Around` 等）来定义切面。
- **如果你需要更强大的 AOP 功能**，例如构造函数切入点、字段切入点、编译时织入等，Spring AOP 可能无法满足要求，这时你可以考虑使用 **纯粹的 AspectJ**（即通过 AspectJ 编译器进行字节码增强），而不是仅仅依赖 Spring AOP。

### 代码示例

1. **启用 Spring AOP 和 AspectJ 注解支持**

   通过 `@EnableAspectJAutoProxy` 启用 Spring AOP 与 AspectJ 集成：

   ```
   @Configuration
   @EnableAspectJAutoProxy  // 启用 AspectJ 注解支持
   public class AppConfig {
       // 配置类
   }
   ```

2. **创建切面类（使用 AspectJ 注解）**

   下面是一个带有 `@Before` 注解的切面类，这里使用的是 **AspectJ 风格的切点表达式**，并通过 Spring AOP 提供的代理机制来应用切面。

   ```
   @Aspect
   @Component  // Spring 容器管理的切面类
   public class LoggingAspect {
   
       @Before("execution(* com.example.service.UserService.addUser(..))")  // AspectJ 切点表达式
       public void logBefore(JoinPoint joinPoint) {
           String username = (String) joinPoint.getArgs()[0];
           System.out.println("Before executing addUser method, username: " + username);
       }
   }
   ```

3. **目标类（被切面代理的类）**

   ```
   @Service
   public class UserService {
   
       public void addUser(String username) {
           System.out.println("User " + username + " added successfully!");
       }
   }
   ```

### 总结

- **`@EnableAspectJAutoProxy`** 注解既支持 **AspectJ 注解**，也启用了 **Spring AOP** 的代理功能。它使得 Spring 可以支持 AspectJ 风格的切点表达式，并创建代理来应用切面逻辑。
- **Spring AOP** 是基于代理模式的，适用于简单的 AOP 场景，而 **AspectJ** 提供了更强大的功能，适合更复杂的需求。通过 `@EnableAspectJAutoProxy`，Spring AOP 能够集成 **AspectJ** 的切点表达式和注解支持。

因此，`@EnableAspectJAutoProxy` 并不仅仅是启用 **AspectJ**，而是启用了 **Spring AOP 与 AspectJ** 的集成，使得 Spring 可以使用 AspectJ 风格的切点表达式，并通过代理方式实现 AOP。



#### 一种使用aspectj 通过配置(注解或者xml)通过实现 BeanPostProcesser -自动织入

#### 一种 spring aop基于接口使用 ProxyFactoryBean -手动创建

https://docs.spring.io/spring-framework/reference/core/aop-api/advice.html

- **Aspect 切面**：定义一个类，包含增强代码和切点匹配规则
- **Pointcut 切点**：指定哪些方法增强，常用 execution表达式 来匹配
- **Advice 通知**：增强的具体实现，指明在什么时机执行增强逻辑（前、后、环绕等）**@Before**、**@After**、**@Around**  @ AfterReturning ， @异常
- **Weaving 织入**：将增强应用到目标对象的过程，Spring AOP 是在运行时进行织入的。

```java
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Component;

@Aspect  // 声明该类是一个切面
@Component  // 将切面类加入Spring容器管理
public class LoggingAspect {

    // 定义切点：匹配UserService类中的所有方法
    @Pointcut("execution(* com.example.service.UserService.*(..))")  // execution表达式，匹配UserService类的所有方法
    public void userServiceMethods() {}

    // 在addUser方法执行之前执行的增强（前置通知）
    @Before("userServiceMethods()")  // 引用切点方法，执行前
    public void logBefore() {
        System.out.println("Before method execution: Logging before user service method execution.");
    }

    // 在updateUser方法执行之后执行的增强（后置通知）
    @After("execution(* com.example.service.UserService.updateUser(..))")  // 匹配updateUser方法
    public void logAfter() {
        System.out.println("After method execution: Logging after update user method execution.");
    }
}
```

```java
import org.springframework.stereotype.Service;
@Service
public class UserService {
    public void addUser(String username) {
        System.out.println("Adding user: " + username);
    }
    public void deleteUser(String username) {
        System.out.println("Deleting user: " + username);
    }
    public void updateUser(String username) {
        System.out.println("Updating user: " + username);
    }
}
```

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
@SpringBootApplication
public class Application implements CommandLineRunner {
    @Autowired
    private UserService userService;

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Override
    public void run(String... args) throws Exception {
        // 调用 UserService 的方法
        userService.addUser("JohnDoe");
        userService.updateUser("JohnDoe");
        userService.deleteUser("JohnDoe");
    }
}
```

#### Sping 如何对 bean实现aop 的 **BeanPostProcessor**

Spring AOP 使用了 `InstantiationAwareBeanPostProcessor` 的子接口（通常是 `AnnotationAwareAspectJAutoProxyCreator`）来创建代理对象，增强目标对象。

###  Spring Aop- ProxyFactoryBean 手动创建

```java
import org.springframework.aop.MethodBeforeAdvice;
import org.springframework.aop.framework.NameMatchMethodPointcut;
import org.springframework.aop.Pointcut;
import org.springframework.aop.support.DefaultPointcutAdvisor;

import java.lang.reflect.Method;

public class MyAspect {

    // 定义前置通知（MethodBeforeAdvice）
    public static class BeforeMethodAdvice implements MethodBeforeAdvice {
        @Override
        public void before(Method method, Object[] args, Object target) throws Throwable {
            System.out.println("Before executing method: " + method.getName());
        }
    }

    // 创建切点
    public static class MyPointcut extends NameMatchMethodPointcut {
        public MyPointcut() {
            // 这里我们定义了匹配 "doSomething" 和 "doSomethingElse" 方法
            this.addMethodName("doSomething");
            this.addMethodName("doSomethingElse");
        }
    }

    // 创建 Advisor，组合切点和通知
    public static class MyAdvisor extends DefaultPointcutAdvisor {
        public MyAdvisor() {
            // 创建切点
            Pointcut pointcut = new MyPointcut();
            // 创建通知
            MethodBeforeAdvice advice = new BeforeMethodAdvice();
            // 创建 Advisor，将切点和通知组合在一起
            this.setPointcut(pointcut);
            this.setAdvice(advice);
        }
    }
}
```

```java
import org.springframework.aop.framework.ProxyFactoryBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {

    @Bean
    public MyService myService() {
        return new MyServiceImpl();  // 目标类
    }

    @Bean
    public MyAspect.MyAdvisor myAdvisor() {
        return new MyAspect.MyAdvisor();  // 切面配置（Advisor）
    }

    @Bean
    public ProxyFactoryBean proxyFactoryBean() {
        ProxyFactoryBean factoryBean = new ProxyFactoryBean();
        factoryBean.setTarget(myService());  // 设置目标对象
        factoryBean.addAdvisor(myAdvisor());  // 设置 Advisor
        return factoryBean;
    }
}

```

```java
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args) {
        // 初始化 Spring 容器
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

        // 获取代理对象
        MyService myService = (MyService) context.getBean("proxyFactoryBean");

        // 调用目标方法
        myService.doSomething();         // 将触发前置通知
        myService.doSomethingElse();     // 将触发前置通知
        myService.doAnotherThing();      // 不会触发前置通知

        // 关闭容器
        context.close();
    }
}

```



### 代理

1. jdk  动态代理- 反射？ 什么时候生成的 .class 代码的？

   1. 运行时生成一个 ¥proxy*.class， 底层使用classloader 来加载，并且反射生成实列
   2. 必须有接口
   3. 反射调用 目标方法

2. cglib 动态代理- 字节码

   1. 没有接口，继承父类，重写父类

   2. 子类函数中调用父类的目标函数 -直接调用

   3. 生产的代理类 ： * $ Bycglib* 

      ```shell
      # 使用 jvm 参数  ，把当前的 cglib 相关的生成的，生成到本地
      -Dcglib.debugLocation=/user/target/class
      ```

      

   4. ==final== 不能标记



# 3，Spring 三级缓存

在Spring框架中，三级缓存机制主要用于解决循环依赖问题，尤其是在单例Bean的创建过程中。Spring的三级缓存分别是：

1. **一级缓存（Singleton Objects）**：
   - 存储已经完全初始化好的单例Bean。
   - 当Bean完成实例化、属性注入和初始化后，会被放入一级缓存。
   - 通过`DefaultSingletonBeanRegistry`的`singletonObjects`属性实现。
2. **二级缓存（Early Singleton Objects）**：
   - 存储已经实例化但尚未完成属性注入和初始化的Bean。
   - 用于解决循环依赖问题，允许其他Bean引用这些未完全初始化的Bean。
   - 通过`DefaultSingletonBeanRegistry`的`earlySingletonObjects`属性实现。
3. **三级缓存（Singleton Factories）**：
   - 存储Bean的工厂对象（`ObjectFactory`），用于生成Bean的早期引用。
   - 当Bean实例化后，Spring会将其工厂对象放入三级缓存，以便在需要时生成早期引用。
   - 通过`DefaultSingletonBeanRegistry`的`singletonFactories`属性实现。

### 解决循环依赖的流程

1. **实例化Bean**：
   - Spring首先实例化Bean，但此时Bean还未进行属性注入和初始化。
2. **放入三级缓存**：
   - 实例化后，Spring将Bean的工厂对象放入三级缓存。
3. **属性注入**：
   - Spring开始注入Bean的属性。如果发现依赖的Bean还未创建，Spring会先创建依赖的Bean。
4. **处理循环依赖**：
   - 如果依赖的Bean正在创建中（即已经在三级缓存中），Spring会通过三级缓存中的工厂对象获取该Bean的早期引用，并将其放入二级缓存。
5. **完成初始化**：
   - 当Bean完成属性注入和初始化后，Spring会将其从二级缓存移动到一级缓存。
6. **清理缓存**：
   - 最后，Spring会清理二级和三级缓存中与该Bean相关的条目。

### 示例

假设有两个Bean，A和B，它们相互依赖：

```
@Component
public class A {
    @Autowired
    private B b;
}

@Component
public class B {
    @Autowired
    private A a;
}
```

1. Spring开始创建Bean A，实例化A后将其工厂对象放入三级缓存。
2. Spring尝试注入A的属性B，发现B还未创建，于是开始创建B。
3. Spring实例化B后将其工厂对象放入三级缓存。
4. Spring尝试注入B的属性A，发现A正在创建中，于是通过三级缓存获取A的早期引用，并将其放入二级缓存。
5. Spring完成B的属性注入和初始化，将B放入一级缓存。
6. Spring回到A的创建过程，完成A的属性注入和初始化，将A放入一级缓存。
7. Spring清理二级和三级缓存中与A和B相关的条目。

通过三级缓存机制，Spring能够有效地解决单例Bean之间的循环依赖问题。



==？？==工厂对象的作用是延迟生成Bean的早期引用





## **AnnotationConfigApplicationContext**



是Spring框架的核心，负责管理对象的创建和依赖注入。不过用户可能已经知道这些，所以可能需要更深入的过程描述。

接下来，我需要回忆Spring IOC容器的工作流程。通常这个过程包括配置元数据、实例化容器、Bean的实例化、依赖注入、初始化以及销毁等步骤。用户可能希望了解每个步骤的具体细节，比如配置元数据的方式有哪些，容器如何读取这些配置，Bean的生命周期等。



# 说说spring ioc 过程

> 1. 加载配置：解析XML、注解或Java配置，生成BeanDefinition。 
> 2. Bean的实例化：通过反射或工厂方法创建Bean实例。 
> 3. 依赖注入：设置属性和依赖的其他Bean。
> 4.  处理Aware接口：注入容器相关的信息。 
> 5. BeanPostProcessor的前后处理：自定义处理Bean。
> 6.  初始化方法：执行@PostConstruct和init-method。
> 7.  使用Bean：应用运行时使用Bean。
> 8.  销毁阶段：容器关闭时执行销毁方法。

Spring IoC（控制反转）是 Spring 框架的核心机制，其核心思想是将对象的创建、依赖管理和生命周期交由 Spring 容器管理。以下是 Spring IoC 的主要过程：

### **1. 加载配置（Configuration Loading）**

Spring 容器启动时，会读取配置元数据（XML、Java 注解或 Java Config），定义 Bean 的元信息，例如：

- **XML 配置**：通过 `<bean>` 标签定义 Bean。
- **注解**：如 `@Component`、`@Service` 等，结合 `@ComponentScan` 扫描。
- **Java Config**：使用 `@Configuration` 和 `@Bean` 定义。

**启动时机**：配置加载在容器初始化时触发（如 `ApplicationContext` 构造函数或 `refresh()` 方法调用）。

- 核心类：
  - `Resource` 和 `ResourceLoader`：定位配置文件。
  - `BeanDefinitionReader`：解析配置为 `BeanDefinition`。
  - `BeanDefinitionRegistry`：注册和管理 `BeanDefinition`。
  - `ApplicationContext`：封装容器的高级操作。
- **流程**：定位资源 → 解析配置 → 注册 BeanDefinition → 实例化 Bean。

------

### **2. 解析配置生成 BeanDefinition**

容器将配置元数据解析为 **`BeanDefinition`** 对象，存储以下信息：

- Bean 的类名（Class）

- 作用域（Singleton/Prototype）

- 初始化/销毁方法（`init-method`/`destroy-method`）

- 依赖关系（属性或构造器参数）。

  ```
  核心类：
  XmlBeanDefinitionReader：解析 XML 配置。
  AnnotatedBeanDefinitionReader 和 ClassPathBeanDefinitionScanner：处理注解配置。
  ConfigurationClassParser：解析 Java Config 类。
  协作流程：
  配置资源 → BeanDefinitionReader → BeanDefinitionDocumentReader/ConfigurationClassParser → BeanDefinition → 注册到 BeanDefinitionRegistry。
  设计模式：
  模板方法模式（如 AbstractBeanDefinitionReader）、委托模式（如 BeanDefinitionParserDelegate）
  ```

------

### **3. Bean 的实例化（Instantiation）**

通过反射或工厂方法创建 Bean 的实例：

- **默认构造器**：`Class.newInstance()`。
- **静态工厂方法**：指定 `factory-method`。
- **实例工厂方法**：通过另一个 Bean 的工厂方法创建。

------

### **4. 依赖注入（Dependency Injection）**

容器根据配置为 Bean 注入依赖：

- **属性注入**：通过 Setter 方法（XML 的 `<property>` 或 `@Autowired`）。
- **构造器注入**：通过构造方法（XML 的 `<constructor-arg>` 或 `@Autowired`）。
- **自动装配**：按类型（`byType`）或名称（`byName`）自动匹配依赖。

------

### **5. 处理 Aware 接口**

若 Bean 实现了某些 `Aware` 接口，容器会注入相关资源：

- `BeanNameAware`：设置 Bean 的 ID。
- `BeanFactoryAware`：注入 BeanFactory。
- `ApplicationContextAware`：注入 ApplicationContext。

------

### **6. BeanPostProcessor 处理**

通过 `BeanPostProcessor` 在 Bean 初始化前后执行自定义逻辑：

- `postProcessBeforeInitialization`：在初始化前调用（如 `@PostConstruct`）。
- `postProcessAfterInitialization`：在初始化后调用（如 AOP 代理）。

------

### **7. 初始化（Initialization）**

执行 Bean 的初始化方法：

- `@PostConstruct` 注解的方法。
- XML 中配置的 `init-method`。
- `InitializingBean` 接口的 `afterPropertiesSet()`。

------

### **8. Bean 就绪**

此时 Bean 已完全初始化，可被应用程序使用。

------

### **9. 销毁（Destruction）**

容器关闭时销毁 Bean：

- `@PreDestroy` 注解的方法。
- XML 中配置的 `destroy-method`。
- `DisposableBean` 接口的 `destroy()`。

------

### **关键扩展点**

- **BeanFactoryPostProcessor**：修改 BeanDefinition（如 `PropertySourcesPlaceholderConfigurer` 处理占位符）。
- **BeanPostProcessor**：干预 Bean 的初始化过程（如 AOP、事务代理）。
- **生命周期接口**：`InitializingBean`、`DisposableBean`。





# 2，Spring Bean的生命周期是什么？从实例化到销毁的过程如何？

主要的阶段：

1. 实例化（Instantiation）
2.  属性赋值（Population）
3.  初始化（Initialization） 
4.  使用（In use）
5. 销毁（Destruction）



### **1. 实例化阶段（Instantiation）**

- **Bean的创建**：通过构造函数或工厂方法实例化Bean。
- 扩展点：
  - **`InstantiationAwareBeanPostProcessor.postProcessBeforeInstantiation`**：在实例化前执行，可返回代理对象替代默认实例化。
  - **`InstantiationAwareBeanPostProcessor.postProcessAfterInstantiation`**：实例化后、属性注入前执行，可干预属性填充。

------

### **2. 属性赋值阶段（Population）**

- **依赖注入**：通过Setter、构造器或自动装配注入属性值及依赖的Bean。
- 扩展点：
  - **`InstantiationAwareBeanPostProcessor.postProcessProperties`**：处理属性注入（如`@Autowired`）。

------

### **3. Aware接口回调**

- 设置容器信息

  ：若Bean实现了*Aware接口，Spring会注入相关对象。

  - **`BeanNameAware`**：设置Bean的名称。
  - **`BeanFactoryAware`**：注入BeanFactory实例。
  - **`ApplicationContextAware`**（仅在ApplicationContext中生效）：注入ApplicationContext、Environment等。

------

### **4. 初始化阶段（Initialization）**

- **`BeanPostProcessor.postProcessBeforeInitialization`**：初始化前的处理（如`@PostConstruct`注解的方法）。
- **`InitializingBean.afterPropertiesSet`**：属性赋值完成后调用。
- **自定义初始化方法**：通过`@Bean(initMethod="...")`或XML的`init-method`指定。
- **`BeanPostProcessor.postProcessAfterInitialization`**：初始化后的处理（如生成AOP代理）。

------

### **5. 使用阶段（In Use）**

- Bean完全初始化后，可被应用程序正常使用。

------

### **6. 销毁阶段（Destruction）**

- **`DisposableBean.destroy`**：容器关闭时调用。
- **自定义销毁方法**：通过`@Bean(destroyMethod="...")`或XML的`destroy-method`指定。

------

### **完整流程图**

```
1. InstantiationAwareBeanPostProcessor.postProcessBeforeInstantiation
2. Bean实例化（构造方法/工厂）
3. InstantiationAwareBeanPostProcessor.postProcessAfterInstantiation
4. 属性注入（依赖注入）
5. Aware接口回调（BeanNameAware, BeanFactoryAware等）
6. BeanPostProcessor.postProcessBeforeInitialization
7. InitializingBean.afterPropertiesSet()
8. 自定义init方法
9. BeanPostProcessor.postProcessAfterInitialization
10. Bean就绪，可被使用
11. DisposableBean.destroy()
12. 自定义destroy方法
```

------

### **关键注意事项**

- **作用域影响**：Singleton Bean的完整生命周期由容器管理，而Prototype Bean的销毁需手动触发。
- **扩展点作用**：`BeanPostProcessor`和`InstantiationAwareBeanPostProcessor`提供高度定制能力（如AOP、属性校验）。
- **ApplicationContext与BeanFactory**：ApplicationContext自动注册`BeanPostProcessor`等组件，BeanFactory需手动配置。

理解这些步骤有助于在Spring中更好地控制Bean的创建、初始化和销毁过程，并利用扩展点实现自定义逻辑。





Spring的AOP（Aspect-Oriented Programming，面向切面编程）是Spring框架的核心模块之一，用于**解耦横切关注点**（如日志、事务、安全等），通过动态代理技术在不修改原有代码的基础上增强功能。

------

### **核心概念**

1. **切面（Aspect）**
   封装横切逻辑的模块（如日志工具类），包含**通知（Advice）\**和\**切点（Pointcut）**。
2. **连接点（Join Point）**
   程序执行过程中的某个点（如方法调用、异常抛出），Spring AOP仅支持方法级别的连接点。
3. **通知（Advice）**
   在特定连接点执行的动作，分为：
   - `@Before`：方法执行前
   - `@AfterReturning`：方法成功返回后
   - `@AfterThrowing`：方法抛出异常后
   - `@After`（最终通知）：无论方法是否成功
   - `@Around`：包裹方法，可控制方法执行流程
4. **切点（Pointcut）**
   通过表达式（如`execution(* com.example.service.*.*(..))`）定义哪些连接点需要被拦截。
5. **织入（Weaving）**
   将切面代码插入目标对象的过程。Spring AOP通过**动态代理**实现（JDK动态代理或CGLIB）。

------

### **实现原理**

- 动态代理

  ：

  - 若目标对象实现接口，使用**JDK动态代理**生成代理类。
  - 若无接口，使用**CGLIB**生成子类代理。

- **代理对象**：
  在方法调用时，代理对象会触发关联的Advice逻辑。

------

### **示例代码**

```
JAVA



@Aspect
@Component
public class LoggingAspect {

    // 定义切点：拦截service包下所有方法
    @Pointcut("execution(* com.example.service.*.*(..))")
    private void serviceMethods() {}

    // 前置通知
    @Before("serviceMethods()")
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("调用方法: " + joinPoint.getSignature().getName());
    }

    // 环绕通知（控制方法执行）
    @Around("serviceMethods()")
    public Object measureTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object result = joinPoint.proceed(); // 执行原方法
        long end = System.currentTimeMillis();
        System.out.println("方法执行耗时: " + (end - start) + "ms");
        return result;
    }
}
```

------

### **与AspectJ的区别**

|   特性   |       Spring AOP       |            AspectJ            |
| :------: | :--------------------: | :---------------------------: |
| 实现方式 |        动态代理        |      编译时/类加载时织入      |
|   性能   |     运行时开销略高     |    更高性能（编译时优化）     |
| 功能范围 |   仅支持方法级别拦截   |   支持字段、构造器、静态块    |
|   依赖   |   轻量，内置于Spring   | 需额外依赖（如AspectJ编译器） |
| 适用场景 | 简单横切逻辑（如日志） |   复杂场景（如系统级监控）    |

------

### **适用场景**

- 统一日志记录
- 声明式事务管理（`@Transactional`）
- 权限校验（如拦截未登录请求）
- 性能监控（统计方法耗时）
- 异常统一处理

------

### **配置方式**

1. **注解驱动**（推荐）
   使用`@EnableAspectJAutoProxy`启用自动代理：

   ```java
   @Configuration
   @EnableAspectJAutoProxy
   public class AppConfig {}
   ```

2. **XML配置**

   ```
   <aop:aspectj-autoproxy/>
   ```

------

### **局限性**

- 仅作用于Spring管理的Bean。
- 无法拦截私有方法或非Spring对象的方法。
- 对代码侵入性低，但功能不如AspectJ全面。

通过AOP，Spring实现了业务逻辑与横切关注点的解耦，提升了代码的可维护性和复用性。



# 2.3，@EnableAspectJAutoProxy

Spring 的 `@EnableAspectJAutoProxy` 注解名称中包含 "AspectJ" 的原因与其设计背景和实现方式密切相关。以下是关键点解释：

------

### **1. 为什么是 "AspectJ" 而不是 "AOP"？**

- **历史兼容性**：
  Spring AOP 在设计时**借用了 AspectJ 的注解和切点表达式语法**（如 `@Aspect`、`@Pointcut`、`execution(...)`），但底层实现机制不同。
  - 使用 "AspectJ" 是为了表明**语法兼容性**，而非直接依赖 AspectJ 的完整实现。
- **明确功能范围**：
  `@EnableAspectJAutoProxy` 的 "AutoProxy" 表明它是通过**动态代理**实现 AOP，而不是完整的 AspectJ 织入（如编译时/加载时织入）。
  - 如果命名为 `@EnableAOP`，可能无法清晰表达其基于代理的轻量级特性。

------

### **2. 为什么需要 "AspectJ" 的参与？**

- **语法标准化**：
  AspectJ 是 Java 领域**最成熟的 AOP 框架**，其切点表达式语法（如 `execution`、`within`）已成为行业标准。
  - Spring AOP **复用 AspectJ 的语法**，降低学习成本，同时保持与 AspectJ 生态的兼容性。
- **注解驱动设计**：
  Spring 使用 AspectJ 的注解（如 `@Aspect`、`@Pointcut`）来定义切面和切点，但这些注解仅作为**元数据标记**，实际功能由 Spring AOP 实现。

------

### **3. 与完整 AspectJ 的区别**

|   **特性**   |          **Spring AOP**          |             **AspectJ**             |
| :----------: | :------------------------------: | :---------------------------------: |
| **实现方式** |      动态代理（JDK/CGLIB）       | 编译时/加载时织入（直接修改字节码） |
| **语法支持** |    仅支持方法级别的切点表达式    | 支持方法、字段、构造器等所有连接点  |
|   **性能**   |     运行时代理有轻微性能开销     |    编译时优化，无运行时代理开销     |
|   **依赖**   |           无需额外配置           | 需 AspectJ 编译器或织入器（如 LTW） |
| **注解用途** | 仅标记元数据，功能由 Spring 实现 |     直接驱动 AspectJ 的完整功能     |

------

### **4. 为何不直接叫 `@EnableAOP`？**

- **避免歧义**：
  "AOP" 是通用概念，而 Spring 的 AOP 实现是**基于代理的轻量级方案**。命名中包含 "AspectJ" 可以明确其语法来源，同时与完整 AspectJ 方案区分。
- **功能限定**：
  `@EnableAspectJAutoProxy` 的 "AutoProxy" 强调其核心机制是**自动创建代理对象**（而非字节码织入），更精准地描述其技术本质。

------

### **总结**

- **名称含义**：
  `@EnableAspectJAutoProxy` = **启用基于 AspectJ 语法**的**自动代理机制**（Spring AOP）。
- **设计意图**：
  Spring 通过此注解提供一种轻量级、易用的 AOP 实现，同时复用 AspectJ 的成熟语法，降低开发者的学习成本。
  若需更强大的 AOP 功能（如拦截字段、构造器），仍需切换到完整的 AspectJ 实现。



### **6. BeanPostProcessor 扩展**

Spring 提供扩展点，允许在 Bean 初始化前后插入逻辑：

- **`postProcessBeforeInitialization()`**：在 Bean 初始化前执行（如 `@PostConstruct` 处理）。
- **`postProcessAfterInitialization()`**：在 Bean 初始化后执行（如 AOP 代理的生成）。

