## 面试题

-  什么是 Spring Boot？包含哪些核心组件？
-  Spring Boot 的优势有哪些？
-  spring boot启动过程都发生了什么？
-  Spring Boot 的自动配置
-  什么是 Spring Boot Starter？
-  Spring Boot 的默认配置是什么？
-  Spring Boot 中的 Actuator 是什么？
-  如何自定义一个 Spring Boot Starter



## 1. 什么是 Spring Boot？包含哪些核心组件？

## 2. Spring Boot 的优势有哪些？



## 3. spring boot启动过程都发生了什么？

## 4. Spring Boot 的自动配置



## 5. Spring Boot 的默认配置是什么？



## 6. Spring Boot 中的 Actuator 是什么？



## 7. 什么是 Spring Boot Starter？

## 8. 如何自定义一个 Spring Boot Starter







## 读取jar 包中指定的文件内容

> 方式很多

```java
import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.JarURLConnection;
import java.net.URL;

public class ReadJarFileExample {

    private static void readContentFromUrl(URL url) throws Exception {
        // 判断是否为 JAR 协议（如 "jar:file:/path/to/jar!/internal/file.txt"）
        if ("jar".equals(url.getProtocol())) {
            // 打开 JAR 连接
            JarURLConnection jarConnection = (JarURLConnection) url.openConnection();
            // 获取 JAR 包内的文件输入流
            try (InputStream inputStream = jarConnection.getInputStream();
                 BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream))) {
                String line;
                System.out.println("Content of " + url + ":");
                while ((line = reader.readLine()) != null) {
                    System.out.println(line);
                }
            }
        } else {
            // 处理普通文件（非 JAR 包内）
            try (InputStream inputStream = url.openStream();
                 BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream))) {
                // 读取内容...
            }
        }
    }
}
```





## 一，==重点==Spring Boot 的自动配置

Spring Boot 的自动配置（Auto-Configuration）是其核心特性之一，它通过条件化配置（Conditional Configuration）和约定优于配置（Convention over Configuration）的原则，减少开发者的手动配置工作。其自动装配原理可以分为以下几个关键步骤：

------

### **1. 自动配置的触发机制**

- **`@EnableAutoConfiguration` 注解**：
  Spring Boot 应用的主类通常标注了 `@SpringBootApplication`，该注解组合了 `@EnableAutoConfiguration`，触发自动配置逻辑。
- **`AutoConfigurationImportSelector` 类**：
  `@EnableAutoConfiguration` 通过 `@Import` 导入 `AutoConfigurationImportSelector`，该类负责扫描并加载所有符合条件的自动配置类。

------

### **2. 加载自动配置类**

- **`META-INF/spring.factories` 文件**：
  Spring Boot 在 `spring-boot-autoconfigure` 模块中提供了 `META-INF/spring.factories` 文件，其中定义了所有内置的自动配置类（如 `DataSourceAutoConfiguration`、`WebMvcAutoConfiguration` 等）。
- **自动配置类的筛选**：
  `AutoConfigurationImportSelector` 会加载这些配置类，但最终是否生效取决于**条件注解**（Conditional Annotations）的评估结果。

------

### **3. 条件化配置（Conditional Configuration）**

自动配置类使用 `@Conditional` 系列注解，根据当前环境动态决定是否生效。常见的条件注解包括：

- `@ConditionalOnClass`：类路径中存在指定类时生效。
- `@ConditionalOnMissingBean`：容器中不存在指定 Bean 时生效。
- `@ConditionalOnProperty`：配置文件中存在指定属性时生效。
- `@ConditionalOnWebApplication`：当前应用是 Web 应用时生效。

**示例**：`DataSourceAutoConfiguration` 仅在类路径下存在 `DataSource` 类时才会生效，且会检查是否已手动配置了 `DataSource` Bean。

------

### **4. 自动配置类的执行**

每个自动配置类（标注 `@Configuration`）会定义多个 `@Bean` 方法，但只有在**所有条件满足**时，这些 Bean 才会被创建并注册到 Spring 容器中。

**示例**：`WebMvcAutoConfiguration` 会在检测到 `Spring MVC` 相关类存在时，自动配置 `ViewResolver`、`HandlerMapping` 等组件。

------

### **5. 配置属性的绑定**

自动配置通常与 `application.properties` 或 `application.yml` 中的属性绑定：

- **`@ConfigurationProperties` 注解**：将外部配置（如 `spring.datasource.url`）绑定到 Java 对象。
- **属性优先级**：开发者手动配置的属性会覆盖自动配置的默认值。

------

### **6. 自动装配（Auto-Wiring）**

自动配置生成的 Bean 会通过 Spring 的依赖注入（DI）机制被自动装配到其他组件中：

- **`@Autowired`**：按类型注入。
- **`@Resource`**：按名称注入。
- **构造器注入**：Spring 4.3+ 支持隐式的构造器注入。

------

### **核心流程图**

```plaintext
启动应用 → 加载 @EnableAutoConfiguration → 扫描 spring.factories → 筛选自动配置类 → 评估条件注解 → 注册符合条件的 Bean → 自动装配到需要的组件中。
```

------

### **调试自动配置**

