## 注解
1. @Bean
   1. 加上bean注解,spring会自动调用这个方法,并且将当前方法的返回值对象,交给IOC容器管理.成为IOC容器的Bean对象
   ```java
    //声明第三方bean
    @Bean //将当前方法的返回值对象交给IOC容器管理, 成为IOC容器bean
    public SAXReader saxReader(){
        return new SAXReader();
    }
   ```

### 为什么springboot比spring更便捷
#### 起步依赖
起步依赖的原理就是maven的依赖传递.引入一个依赖,它所依赖的依赖也会被自动引入进来

```
起步依赖本质就是maven的依赖传递吗?
chatgpt的回答
起步依赖（Starters）是 Spring Boot 提供的一种简化 Maven 或 Gradle 依赖管理的方式。你可以将它看作一种特殊类型的依赖传递，但它提供的不仅仅是依赖传递。

当你在 Maven 或 Gradle 项目中引入一个起步依赖，如 spring-boot-starter-web，它会自动引入所有为创建 web 应用程序所需的库。这包括 Spring MVC、Spring Web、Jackson（用于 JSON 处理）等等。这个过程可以看作是依赖传递的一部分，但起步依赖提供的服务比这更多。

起步依赖还可以帮助你管理依赖的版本。Spring Boot 提供了一套经过充分测试并且能很好地协同工作的库的版本。这就意味着你不需要手动指定这些库的版本，也无需担心版本冲突的问题。这一点超越了普通的依赖传递。

此外，起步依赖还与 Spring Boot 的自动配置功能紧密结合。当你引入一个起步依赖时，Spring Boot 会自动配置相关的功能。例如，当你引入 spring-boot-starter-web 时，Spring Boot 会自动配置一个嵌入式的 Tomcat 服务器，以及 Spring MVC 的所有必要设置。

所以，尽管起步依赖在一定程度上涉及到依赖传递，但它们提供的功能远不止这些。
```
#### 自动配置
当spring容器启动后,一些配置类,bean对象就自动存入到了IOC容器中,不需要我们手动去声明,简化开发.

面试的时候问springboot的原理,就是问自动配置的原理.

1. 使用三方包,要是想要将组件扫描,要在配置类上写注解@ComponentScan
2. 使用@Import注解导入.导入的是类.
   1. 导入ImportSelector接口实现类
3. 前面两种方法,都需要我们十分了解第三方包,这不现实.第三方包自己来决定要导入哪些bean.
   1. 