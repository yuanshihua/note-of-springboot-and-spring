
##目前为止第七章7.12的理解：


1,@Bean：这个注解的用于指明一个方法的实例化，配置，和初始化一个被springIOC容器管理的对象,与spring xml中的beans标签扮演着相同的角色


2,@Configuration:用@Configuration注解一个类，指明这个类的主要作用是作为一个bean定义的源头，就是说这个类是专门用于定义bean的。使用@Configuration注解的类，通过调用其他bean的方法，可以允许接口bean被定义，但是要在同一个类中。简单的例子如下：
```java
@Configuration
public class AppConfig {
    @Bean
    public MyService myService() {
        return new MyServiceImpl();
    }
}
```


3,使用AnnotationConfigApplicationContext实例化spring容器
:    使用@Configuration注解的类可以进行初始化，例如：
```
public static void main(String[] args) {
    ApplicationContext ctx = new  AnnotationConfigApplicationContext(AppConfig.class);
    MyService myService = ctx.getBean(MyService.class);
    myService.doStuff();
}
```

> *AnnotationConfigApplicationContext不局限于被@Configuration注解的类，任何被@Component注解的，或者满足JSR-330规范的类都可以作为传入的参数，初始化AnnotationConfigApplicationContext容器。例如*：

```
public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext
    (MyServiceImpl.class,Dependency1.class, Dependency2.class);
    MyService myService = ctx.getBean(MyService.class);
    myService.doStuff();
}
```
> *AnnotationConfigApplicationContext可以使用无参的构造方法实例化，但是之后要使用register()方法配置.这种方式在使用编程的方式而不是注解的方式创建AnnotationConfigApplicationContext尤为好用。*

```
public static void main(String[] args) {
    AnnotationConfigApplicationContext ctx = new  AnnotationConfigApplicationContext();
    ctx.register(AppConfig.class, OtherConfig.class);
    ctx.register(AdditionalConfig.class);
    ctx.refresh();
    MyService myService = ctx.getBean(MyService.class);
    myService.doStuff();
}
```
