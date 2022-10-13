# Spring

## MVC的由来

早期JavaWeb程序由Java、JDBC、JSP组成，有些页面需要服务器动态渲染，同时JSP格式为HTML+Java混写，所以当时大部分压力都在服务器上，同时前后端需求不能及时响应，因此项目开发项目很低。如果代码还不进行规范的话就会看着非常混乱，难以维护也难以扩展，于是在这种情况下，MVC思想诞生。MVC设计思想希望程序员对于服务器层面的逻辑进行分层管理分为Model、Controller、View

- Model：主要负责处理业务，包括数据库的交互
- Controller：主要负责接收请求，然后根据不同的情况把请求转发给不同的Model层业务组件处理
- View：这一层就专门负责处理视图在后端服务器的渲染
- **MVC并不是一种具体的技术，而是一种指导思想**
  - 刚开始时的MVC
    - Servlet充当MVC里的C，接受请求
    - JDBC和一些业务类作为MVC中的M，用来处理业务逻辑
    - JSP作为MVC里面的V，视图层界面，由后端把数据动态渲染上去上去以后返回给浏览器前端显示
  - 最早的框架整合式开发SSH
    - Structs
    - Spring
    - Hibernate
  - Structs被取代，出现了SSM框架
    - Spring MVC
    - Spring
    - MyBatis

## Spring是什么

Spring 就是一个存放、管理对象的容器，具体就是用IOC。相当于我们用一个容器来管理这个项目里所有可能被用到的对象。Spring把这些对象称为Bean

## 百花齐放：框架开发时代的来临

继Spring框架推出后，Structs取代以前全省的Servlet开发的痛苦，参照Spring管理Servlet。

Hibernate 和Mybatis简化且优化了以前非常复杂且易错的JDBC原生写法。

此时Spring担起整合各个框架的职责，于是便从成了最早的框架整合式开发：SSH：Structs+Spring+Hibernate

## Spring生态圈的扩展

Spring优化Structs框架推出Spring MVC，由Structs的一个请求映射一个类的全类名到一个请求映射一个类的全类名+里面一个方法的方法签名 。Spring Web 是Spring Framework在Controller层的一套完整解决方案

### SSM和MVC有什么关系？

SSM指的是三个框架的整合开发。MVC只是一种思想 能够大幅提升后台代码质量的规范。一般把后台分为Controller -Service-DAO三个包，下面放各种Java文件，但这三个包不是对应MVC。此时：

- Controller包 使用Spring MVC，对应MVC 的Controller
- Service 和DAO层，Service就是一些Java文件，使用Soring IOC管理，DAO层使用的框架是MyBatis，对应MVC的Model
- 视图层，JSP，对应MVC的View

## Spring Boot

Spring Boot 为了简化其他框架和Spring整合开发时的繁琐配置