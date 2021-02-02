> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://paper.seebug.org/1075/#1-ssm

**作者：天融信阿尔法实验室  
公众号：[Java 框架级 SSM 代码审计思路](https://mp.weixin.qq.com/s?__biz=Mzg3MDAzMDQxNw==&mid=2247487592&idx=1&sn=b17808935868f9af51de882caf4d8df0&chksm=ce955d56f9e2d440d03a6901cd8a015b1434bd68a4d87e9ea27f9c84c922816ddad5626cb3ad&mpshare=1&scene=1&srcid=1113bdNAI13VhYCweeaRrnhm&sharer_sharetime=1573637041587&sharer_shareid=bafb2678ed1f77a340809d0b35c3d277&key=5d629f0789f67d1870bedd2206ff397eac9190c1e03c755a744d04c6c5b38ca5933c4a26f6bc6c076116fe68bb467bf6a06a3d589d709b1177eea64fd2b364e9ae247ed7ce4d5ff7d99951edbd7c1a94&ascene=1&uin=MzM5ODEzOTY3MQ%3D%3D&devicetype=Windows+10&version=62060833&lang=zh_CN&pass_ticket=rrTFxsqj28yb4O2XF07Nthrp%2BUPmvCCfD9Aj4ZXY%2B%2FT26iM%2Fo7hH0cMX6zkyixO%2F "Java框架级SSM代码审计思路")**

SSM 框架，即 SpringMVC+Spring+Mybatis 三个开源框架整合在一起的缩写。

在 SSM 框架之前生产环境中 SSH 框架占据多数，即 Struts2+Spring+Hibernate 三个开源框架整合而成。后因 Struts2 爆出众多高危漏洞，导致目前 SSM 逐渐代替 SSH 成为主流开发框架的选择。

审计 SSM 框架首先就要对 MVC 设计模式和，web 三层架构有一定程度的了解，限于篇幅原因这里就简单介绍一下

1.1 SpringMVC
-------------

是一种基于 Java 的实现 MVC 设计模式的请求驱动类型的轻量级 Web 框架，使用了 MVC 架构模式的思想，将 web 层进行职责解耦，基于请求驱动指的就是使用请求 - 响应模型，框架的目的就是帮助我们简化开发。

1.2 Spring
----------

是分层的 Java SE/EE full-stack 轻量级开源框架，以 IOC（Inverse of Control，控制反转）和 AOP（Aspect Oriented Programming，面向切面编程）为内核，使用基本的 JavaBean 完成以前只可能由 EJB 完成的工作，取代了 EJB 臃肿和低效的开发模式，Spring 的用途不仅仅限于服务器端的开发。从简单性、可测试性和松耦合性角度而言，绝大部分 Java 应用都可以从 Spring 中受益

1.3 Mybatis
-----------

是支持定制化 SQL、存储过程以及高级映射的优秀的持久层框架。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。MyBatis 可以对配置和原生 Map 使用简单的 XML 或注解，将接口和 Java 的 POJOs(Plain Old Java Objects, 普通的 Java 对象) 映射成数据库中的记录。

1.4 Servlet
-----------

还有一项技术虽然名称没有出现在这三个开源框架中但是 SpringMVC 的底层就是以此技术进行构建的，这项技术就是 Servlet

Servlet 是基于 Java 技术的 Web 组件，由容器管理并产生动态的内容。Servlet 与客户端通过 Servlet 容器实现的请求 / 响应模型进行交互。

相对以 SSM 框架搭建的 java web 项目进行审计，上述这些都是要有一定程度的了解的。

2.1 审计的出发点 web.xml
------------------

其实代码审计的核心思想就是追踪参数，而追踪参数的步骤就是程序执行的步骤，说白了代码审计就是一个跟踪程序执行步骤的一个过程，当我们知道了 SSM 框架的一个执行流程，自然就知道了如何如跟踪一个参数，剩下的就是去观察在参数传递的过程中有没有一些常见的漏洞点。

我们这里通过一个简单的 Demo 来描述一下 SSM 框架搭建的项目是如何完成一次用户请求，它的流程是怎么样的，而参数又是怎样被传递怎样被过滤的，当我们明白了这些，就可以尝试自己上手一些 SSM 的项目审计。

首先我把 Demo 的全部文件和文件结构粘贴出来

![](https://images.seebug.org/content/images/2019/11/13/1573637870000-clip_image002.png-w331s)

这是一个简单的图书管理 Demo 目录，功能是对图书名称，数量和简介简单的增删改查

首先不管我们是审计一个项目还是包括 Tomcat 加载一个项目一般都是由 web.xml 这个文件开始的

当然一个项目中没有 web.xml 也是可以的，可以通过 servlet3.0 开始提供的一些新注解来达到和配置 web.xml 一样的效果，但是这样的项目很少会碰到，所以我们以主流的配置 web.xml 的项目来作为讲解。

Tomcat 会加载 web.xml 文件读取文件中的内容

![](https://images.seebug.org/content/images/2019/11/13/1573637870000-clip_image004.png-w331s)

web.xml 文件主要的工作包括两部分：1、web.xml 启动 spring 容器；2、DispathcheServlet 的声明；3、其余工作是 session 过期，字符串编码等

首先是生成 DispatcherServlet 类, DispatcherServlet 是前端控制器设计模式的实现，提供 Spring Web MVC 的集中访问点（也就是把前端请求分发到目标 controller），而且与 Spring IoC 容器无缝集成，从而可以获得 Spring 的所有好处。

简单理解就是将我们的请求转发至我们的 SpringMVC 中去，交由我们 SpringMVC 的 Controller 来进行接下来的处理

然后下面有一个子标签，是生成 DispatcherServlet 时的初始化参数 contextConfigLocation 参数，Spring 会根据这个参数去加载所有逗号分隔的 xml 文件，如果没有这个参数，Spring 默认加载 WEB-INF/DispatcherServlet-servlet.xml 文件

下面的标签中还有一个子标签里面的 value 是 “/” 代表拦截所有请求。

下面的标签放在后面讲

2.2 Spring 核心配置文件 applicationContext.xml
----------------------------------------

然后我们根据加载顺序去看 applicationContext.xml

![](https://images.seebug.org/content/images/2019/11/13/1573637870000-clip_image006.png-w331s)

applicationContext.xml 中包含了三个配置文件，这三个配置文件就是我们用 Spring 来整合 SpringMVC 和 Mybaits 的配置文件，其实这三个配置文件中的内容都可以直接写 applicationContext.xml 中因为 applicationContext.xml 是 Spring 的核心配置文件，例如生成 Bean，配置连接池，生成 sqlSessionFactory。但是为了便于理解，我们这些配置分别写在三个配置文件中，由 applicationContext.xml 将这三个 xml 进行关联，由下面这张截图我们可以清晰的看到 applicationContext.xml 将这三个配置文件关联了起来。

![](https://images.seebug.org/content/images/2019/11/13/1573637870000-clip_image008.png-w331s)

首先数据经由 DispatcherServlet 派发至 Spring-mvc 的 controller 层所以我们先看 Spring-mvc.xml 这个配置文件

![](https://images.seebug.org/content/images/2019/11/13/1573637870000-clip_image010.png-w331s)

标签

如果在 web.xml 中 servlet-mapping 的 url-pattern 设置的是 /，而不是如. do。表示将所有的文件，包含静态资源文件都交给 spring mvc 处理。就需要用到了。如果不加，DispatcherServlet 则无法区分请求是资源文件还是 mvc 的注解，而导致 controller 的请求报 404 错误

在 Spring-mvc.xml 中配置后，会在 Spring MVC 上下文中定义一个 org.springframework.web.servlet.resource.DefaultServletHttpRequestHandler，它会像一个检查员，对进入 DispatcherServlet 的 URL 进行筛查，如果发现是静态资源的请求，就将该请求转由 Web 应用服务器默认的 Servlet 处理，如果不是静态资源的请求，才由 DispatcherServlet 继续处理，这么做是为了保证 Spring-mvc 优雅 REST 风格的资源 URL。

剩下两项一个是指定了返回的 view 所在的路径，另一个是指定 SpringMVC 注解的扫描路径

不难看出这个配置文件中都是和 Spring-mvc 相关的配置。

2.3 SSM 之 SpringMVC 执行流程
------------------------

接下来就是我们 SpringMVC controller 层接受前台传入的数据, 这里我们让 demo 跑起来以方便演示和讲解

![](https://images.seebug.org/content/images/2019/11/13/1573637870000-clip_image012.png-w331s)

这是我们的 index 首页，我们看下页面源码

![](https://images.seebug.org/content/images/2019/11/13/1573637871000-clip_image014.png-w331s)

可以看到 a 标签的超链接是 http://localhost:8080/SSMFrameWorkTest_war/book/allbook

${pageContext.request.contextPath}

是 JSP 取得绝对路径的方法, 也就是取出部署的应用程序名或者是当前的项目名称, 这样当我们把项目部署到生产环境中时不容易出错

后台此时收到的请求路径为 / book/allbook 首先 SpringMVC 在项目启动时会去扫描我们指定的要扫描的路径也就是 com.kuang.controller 这个路径下的所有类，我们看下 BookController 这个类的代码

![](https://images.seebug.org/content/images/2019/11/13/1573637871000-clip_image016.png-w331s)

SpringMVC 会扫描这个类中的所有注解，当看到 @Controller 是会生成该 controller 的 Bean，扫描到 @RequestMappting 注解的时候会将 @RequestMappting 中的 URI 和下面的方法形成映射。所以我们请求的 URI 是 “/book/allBool” SpringMVC 就会将数据交由 BookController 类的 list 方法来处理

我们仔细观察 list 方法，里面调用了 bookService 参数的 queryAllBook 方法，这里使用到了两个注解 @Autowired，@Qualifier，简单介绍下两个注解的作用

@Autowrite

作用：自动按照类型注入, 只要有唯一的类型匹配就能注入成功，当传入的类型不唯一时，则会报错。

@Qualiier

作用：在自动按照类型注入的基础上，在按照 bean 的 id 注入。它在给类成员注入数据时，不能独立使用。但是再给方法的形参注入数据的时候，可以独立使用。

由此可以看到 bookService 参数的类型是 BookService 类型，通过注解自动注入的 Bean 的 id 叫做 BookServiceImpl

2.4 SSM 之 Spring 执行流程
---------------------

这里我们就要从 SpringMVC 的部分过渡到 Spring 的部分了, 所谓的过渡就是我们从 SpringMVC 的 Controller 层去调用 Service 层而这 Service 就是我们使用 Spring 进行 IOC 控制和 AOP 编程的地方。

首先我们需要先要去看 spring-service.xml 这个配置文件，

![](https://images.seebug.org/content/images/2019/11/13/1573637871000-clip_image018.png-w331s)

这里我们看到了一个很重要的东西 id 为 BookServiceImpl 的 bean，我们可以看到这个 bean 的 class 路径是 com.kuang.service.BookServiceImpl，

这个标签就牵扯到了 Spring 一大核心功能点 就是 IOC(Inversion of Control) 控制反转，名字听起来特别唬人，其实特别容易理解，就是本来写一个项目需要我们自己手动去 new 一个实例出来，用了 Spring 以后我们至于要把我们需要生成实例的那个类的路径，以及我们在 new 一个实例时需要传入的的参数，传入参数的方法可以是通过构造方法，也可以通过 set 方法，我们还可以给这个 bean 起一个名称来方便我们调用（如果不用 id 参数之名的话那么这个 bean 的名称默认为 类名开头字母小写，比如 BookServiceImpl，如不特别指定，那么生成的 bean 的名称就是 bookServiceImpl）。Spring 就会在启动时将这些我们指定好的类生成的实例放入 IOC 容器中供我们使用，通俗点说就是本来由我们手动生成实例的过程交由 Spring 来做了，这就是所谓的控制反转。

接下来我们去看 BookServiceImpl 这个类的详细信息

![](https://images.seebug.org/content/images/2019/11/13/1573637871000-clip_image020.png-w331s)

首先看到该类是实现了 BookService 这个接口，ok 我们先去看 BookService 这个接口

![](https://images.seebug.org/content/images/2019/11/13/1573637871000-clip_image022.png-w331s)

可以看到接口中定义了四种方法，为了方便理解，这些方法的名字是对应着日常项目中最长用的操作数据库的四个方法即，增删改查。

好了，看完了接口我们来看接口的实现类也就是 BookServiceImpl。

![](https://images.seebug.org/content/images/2019/11/13/1573637871000-clip_image024.png-w331s)

由于实现了 BookService 这个接口，自然也就需要实现该接口下的所有方法，我们找到 queryAllBook 方法，发现 queryAllBook 调用了 bookMapper 参数的 queryAllBook 方法，而 bookMapper 是 BookMapper 类型的参数。

我们回过头来看 spring-service.xml 中的这一项配置，之前说了这一配置是将 BookServiceImpl 这个类生成一个 bean 并放入 Spring 的 IOC 容器中，标签的意思是通过该类提供的 set 方法在 bean 生成时向指定的参数注入 value，name 属性就是指定的参数的名称，可以看到我们 BookServiceImpl 中确实有一个私有参数名叫 bookMapper

并且提供了该属性的 set 方法， ref 属性是指要注入的 value 是其他的 Bean 类型，如果传入的是一些基本类型或者 String 类型就不需要用 ref 将 ref 改成 value 就可以

![](https://images.seebug.org/content/images/2019/11/13/1573637871000-clip_image026.png-w331s)

这里我们可以看到我们通过 ref 属性向 BookServiceImpl 这个类中的 bookMapper 参数注入了一个 value，这个 value 是一个其他的 bean 类型，这个 bean 的 id 叫做 bookMapper。此时由于我们 Service 层的 BookServiceImpl 的 queryAllBook 方法的实现方式其实就是调用了 id 为 bookMapper 的 bean 的 queryAllBook 方法。所以这个 id 为 bookMapper 的 bean 就是程序执行的下一步。

2.5 SSM 之 Mybatis 执行流程
----------------------

接下来就是是我们的 web 三层架构的数据访问层也就是我们 Mybaits 负责的部分，通常这一部分的包名会叫做 xxxdao，也就是开发中常说的 dao 层，该包下面的类和接口通常都叫做 xxxDao 或者 xxxMapper，当然不遵守这个规范也可以但是不推荐。此时我们的请求要从 Spring 负责的业务层过渡到 Mybatis 负责的数据层了，但是 Mybaits 和 Spring 的关系不像 SpringMVC 和 Spring 的关系一样可以无缝衔接，所以我们需要通过配置文件将 Mybatis 和 Spring 关联起来，这里我们看一下 pom.xml

![](https://images.seebug.org/content/images/2019/11/13/1573637871000-clip_image028.png-w331s)

可以看到我们导入的包除了 Mybatis 本身，还倒入了一个 mybatis-spring 的包，目的就是为了将 Mybatis 和 Spring 做结合，spring-dao.xml 也就是用来整合 Spring 和 Mybatis 的配置文件。

刚才我们看到 Spring 启动加载 bean 时会注入一个 id 为 bookMapper 的 bean 但是我们并未在之前的任何配置文件包括注解中看到有这个 bean 的相关信息，所以我们接下来要看 spring-dao.xml 中有没有和这个 bean 有关的信息

![](https://images.seebug.org/content/images/2019/11/13/1573637871000-clip_image030.png-w331s)

每项配置的作用基本都用注释的方式标明了

这里关联了一个 properties 文件

![](https://images.seebug.org/content/images/2019/11/13/1573637872000-clip_image032.png-w331s)

里面是连接数据库和配置连接池时需要的信息，没什么好说的。

我们着重看这个配置

![](https://images.seebug.org/content/images/2019/11/13/1573637872000-clip_image034.png-w331s)

这个配置通过生成 MapperScannerConfigurer 的 bean 来实现自动扫描 com.kuang.dao 下面的接口包，然后动态注入到 Spring IOC 容器中，同样动态注入的 bean 的 id 默认为类名（开头字母小写），我们看下到目录下有哪些文件。

![](https://images.seebug.org/content/images/2019/11/13/1573637872000-clip_image036.png-w331s)

我们看到有一个叫 BookMapper 的接口文件，这样就明白了之前生成 BookServiceImpl 这个 bean 是通过也就是通过 BookServiceImpl 类中的

public void setBookMapper(BookMapper bookMapper) {

? this.bookMapper = bookMapper;

}

方法注入的这个 bookMapper 是哪里来的了，是由我们配置了 MapperScannerConfigurer 这个 bean 后这个 bean 帮我们扫描 dao 包下的借口文件并生成 bean 然后再帮我们注入到 Spring 的 IOC 容器中，所以我们才可以在 BookServiceImpl 这个 bean 中通过标签注入 bookmapper 这个 bean

然后我们来看这项配置

![](https://images.seebug.org/content/images/2019/11/13/1573637872000-clip_image038.png-w331s)

这里是生成一个 id 为 sqlSessionFactory 的 bean，这里就要引出 Mybatis 中的两个关键对象即 sqlSessionFactory 和 sqlSession。

简单介绍下这两个对象

SqlSessionFactory

SqlSessionFactory 是 MyBatis 的关键对象，它是单个数据库映射关系经过编译后的内存镜像。SqlSessionFactory 对象的实例可以通过 SqlSessionBuilder 对象获得，而 SqlSessionBuilder 则可以从 XML 配置文件或一个预先定制的 Configuration 的实例构建出 SqlSessionFactory 的实例。SqlSessionFactory 是创建 SqlSession 的工厂。

SqlSession

SqlSession 是执行持久化操作的对象，类似于 JDBC 中的 Connection。它是应用程序与持久存储层之间执行交互操作的一个单线程对象。SqlSession 对象完全包括以数据库为背景的所有执行 SQL 操作的方法，它的底层封装了 JDBC 连接，可以用 SqlSession 实例来直接执行已映射的 SQL 语句。

SqlSessionFactory 和 SqlSession 的实现过程：

mybatis 框架主要是围绕着 SqlSessionFactory 进行的，创建过程大概如下：

(1)、定义一个 Configuration 对象，其中包含数据源、事务、mapper 文件资源以及影响数据库行为属性设置 settings

(2)、通过配置对象，则可以创建一个 SqlSessionFactoryBuilder 对象

(3)、通过 SqlSessionFactoryBuilder 获得 SqlSessionFactory 的实例。

(4)、SqlSessionFactory 的实例可以获得操作数据的 SqlSession 实例，通过这个实例对数据库进行

如果是 spring 和 mybaits 整合之后的配置文件, 一般以这种方式实现, SqlSessionFactory 的创建:

SqlSessionFactoryBean 是一个工厂 Bean，根据配置来创建 SqlSessionFactory

如果是单独的使用手动创建 SqlSessionFactory 和 SqlSession 话流程如下

![](https://images.seebug.org/content/images/2019/11/13/1573637872000-clip_image040.png-w331s)

看完了 SqlSessionFactory 和 SqlSession 的基础知识我们同时注意到下面这个标签的 value 属性，“classpath:mybatis-config.xml”

![](https://images.seebug.org/content/images/2019/11/13/1573637873000-clip_image042.png-w331s)

这里又引入了一个 xml 配置文件，还记得我上面刚说过 spring-dao.xml 是用来整合 Spring 和 Mybatis 的么？这个 mybatis-config.xml 就是我们 Mybatis 的配置文件。

好了 spring-dao.xml 这个用来整合 Spring 和 Mybatis 的配置文件我们已经了解了，程序按着刚才的请求接着向下走。

我们刚在走到了 BookServiceImpl 类的 queryAllBook 方法，然后该方法又是调用了 bookMapper 的 queryAllBook 方法。现在我们清楚了 bookMapper 的类型是 BookMapper

又从 sping-dao.xml 的配置文件中看到了该文件的位置位于 com.kuang.dao 路径下，我们现在就打开 BookMapper.java 文件看一看

![](https://images.seebug.org/content/images/2019/11/13/1573637873000-clip_image044.png-w331s)

我们注意到这只是个接口，我们都知道接口是不能实例化的接口只是提供一个规范，这是我们就有疑问了，那我们调用的 bookMapper 的 queryAllBook 是怎么执行的？

我们在仔细看下 dao 目录下的文件，

![](https://images.seebug.org/content/images/2019/11/13/1573637873000-clip_image046.png-w331s)

发现还有一个名字和 BookMapper.java 名字一样的 xml 文件，我们打开看一下内容。

![](https://images.seebug.org/content/images/2019/11/13/1573637873000-clip_image048.png-w331s)

看到这个文件，虽然我们对 mybatis 的了解不多，但是我们应该大概明白了，为什么我们 BookMapper 明明只是接口，我们却可以实例化生成 BookMapper 的 bean 并且可以调用他的方法了。

但是只有 BookMapper.java 和 BookMapper.xml 显然不能就是 Mybatis 的全部了，两个文件之间此时除了名字相同以外还没有什么直接联系，所以我们还需要关联起来，我们来看看 mybatis-config.xml 这个 Mybatis 的配置文件

![](https://images.seebug.org/content/images/2019/11/13/1573637873000-clip_image050.png-w331s)

我们看到了这个标签的 resource 属性的 value 就是我们 BookMapper.xml 的路径 MyBatis 是基于 sql 映射配置的框架，sql 语句都写在 Mapper 配置文件中，当构建 SqlSession 类之后，就需要去读取 Mapper 配置文件中的 sql 配置。而 标签就是用来配置需要加载的 sql 映射配置文件路径的。

也就是说最终由我们的 Spring 帮我生成 BookMapper 的代理对象然后由 Mybaits 通过标签将 BookMapper 代理对象中的方法和 BookMapper.xml 中的配置进行一一的映射，并最终执行其中的 Sql 语句。

我们看到我们此次请求最终调用的 BookMapper 的 queryAllBook 方法，这时我们就需要去 BookMapper.xml 去寻找与之对应的 Sql 语句了

![](https://images.seebug.org/content/images/2019/11/13/1573637873000-clip_image052.png-w331s)

很容易就找到了

我们看到最后执行的 sql 语句是

SELECT * from ssmbuild.books

至此我们的请求已经完成从一开始的由 DispatcherServlet 这个前端控制器派发给 SpringMVC 并最终通过 Mybatis 执行我们需要对数据库进行的操作。

生产环境的业务代码，会比这个 Demo 复杂，但是整体的执行流程和思路并不会有什么太大的变化，所以审计思路也是如此。

SSM 框架有三种配置方式，即全局采用 xml 配置文件的形式，全局采取注解的配置方式，或者注解和 xml 配置文件配合使用的方式，区别只是在于写法不一样，执行流程不会因此发生太多改变。

2.6 审计的重点 filter 过滤器
--------------------

此时在将 web.xml 时还有一个标签说放在后面讲，就是 web.xml 的标签

SpringMVC 时构建于 Servlet 之上的，所以 Servlet 中的过滤器自然也是可以使用，只不过不能配置在 spring-mvc.xml 中，而是要直接配置在 web.xml 中，因为是属于 Servlet 的技术嘛。

我们重回 web.xml

![](https://images.seebug.org/content/images/2019/11/13/1573637873000-clip_image054.png-w331s)

为了方便之前的讲解，我将这两个 filter 注释掉了。也就是说这两个 filter 并没有生效。我们以下面的 filter-name 为 XSSEscape 的 filter 来进行讲解。

首先我们此时程序是没有 XSS 防护的，所以存在存储型 XSS 漏洞，我们来尝试存储型 XSS 攻击

![](https://images.seebug.org/content/images/2019/11/13/1573637873000-clip_image056.png-w331s)

我们点击新增功能

![](https://images.seebug.org/content/images/2019/11/13/1573637873000-clip_image058.png-w331s)

看一下提交路径

![](https://images.seebug.org/content/images/2019/11/13/1573637874000-clip_image060.png-w331s)

去后台找与之对应的方法

![](https://images.seebug.org/content/images/2019/11/13/1573637874000-clip_image062.png-w331s)

找到后在这里下断点看传入参数的详细信息

![](https://images.seebug.org/content/images/2019/11/13/1573637874000-clip_image064.png-w331s)

看到没有任何过滤 XSS 语句就这么直接传了进来

![](https://images.seebug.org/content/images/2019/11/13/1573637874000-clip_image066.png-w331s)

如果我们此时想要防御这个 XSS 攻击就可以在 web.xml 中配置上我们的

![](https://images.seebug.org/content/images/2019/11/13/1573637874000-clip_image068.png-w331s)

这里声明了了我在 com.kuang.filter 的包路径下又一个类叫 XssFilter 是一个过滤器

下面的属性中的 REQUEST 的意思是

只要发起的操作是一次 HTTP 请求，比如请求某个 URL、发起了一个 GET 请求、表单提交方式为 POST 的 POST 请求、表单提交方式为 GET 的 GET 请求。一次重定向则前后相当于发起了两次请求，这些情况下有几次请求就会走几次指定过滤器。

属性 2.4 版本的 servlet 中添加的新的属性标签，总共有四个值 REQUEST,FORWARD,INCLUDE 和 ERROR，以下把这四个值简单说明一下

1、REQUEST

只要发起的操作是一次 HTTP 请求，比如请求某个 URL、发起了一个 GET 请求、表单提交方式为 POST 的 POST 请求、表单提交方式为 GET 的 GET 请求。一次重定向则前后相当于发起了两次请求，这些情况下有几次请求就会走几次指定过滤器。

2、FOWARD

只有当当前页面是通过请求转发转发过来的情形时，才会走指定的过滤器

3、INCLUDE

只要是通过，嵌入进来的页面，每嵌入的一个页面，都会走一次指定的过滤器。

4、ERROR

假如 web.xml 里面配置了`<error-page></error-page>`：

例如

```
<error-page>

    <error-code>400</error-code>

    <location>/filter/error.jsp</location>

</error-page>
```

意思是 HTTP 请求响应的状态码只要是 400、404、500 三种状态码之一，容器就会将请求转发到 error.jsp 下，这就触发了一次 error，走进了配置的 DispatchFilter。需要注意的是注意一点的是，虽然把请求转发到 error.jsp 是一次 forward 的过程，但是配置成 FORWARD 并不会走 DispatchFilter 这个过滤器。

这四种 dispatcher 方式可以单独使用，也可以组合使用，配置多个`<dispatcher></dispatcher>`即可。

在审计的时候的过滤器`<dispatcher>`属性中使用的值也是我们关注的一个点，

`<url-pattern>`属性是指明我们要过滤访问哪些资源的请求，“/*” 的意思就是拦截所有对后台的请求, 包括对一个简单的对 jsp 页面的 GET 请求，同时我们可以具体的指定拦截对某一资源的请求，同时也可以设置对某些资源的请求不过滤单独进行放过，

举例说明

```
<filter>

     <filter-name>XSSEscape</filter-name>

     <filter-class>com.springtest.filter.XssFilter</filter-class>

  </filter>

  <filter-mapping>

     <filter-name>XSSEscape</filter-name>

     <url-pattern>/com/app/UserControl</url-pattern>

     <dispatcher>REQUEST</dispatcher>

  </filter-mapping>
```

既然等指定单独过滤特定资源，自然也就可以指定对特定资源的放行。

如果设置全局的资源请求过滤的话肯定是不合理的，生产环境中又很多静态资源是不需要进行过滤的，所以我们可以指定将这些资源进行放行，

例如

```
<filter>

  <filter-name> XSSEscape </filter-name>

  <filter-class> com.springtest.filter.XssFilter </filter-class>

  <init-param>

       <!-- 配置不需要被登录过滤器拦截的链接，只支持配后缀、前缀 及全路径，多个配置用逗号分隔 -->

       <param-name>excludedPaths</param-name>

       <param-value>/pages/*,*.html,*.js,*.ico</param-value>

  </init-param>

</filter>

<filter-mapping>

  <filter-name> XSSEscape </filter-name>

  <url-pattern>/*</url-pattern>

</filter-mapping>
```

这样我们的 serlvet 在路径选择时当有对 html js 和 ico 资源发起的请求就不回在将改请求转发至 XssFilter 类。

我们在审计代码时 这里也是需要注意的一个点，因为有可能开发人员的错误配置导致本应该经过过滤器的请求，错误的给直接放行了，这样即使项目中有过滤器，也是不会生效的。

明白了`<filter>`标签的作用我们就去看 XssFilter 这个类的内容

![](https://images.seebug.org/content/images/2019/11/13/1573637874000-clip_image070.png-w331s)

可以看到 filter 包下有两个 java 类，我们先看 XssFilter 这个类

![](https://images.seebug.org/content/images/2019/11/13/1573637875000-clip_image072.png-w331s)

可以看到我们的 XssFilter 这个类实现了一个叫 Filter 的接口

我们去看一下 Filter 接口的源码

![](https://images.seebug.org/content/images/2019/11/13/1573637875000-clip_image074.png-w331s)

可以看到 Filter 所属的包是 javax.servlet

Filter 是 Servlet 的三大组件之一

javax.servlet.Filter 是一个接口，过滤请求，实现请求的拦截或者放行，并且添加新的功能

众所周知接口其实就是一个标准，所以我们想要编写自己的过滤器自然也要遵守这个标准即实现 Filter 这个接口。

Filter 接口中有三个方法，这里简单介绍一下

init 方法：

在创建完过滤器对象之后被调用。只执行一次

doFilter 方法：

执行过滤任务方法。执行多次。

destroy 方法：

Web 服务器停止或者 Web 应用重新加载，销毁过滤器对象。

当 Servlet 容器开始调用某个 Servlet 程序时，如果发现已经注册了一个 Filter 程序来对该 Servlet 进行拦截，那么容器不再直接调用 Servlet 的 service 方法，而是调用 Filter 的 doFilter 方法，再由 doFilter 方法决定是否去激活 service 方法

不难看出需要我们重点关注的方法是 doFilter 方法

![](https://images.seebug.org/content/images/2019/11/13/1573637875000-clip_image076.png-w331s)

这里的 request 的参数和 response 参数可以理解成封装了请求数据和相应数据的对象，我们需要过滤的数据就是存放在这两个对象中，

最后一个参数 FilterChain，通过名字我们猜这个参数是一个过滤链，查看一下 FilterChain 的源码

![](https://images.seebug.org/content/images/2019/11/13/1573637875000-clip_image078.png-w331s)

看到 FilterChain 是一个接口，而且这个接口只有一个方法，那就是 doFilter 方法，FilterChain 参数存在的意义就在于，在一个 Web 应用程序中可以注册多个 Filter 程序，每个 Filter 程序都可以对一个或一组 Servlet 程序进行拦截。如果有多个 Filter 程序都可以对某个 Servlet 程序的访问过程进行拦截，当针对该 Servlet 的访问请求到达时，Web 容器将把这多个 Filter 程序组合成一个 Filter 链（也叫过滤器链），

Filter 链中的各个 Filter 的拦截顺序与它们在 web.xml 文件中的映射顺序一致，上一个 Filter.doFilter 方法中调用 FilterChain.doFilter 方法将激活下一个 Filter 的 doFilter 方法，最后一个 Filter.doFilter 方法中调用的 FilterChain.doFilter 方法将激活目标 Servlet 的 service 方法

只要 Filter 链中任意一个 Filter 没有调用 FilterChain.doFilter 方法，则目标 Servlet 的 service 方法都不会被执行

介绍完 FilterChain 接下来大家应该发现，虽然名字叫过滤器

但是调用 chain.dofilter 方法似乎并没有执行任何类似过滤的工作，没有看到任何类似黑名单或者白名单的过滤规则

在调用 chain.dofilter 方法时我们传递了两个参数进去

new XSSRequestWrapper((HttpServletRequest) request) 和 response

这就是说我们传递了一个 XSSRequestWrapper 对象和 ServletRespons 对象，我们关心的当然是这个 XSSRequestWrapper

在传递参数的过程中我们通过调用 XSSRequestWrapper 的构造器，传递了 HttpServletRequest 对象，这里简单从继承关系让大家看一下 HttpServletRequest 和 ServletRequest 的关系

![](https://images.seebug.org/content/images/2019/11/13/1573637875000-clip_image080.png-w331s)

既然这里生成了一个 XSSRequestWrapper 对象并传入的参数那我们自然要跟进一探究竟

![](https://images.seebug.org/content/images/2019/11/13/1573637875000-clip_image082.png-w331s)

正好 filter 下面有一个叫 XSSRequestWrapper 的类，我们看一下代码

![](https://images.seebug.org/content/images/2019/11/13/1573637875000-clip_image084.png-w331s)

看到这里大家应该恍然大悟，原来过滤的行为是在这里进行了，而 XssFilter 的存在只是在链式执行过滤器并最终将值传给 Servlet 时调用 XSSRequestWrapper 来进行过滤并获取过滤结果而已。

这里对过滤规则就不过多赘述，网上有很多好的过滤规则，这里就不多提了。

这里肯定有很多人并明白问什么不将过滤的逻辑代码写在 XssFilter 中而是又新写了一个类，不是多此一举么？

这么做当然不是多此一举，首先解耦这个理由就已经足够充分了，其次我们看到 XSSRequestWrapper 继承了一个类 HttpServletRequestWrapper

这里我们看一下 HttpServletRequestWrapper 类的继承关系

![](https://images.seebug.org/content/images/2019/11/13/1573637875000-clip_image086.png-w331s)

我们可以看到 HttpServletRequestWrapper 是实现了 HttpServletRequest 接口的，我们这里提一下过滤这个概念，我们的想法是尽可能的把请求中的有危害的数据或者特殊符号过滤掉，然后将过滤后的数据转发向后面的业务代码并继续执行，而不是说发现请求数据中有特殊字符就直接停止执行，抛出异常，返回给用户一个 400 页面，所以既然要继续执行，那我们就要去修改或者转义 HttpServletRequest 对象中的恶意数据或者特殊字符。然而 HttpServletRequest 对象中的数据是不允许被修改的，也就是说 HttpServletRequest 对象没有提供给我们直接修改请求数据的方法。

此时矛盾就来了，我们想要修改但是 HttpServletRequest 对象又不给提供，所以 HttpServletRequestWrapper 这个类就出现了，这里用到了常见的 23 中设计模式之一的装饰者模式，限于篇幅原因不可能对装饰者模式在进行讲解了，感兴趣的同学可以自己去研究。也就是说 HttpServletRequestWrapper 这个类的出现就是为了给我们提供修改 request 请求数据的方法的，到这里大家应该就明白了为什么需要单写一个类来进行过滤的行为，不是我们想着么写，而是框架就这么设计的，为的就是解耦。

此时当 HttpServletRequestWrapper 将请求中的数据过滤完，并修改完成后返回然后作为 chain.doFilter 方法的形参进行传递。

结合之前说的，最后一个 Filter.doFilter 方法中调用的 FilterChain.doFilter 方法将激活目标 Servlet 的 service 方法

由于我们没有配置第二个 Filter 所以 XssFilter 中的 chain.doFilter 将会激活我们 Servlet 的 service 方法即 DispatcherServlet 的 service 方法，然后数据将传入我们的 SpringMVC 的 Controller 层交由我们的 BookController 来处理。

我们这次使用 filter 来演示一下效果

![](https://images.seebug.org/content/images/2019/11/13/1573637876000-clip_image088.png-w331s)

老地方下断

![](https://images.seebug.org/content/images/2019/11/13/1573637876000-clip_image090.png-w331s)

然后再次执行到这里时 XSS 语句中的特殊字符已经被 Filter 转义。

![](https://images.seebug.org/content/images/2019/11/13/1573637876000-clip_image092.png-w331s)

自然也就不会存在 Xss 的问题了。

3.1 思路总结
--------

最后总结一下 SSM 框架的审计思路，审计思路其实就是我们代码的执行思路

和审计非 SSM 框架代码的主要区别就是在于 SSM 框架的各种 XML 配置，和注解配置，需要我们根据 XML 中的配置和注解来查看代码的执行路径，SSM 框架中常见的注解和注解中的属性，以及常见的标签和标签的各个属性。

审计漏洞的方式同正常的 java 代码审计没有区别，网上有很多非常优秀的 java 代码审计文章，关于每个漏洞的审计方式写的都非常全面，我们需要的就只是将其移植到 SSM 框架的审计中来，我们明白 SSM 的执行流程了，自然就明白了该怎么在 SSM 框架中跟踪参数，例如刚刚讲的 XSS 漏洞，我们根据 XML 中的配置和注解中的配置一路跟到了 Mybatis 的 mapper.xml 这个映射文件，找到了最中执行的

```
insert into ssmbuild.books(bookName,bookCounts,detail)

         values (#{bookName}, #{bookCounts}, #{detail})
```

这个 sql 语句，发现我们传入的 books 参数直到 sql 语句执行的前一刻都没有经过任何的过滤处理，所以此处插入数据库的参数自然是不可信的脏数据。

当我们再次查询这条数据并返回到前端时就非常可能造成存储型 XSS 攻击

我们在审计这类漏洞时，最简单的方法就是先去 web.xml 中去查看有没有配置相关的过滤器，如果有哪我们就去看看过滤器的规则是否严格，如果没有那就很有可能存在漏洞。

3.2 补充知识
--------

最后还要提一个必要重要的 Mybaits 知识点就是 Mybatis 的预编译，关于 java 的预编译简单介绍一下

非预编译的情况下我们每次执行 sql 都需要将 slq 和参数拼接在一起然后传给数据库编译执行，这样采用拼接的方式非常容易产生 SQL 注入漏洞，当然可以使用 filter 对参数进行过滤来避免产生 SQL 注入，

而在预编译的情况下，程序会提前将我们的 sql 语句编译好，程序执行的时候只需要将传递进来的参数交由数据库进行操作就可以了，此时不论传来的参数是什么，都不会被当成时 SQL 语句的一部分，因为真正的 SQL 语句已经提前编译好了，所以即使不过滤也不会产生 SQL 注入这类漏洞，

以下面这个 mapper.xml 中的 SQL 语句举例

```
insert into ssmbuild.books(bookName,bookCounts,detail)

         values (#{bookName}, #{bookCounts}, #{detail})
```

#{bookName} 这种形式的就是采用了预编译的形式传参，而以下这种形式

```
insert into ssmbuild.books(bookName,bookCounts,detail)

         values ('${bookName}','${bookCounts}', '${detail}')
```

'${bookName}'这种写法就是没有使用预编译的形式进行传参数，此时如果不对传入的参数进行过滤和校验的话就会产生 SQL 注入漏洞

'${xxxx}'和 #{xxxx} 其实就是 jdbc 的 Statement 和 PreparedStatement 对象。

3.3 学习建议
--------

整篇文章对 SSM 框架的整个执行流程和审计流程进行了简单的讲解，后续想要增强 SSM 框架的审计水平，推荐大家自己上手一些简单 SSM 框架搭建的项目，实战永远是最快的学习方式，大家在审计 SSM 框架可能遇到的最大的困难就是有很多新的之前没有碰到过的注解，和 XML 中一些 SSM 独有的标签，这些注解和标签数量很多，没有办法在一篇文章中讲完，大家碰到不懂的注解和标签都可以通过官方提供的文档和搜索引擎来寻找答案。

最后感谢大家的耐心观看。

![](https://images.seebug.org/content/images/2017/08/0e69b04c-e31f-4884-8091-24ec334fbd7e.jpeg) 本文由 Seebug Paper 发布，如需转载请注明来源。本文地址：[https://paper.seebug.org/1075/](https://paper.seebug.org/1075/)