- **`--debug` 启动参数**：
  通过 `java -jar yourapp.jar --debug` 可以查看自动配置报告，显示哪些配置类生效或被排除。
- **`ConditionEvaluationReport`**：
  在日志中查看条件评估的详细信息。

------

### **自定义自动配置**

开发者可以创建自己的自动配置类：

1. 编写 `@Configuration` 类并添加条件注解。

2. 在 `META-INF/spring.factories` 中注册：

   ```
   org.springframework.boot.autoconfigure.EnableAutoConfiguration=com.example.MyAutoConfiguration
   ```

------

### **总结**

Spring Boot 的自动配置通过以下机制实现：

1. **条件化加载**：基于类路径、环境变量等动态决定配置类是否生效。
2. **约定优于配置**：提供默认实现，允许开发者通过属性文件覆盖。
3. **SPI 机制**：通过 `spring.factories` 发现和加载配置类。

这种设计大幅减少了样板代码，同时保留了高度的灵活性。

> SPI全称是Service Provider Interface，这是Java的一种服务发现机制。在Java中，通常是通过在META-INF/services目录下放置配置文件来实现的，但用户提到的是Spring框架中的实现





## Spring Boot 的自动装配流程

Spring Boot 的自动装配（Auto-configuration）是其核心特性之一，它能够根据项目中引入的依赖和配置，自动配置 Spring 应用。以下是 Spring Boot 自动装配的核心流程：

### 1. **启动类标注 `@SpringBootApplication`**

通常，Spring Boot 应用的入口类会使用 `@SpringBootApplication` 注解。这个注解是一个组合注解，包含了以下几个重要的注解：

- `@Configuration`：将类标记为配置类，允许在其中定义 Bean。
- `@EnableAutoConfiguration`：启用自动装配功能。
- `@ComponentScan`：启用组件扫描，默认扫描启动类所在包及其子包中的组件。

### 2. **`@EnableAutoConfiguration` 触发自动装配**

当 Spring Boot 应用启动时，`@EnableAutoConfiguration` 注解会触发自动装配机制。具体流程如下：

#### a. **收集候选配置类**

Spring Boot 会扫描 `META-INF/spring.factories` 文件，该文件位于所有引入的 JAR 包中。在这个文件中，列出了所有可用的自动配置类。例如：

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.example.AutoConfigurationA,\
com.example.AutoConfigurationB
```

#### b. **条件化筛选配置类**

Spring Boot 使用条件注解（如 `@ConditionalOnClass`、`@ConditionalOnMissingBean` 等）来决定是否应用某个自动配置类。条件注解用于检查当前环境是否满足特定条件，例如某个类是否存在、某个 Bean 是否已经定义等。

例如，`DataSourceAutoConfiguration` 只有在类路径中存在 `DataSource` 相关的类时才会被激活。

#### c. **应用符合条件的配置类**

满足条件的自动配置类会被加载并应用，这些配置类通常会定义和配置相关的 Bean。例如，`DataSourceAutoConfiguration` 可能会配置一个默认的数据源。

### 3. **用户自定义配置优先**

Spring Boot 的自动装配机制设计为“约定大于配置”，但同时也允许用户通过自定义配置覆盖自动装配的默认设置。用户可以在 `application.properties` 或 `application.yml` 文件中设置相关属性，或者在配置类中使用 `@Bean` 注解定义自己的 Bean。自定义配置会优先于自动装配的配置生效。

### 4. **自动装配的执行顺序**

自动装配的执行顺序如下：

1. **加载所有候选的自动配置类**：通过 `spring.factories` 收集。
2. **条件评估**：根据条件注解筛选出适用的配置类。
3. **应用配置类**：将筛选出的配置类应用到 Spring 容器中，注册相应的 Bean。
4. **用户配置覆盖**：如果有用户自定义配置，覆盖自动装配的默认配置。

### 5. **禁用特定的自动装配**

如果需要禁用某些自动装配，可以通过以下方式：

- **使用 `@EnableAutoConfiguration` 的 `exclude` 属性**：

  ```java
  @SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})
  public class MyApplication {
      public static void main(String[] args) {
          SpringApplication.run(MyApplication.class, args);
      }
  }
  ```

- **在 `application.properties` 中设置**：

  ```properties
  spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
  ```

### 6. **自定义自动装配**

除了使用 Spring Boot 提供的自动装配功能，开发者也可以创建自己的自动装配逻辑：

1. **创建配置类**，使用 `@Configuration` 注解。
2. **使用条件注解**，根据需要定义条件。
3. **在 `META-INF/spring.factories` 中注册自定义的自动配置类**。

例如，在 `spring.factories` 中添加：

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.example.MyCustomAutoConfiguration
```

### 总结

Spring Boot 的自动装配通过以下核心流程实现：

1. **启动类标注 `@SpringBootApplication`，启用自动装配。**
2. **扫描 `META-INF/spring.factories`，收集候选自动配置类。**
3. **基于条件注解筛选并应用适用的配置类。**
4. **允许用户通过配置文件或自定义配置覆盖默认设置。**
5. **提供机制禁用特定的自动装配。**
6. **支持创建和注册自定义的自动装配逻辑。**

这种机制极大地简化了 Spring 应用的配置过程，提高了开发效率，同时保持了足够的灵活性以满足不同的需求。