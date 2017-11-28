
## spring boot常用指令

在项目文件夹下：


mvn -Pnexus dependency:tree

mvn -Pnexus package -DskipTests：打包

mvn -Pnexus spring-boot:run：运行springboot

java -jar target\auconfig-0.0.1-SNAPSHOT.jar

java -jar target\auconfig-0.0.1-SNAPSHOT.jar --spring.profiles.active=prod：激活application-pro.properties这个配置文件

java -jar target\auconfig-0.0.1-SNAPSHOT.jar --app.count=101 --logging.level.root=WARN（DEBUG）:日志级别改成WARN


## spring boot 读书笔记（有点乱）

### 项目的包结构
```
com
  +- example
    +- myproject
      +- Application.java
      |
      +- domain
      | +- Customer.java
      | +- CustomerRepository.java
      |
      +- service
      | +- CustomerService.java
      |
      +- web
        +- CustomerController.java


```

spring boot 只需要非常少的spring的配置就能运行，既可以以IDE的形式运行，也可以在命令行中运行。

Spring Boot集成了Tomcat、Jetty等Servlet 3.0+的Web容器。

spring-boot-starter-parent提供了所需的依赖，并且可以省略依赖的版本。

spring-boot-starter-web可以用来生成web程序，自带tomcat等。


如果想要运行一些特殊的代码，可以实现ApplicationRunner 或者 CommandLineRunner接口。这两者都会提供一个run方法，在SpringApplication.run(…)之前被调用。代码如下所示：

```

import org.springframework.boot.*
import org.springframework.stereotype.*
@Component
public class MyBean implements CommandLineRunner {
Spring Boot Reference Guide
1.5.8.RELEASE Spring Boot 57
public void run(String... args) {
// Do something...
  }
}

```

@SpringBootApplication注解代替了 @ComponentScan和 @Configuration，@EnableAutoConfiguration注解，更加的简便。@SpringBootApplication还提供了别名，可以自定义@EnableAutoConfiguration and @ComponentScan的属性

```
@SpringBootApplication
public class ClassApplication {

	@Autowired
	private ClassService service;

	public static void main(String[] args) {
		SpringApplication.run(ClassApplication.class, args);
	}

	@Bean
	public ClassService service() {
		service.start();
		return service;
	}
}
```
spring boot 为springmvc提供了自动配置，自动配置包含以下特性：

-包含了ContentNegotiatingViewResolver and BeanNameViewResolver beans.

-对服务静态资源的支持，包含WebJars

- 为Converter, GenericConverter, Formatter beans的自动注册

- 对HttpMessageConverters的支持

- 自动注册为MessageCodesResolver

- 静态index.html的支持

- 自定义图标的支持

- 自动使用ConfigurableWebBindingInitializer bean的支持



### Spring Boot使用非常特别的PropertySource命令，旨在允许合理地覆盖值。属性按以下顺序选择：

- 在您的HOME目录设置的Devtools全局属性（~/.spring-boot-devtools.properties）。

- 单元测试中的 @TestPropertySource 注解。


- 单元测试中的 @SpringBootTest#properties 注解属性


- 命令行参数。


- SPRING_APPLICATION_JSON 中的属性值（内嵌JSON嵌入到环境变量或系统属性中）。


- ServletConfig 初始化参数。


- ServletContext 初始化参数。


- 来自 java:comp/env 的JNDI属性。


- Java系统属性（System.getProperties()）。


- 操作系统环境变量。


- RandomValuePropertySource，只有随机的属性 random.* 中。


- jar包外面的 Profile-specific application properties （application- {profile} .properties和YAML变体）


- jar包内的 Profile-specific application properties （application-{profile}.properties和YAML变体）


- jar包外的应用属性文件（application.properties和YAML变体）。


- jar包内的应用属性文件（application.properties和YAML变体）。


- 在@Configuration上的@PropertySource注解。


- 默认属性（使用SpringApplication.setDefaultProperties设置）。

一个具体的例子，假设开发一个使用name属性的@Component：

```
import org.springframework.stereotype.*
import org.springframework.beans.factory.annotation.*

@Component
public class MyBean {

    @Value("${name}")
    private String name;

    // ...

}


```
在应用程序类路径（例如，jar中）中，可以拥有一个application.properties，它为 name 属性提供了默认属性值。 在新环境中运行时，可以在jar外部提供一个application.properties来覆盖 name 属性; 对于一次性测试，可以使用特定的命令行开关启动（例如，java -jar app.jar –name=”Spring”）。
默认情况下，SpringApplication将任何命令行选项参数（以’– ‘开头，例如–server.port=9000）转换为属性，并将其添加到Spring环境中。 如上所述，命令行属性始终优先于其他属性来源。
