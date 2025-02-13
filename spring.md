

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
java


复制代码
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
java


复制代码
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
java


复制代码
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
java


复制代码
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
java


复制代码
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
java


复制代码
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
java


复制代码
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

AOP又名Aspect Oriented Programming 意为 ‘[面向切面编程](https://so.csdn.net/so/search?q=面向切面编程&spm=1001.2101.3001.7020)’通过预编译和运行期间动态代理来实现程序功能增强

#### 他有哪些应用场景

1. 缓存，权限，日志，事务，方法的功能增强；

#### spring 中的aop 

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



### Spring 三级缓存



**AnnotationConfigApplicationContext**



是Spring框架的核心，负责管理对象的创建和依赖注入。不过用户可能已经知道这些，所以可能需要更深入的过程描述。

接下来，我需要回忆Spring IOC容器的工作流程。通常这个过程包括配置元数据、实例化容器、Bean的实例化、依赖注入、初始化以及销毁等步骤。用户可能希望了解每个步骤的具体细节，比如配置元数据的方式有哪些，容器如何读取这些配置，Bean的生命周期等。



### 说说spring ioc 过程



### **6. BeanPostProcessor 扩展**

Spring 提供扩展点，允许在 Bean 初始化前后插入逻辑：

- **`postProcessBeforeInitialization()`**：在 Bean 初始化前执行（如 `@PostConstruct` 处理）。
- **`postProcessAfterInitialization()`**：在 Bean 初始化后执行（如 AOP 代理的生成）。

