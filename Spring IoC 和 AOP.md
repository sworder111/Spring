# Spring IoC 和 AOP

Spring框架：Java开发的行业标准

Spring 全家桶

- Web：Spring Web MVC/Spring MVC、Spring Web Flux
- 持久层：Spring Data/Spring Data JPA、Spring Data Redis、Spring Data  MongoDB
- 安全校验：Spring Security
- 构建工程脚手架： Spring boot
- 微服务：Spring Cloud

IoC是spring全家桶各个功能模块的基础，创建对象的容器。

AOP也是以IoC为基础，AOP是面向切面编程，抽象化的面向对象

- 打印日志
- 事务
- 权限管理

## Ioc

控制反转，将对象的创建进行反转，常规情况下，对象都是开发者手动创建，使用IoC，开发者不再需要创建对象，而是由IoC容器根据需求自动创建项目所需要的对象。

不用IoC：所有对象开发者自己创建

使用IoC：对象不用开发者创建，而是交给Spring框架来完成

使用IoC的方式

1. 基于XML

   - 开发者把需要的对象在XML中进行配置，Spring框架读取这个配置文件，根据配置文件的内容来创建对象

   - 配置XML

     ``` xml
     <?xml version="1.0" encoding="UTF-8" ?>
     <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
            xmlns:context="http://www.springframework.org/schema/context"
            xmlns:aop="http://www.springframework.org/schema/aop"
            xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
         http://www.springframework.org/schema/context
         http://www.springframework.org/schema/context/spring-context-3.0.xsd
         http://www.springframework.org/schema/aop
         http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">
     </beans>
       
     ```

     

   - 在XML中配置Bean

   - ``` xml
     <bean class="com.southwind.ioc.DataConfig" id="config">
             <property name="password" value="root"></property>
             <property name="driver" value="Driver"></property>
             <property name="url" value="loalhost:8080"></property>
             <property name="userName" value="root"></property>
         </bean>
     ```

   - 在测试类中调用 ApplicationContext context = new ClassPathXmlApplicationContext(xml:"");

     System.out.println(conntext.getBean(id:""));

     

2. 基于注解

   1. 配置类

      用一个Java类代替XML文件，把在XML中配置的内容放到配置类中。

      需要在配置类中添加注解@Configuration，中含返回值为目标类的方法

      之后在类前添加Bean注释，添加注释目的为将所创建对象添加到IoC容器中供使用。

      ``` java
      @Configuration
      public class BeanConfiguration {
          @Bean
        public DataConfig dataConfig(){
            val dataConfig = new DataConfig();
            dataConfig.setPassword("root");
            dataConfig.setUrl("localhost:8080");
            dataConfig.setDriver("Driver");
            dataConfig.setUserName("root");
            return dataConfig;
        }
      }
          ApplicationContext context = new AnnotationConfigApplicationContext(com.southwind.ioc.BeanConfiguration.class);
              System.out.println(context.getBean(DataConfig.class));
          
      ```

      此时id为配置类中声明方法名(此程序中为dataConfig)，也可在Bean后添加value修改

      加载配置类的时候，会调用@Bean注释的方法，把返回的对象存入IoC容器中，供开发者使用

   2. 扫包加注解

      - 更简单的方式，不需要添加XML或者配置类，而是直接将Bean的创建交给目标类，在目标类添加注解来创建

      - 在目标类上添加@Compent--意味着将该类注入IoC容器中

      - 

      - ```java
        @Data
        @Component
        public class DataConfig {
          @Value("localhost:3306")
          private String url;
          @Value("Driver")
          private String Driver;
          @Value("root")
          private String userName;
          @Value("root")
          private String password;
        }
        ```

      - AutoWired 通过类型进行注入，与变量叫什么名字无关

      - 使用AutoWird 用于对象之间存在依赖关系时，同时调用与被调用两个类均添加@Compent标签，即同时存在IoC容器中

      - 

      - ```java
        @Component
        public class GlobalConfig {
            @Value("localhost:8080")
          private  String port;
            @Value("/")
          private String path;
            @Autowired
          private DataConfig dataConfig;
        }
        ```

      - 

        - 如果要通过名称取名：@Qualifier（“名称”）

          此时要求在寻找类型的@Compent中也添加（“名称”）且与@Qualifier 相同

## AOP

面向切面编程，是一种抽象化的面向对象编程，对面向对象编程的一种补充，底层使用动态代理机制来实现

eg：返回日志

- 不使用AOP：把业务代码和打印日志耦合起来

- 计算器方法中，日志和业务混合在一起，AOP要做的事情，把日

  志代码全部抽象出去进行统一处理，计算器方法只保留核心代码，做到核心业务和非业务代码解耦合

- 操作步骤

  1. 创建切面类
  2. 实现类添加@Compent注解
  3. 配置自动扫包，开启自动生成代理对象
  4. 使用

- 各操作作用

  - 丢失自动扫包：报错：找不到Bean，不能生成代理

  - 丢失生成自动代理：可以正常运算，但没有注释，相当于直接调用计算器，只取了一半

  - 丢失计算器@Compent标签：缺少业务代码，无法生成动态代理，报错

  - 丢失切面@Aspect标签：可以正常运算，但不会注释，相当于没有生成代理，业务调用计算器本身

  - **想要业务和日志一起输出，要保证代理对象正常创建，要保证代理对象正常创建，则业务与切面缺一不可**

  - Test中实现对象为接口

  - **一个切面只能对应一个接口的实现类且一个代理只能包含一个切面和一个实现类，因此当一个接口有多个实现类时，**

    **会报错找不到对象。**

    **代理对象是由实现类和切面对象一起生成的，一个切面类只能生成一个目标类的代理，所以要保证IoC里只能有一个实现类才可以。**