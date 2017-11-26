
## spring boot常用指令

在项目文件夹下：
mvn -Pnexus dependency:tree

mvn -Pnexus package -DskipTests：打包

mvn -Pnexus spring-boot:run：运行springboot

## spring boot 读书笔记（有点乱）

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

@SpringBootApplication注解代替了ComponentScan和configuration注解，更加的简便。

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
