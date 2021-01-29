# Acme.BookStore
AbpvNext With MySQL

### AbpvNext 官方教程
https://docs.abp.io/zh-Hans/abp/latest/Getting-Started?UI=MVC&DB=EF&Tiered=Yes


# DDD 概念术语

https://tech.meituan.com/2017/12/22/ddd-in-practice.html

# 战略（领域、子域、界限上下文、映射图）

* ### 领域

  现实世界中，领域包含了问题域和解系统。一般认为软件是对现实世界的部分模拟

* ### 子域

  核心域，支撑域， 通用域

* ### 界限上下文

  一个给定的业务领域会包含多个限界上下文，想与一个限界上下文沟通，则需要通过显示边界进行通信。系统通过确定的限界上下文来进行解耦，而每一个上下文内部紧密组织，职责明确，具有较高的内聚性。

  一个很形象的隐喻：细胞质所以能够存在，是因为细胞膜限定了什么在细胞内，什么在细胞外，并且确定了什么物质可以通过细胞膜。

  ![image-20210121091009384](https://github.com/MorningstarJerry/Acme.BookStore/blob/master/pic/b1.png)

* ### 上下文映射图

  **限界上下文之间的映射关系**

  - 合作关系（Partnership）：两个上下文紧密合作的关系，一荣俱荣，一损俱损。
  - 共享内核（Shared Kernel）：两个上下文依赖部分共享的模型。
  - 客户方-供应方开发（Customer-Supplier Development）：上下文之间有组织的上下游依赖。
  - 遵奉者（Conformist）：下游上下文只能盲目依赖上游上下文。
  - 防腐层（Anticorruption Layer）：一个上下文通过一些适配和转换与另一个上下文交互。
  - 开放主机服务（Open Host Service）：定义一种协议来让其他上下文来对本上下文进行访问。
  - 发布语言（Published Language）：通常与OHS一起使用，用于定义开放主机的协议。
  - 大泥球（Big Ball of Mud）：混杂在一起的上下文关系，边界不清晰。
  - 另谋他路（SeparateWay）：两个完全没有任何联系的上下文。
  - ![image-20210121091751900](https://github.com/MorningstarJerry/Acme.BookStore/blob/master/pic/b2.png)

# 战术层

* ### 实体

  当一个对象由其标识（而不是属性）区分时，这种对象称为实体（Entity）。

  例：最简单的，公安系统的身份信息录入，对于人的模拟，即认为是实体，因为每个人是独一无二的，且其具有唯一标识（如公安系统分发的身份证号码）。

  

* ### 值对象

  当一个对象用于对事务进行描述而没有唯一标识时，它被称作值对象（Value Object）。

  例：比如颜色信息，我们只需要知道{“name”:“黑色”，”css”:“#000000”}这样的值信息就能够满足要求了，这避免了我们对标识追踪带来的系统复杂性。

  

* ### 聚合

  Aggregate(聚合）是一组相关对象的集合，作为一个整体被外界访问，聚合根（Aggregate Root）是这个聚合的根节点。

  

* ### 领域服务

  > 一些重要的领域行为或操作，可以归类为领域服务。它既不是实体，也不是值对象的范畴。
  >
  > 上文中，我们将领域行为封装到领域对象中，将资源管理行为封装到资源库中，将外部上下文的交互行为封装到防腐层中。此时，我们再回过头来看领域服务时，能够发现领域服务本身所承载的职责也就更加清晰了，即就是通过串联领域对象、资源库和防腐层等一系列领域内的对象的行为，对其他上下文提供交互的接口。

* ### 应用服务

* ### 领域事件

  > 领域事件是对领域内发生的活动进行的建模。

  ![image-20210121093255483](https://github.com/MorningstarJerry/Acme.BookStore/blob/master/pic/b3.png)

  ### 

* ### 工厂

* ### 资源库

  领域对象需要资源存储，存储的手段可以是多样化的，常见的无非是数据库，分布式缓存，本地缓存等。资源库（Repository）的作用，就是对领域的存储和访问进行统一管理的对象。

* ### 集成界限上下文



# 在线建模工具

https://www.diagrameditor.com/webapp/?splash=0&ui=atlas&tr=0&gh=0&gl=0&gapi=0&od=0&db=0&lang=en

# 设计领域模型的一般步骤如下：

1. 根据需求划分出初步的领域和限界上下文，以及上下文之间的关系；
2. 进一步分析每个上下文内部，识别出哪些是实体，哪些是值对象；
3. 对实体、值对象进行关联和聚合，划分出聚合的范畴和聚合根；
4. 为聚合根设计仓储，并思考实体或值对象的创建方式；
5. 在工程中实践领域模型，并在实践中检验模型的合理性，倒推模型中不足的地方并重构。

# 经典分层结构

![image-20210121094923169](https://github.com/MorningstarJerry/Acme.BookStore/blob/master/pic/b4.png)

### 依赖倒置后 

![image-20210121170706471](https://github.com/MorningstarJerry/Acme.BookStore/blob/master/pic/b5.png)

# 数据扭转

![image-20210121095144667](https://github.com/MorningstarJerry/Acme.BookStore/blob/master/pic/b6.png)

 首先领域的开放服务通过信息传输对象（DTO）来完成与外界的数据交互；在领域内部，我们通过领域对象（DO）作为领域内部的数据和行为载体；在资源库内部，我们沿袭了原有的数据库持久化对象（PO）进行数据库资源的交互。同时，DTO与DO的转换发生在领域服务内，DO与PO的转换发生在资源库内。

# ABP 分层结构

![image-20210121100300048](https://github.com/MorningstarJerry/Acme.BookStore/blob/master/pic/b7.png)

##  Application Services vs Domain Services

While both of [Application Services](https://docs.abp.io/en/abp/latest/Application-Services) and Domain Services implement the business rules, there are fundamental logical and formal differences;

- Application Services implement the **use cases** of the application (user interactions in a typical web application), while Domain Services implement the **core, use case independent domain logic**.
- Application Services get/return [Data Transfer Objects](https://docs.abp.io/en/abp/latest/Data-Transfer-Objects), Domain Service methods typically get and return the **domain objects** ([entities](https://docs.abp.io/en/abp/latest/Entities), [value objects](https://docs.abp.io/en/abp/latest/Value-Objects)).
- Domain services are typically used by the Application Services or other Domain Services, while Application Services are used by the Presentation Layer or Client Applications.

# ABP 微服务完整示例

https://docs.abp.io/zh-Hans/abp/latest/Samples/Microservice-Demo

C:\Users\2294765\Desktop\MyWork\DevDocs\OpenSource\AbpvNext\abp-samples\MicroserviceDemo

* 拥有多个可独立可单独部署的微服务.
* 多个Web应用程序, 每一个都使用不同的API网关.
* 使用Ocelot库开发了多个网关 / BFFs (用于前端的后端).
* 包含使用IdentityServer框架开发的 身份认证服务. 它也是一个带有UI的SSO(单点登陆)应用程序.
* 有多个数据库. 一些微服务有自己的数据库,也有一些服务/应用程序共享同一个数据库(以演示不同的用例).
* 有不同类型的数据库: SQL Server (与 Entity Framework Core ORM) 和 MongoDB.
* 有一个控制台应用程序使用身份验证展示使用服务最简单的方法.
* 使用Redis做分布式缓存.
* 使用RabbitMQ做服务间的消息传递.
* 使用 Docker & Kubernates 来部署&运行所有的服务和应用程序.
* 使用 Elasticsearch & Kibana 来存储和可视化日志 (使用Serilog写日志).

![image-20210105113301005](https://github.com/MorningstarJerry/Acme.BookStore/blob/master/pic/b8.png)

https://github.com/jessetalk/AbpLoanSample

