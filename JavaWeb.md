# JavaWeb相关知识
-----------
## [1.JVM、jre、jdk](https://www.cnblogs.com/yangming1996/p/8508187.html)
 - JVM ：英文名称（Java Virtual Machine），就是我们耳熟能详的 Java 虚拟机。它只认 识 xxx.class 这种类型的文件，它能够将 class 文件中的字节码指令进行识别并调用操作系统向上的 API 完成动作。所以说，jvm 是 Java 能够跨平台的核心，具体的下文会详细说明。
 - JRE ：英文名称（Java Runtime Environment），我们叫它：Java 运行时环境。它主要包含两个部分，jvm 的标准实现和 Java 的一些基本类库。它相对于 jvm 来说，多出来的是一部分的 Java 类库。 
 - JDK ：英文名称（Java Development Kit），Java 开发工具包。jdk 是整个 Java 开发的核心，它集成了 jre 和一些好用的小工具。例如：javac.exe，java.exe，jar.exe 等。
 - 显然，这三者的关系是：一层层的嵌套关系。JDK>JRE>JVM 
## 2.[常见面试题](https://www.cnblogs.com/darren0415/p/6040894.html)
### JVM中的GC（垃圾回收）的机制
- 答：
- （1）如何判断一个对象是否能够被清除
- 方法一：引用计数法（实现简单，但是无法解决循环引用的问题）
在java中是通过引用来和对象进行关联的，也就是说如果要操作对象，必须通过引用来进行。那么很显然一个简单的办法就是通过引用计数来判断一个对象是否可以被回收。不失一般性，如果一个对象没有任何引用与之关联，则说明该对象基本不太可能在其他地方被使用到，那么这个对象就成为可被回收的对象了。

- 方法二：可达性分析：
在Java中采取了 可达性分析法。该方法的基本思想是通过一系列的“GC Roots”对象作为起点进行搜索，如果在“GC Roots”和一个对象之间没有可达路径，则称该对象是不可达的，不过要注意的是被判定为不可达的对象不一定就会成为可回收对象。被判定为不可达的对象要成为可回收对象必须至少经历两次标记过程，如果在这两次标记过程中仍然没有逃脱成为可回收对象的可能性，则基本上就真的成为可回收对象了。

- 备注----两次标记的过程详细：
对于用可达性分析法搜索不到的对象，GC并不一定会回收该对象。要完全回收一个对象，至少需要经过两次标记的过程。

- 第一次标记：对于一个没有其他引用的对象，筛选该对象是否有必要执行finalize()方法，如果没有执行必要，则意味可直接回收。（筛选依据：是否复写或执行过finalize()方法；因为finalize方法只能被执行一次）。
- 第二次标记：如果被筛选判定位有必要执行，则会放入FQueue队列，并自动创建一个低优先级的finalize线程来执行释放操作。如果在一个对象释放前被其他对象引用，则该对象会被移除FQueue队列。

- （2）要准确理解Java的垃圾回收机制，就要从：“什么时候”，“对什么东西”，“做了什么”三个方面来具体分析。
- 第一：“什么时候”即就是GC触发的条件。GC触发的条件有两种。（1）程序调用System.gc时可以触发；（2）系统自身来决定GC触发的时机。
系统判断GC触发的依据：根据Eden区和From Space区的内存大小来决定。当内存大小不足时，则会启动GC线程并停止应用线程。
- 第二：“对什么东西”笼统的认为是Java对象并没有错。但是准确来讲，GC操作的对象分为：通过可达性分析法无法搜索到的对象和可以搜索到的对象。对于搜索不到的方法进行标记。
- 第三：“做了什么”最浅显的理解为释放对象。但是从GC的底层机制可以看出，对于可以搜索到的对象进行复制操作，对于搜索不到的对象，调用finalize()方法进行释放。
具体过程：当GC线程启动时，会通过可达性分析法把Eden区和From Space区的存活对象复制到To Space区，然后把Eden Space和From Space区的对象释放掉。当GC轮训扫描To Space区一定次数后，把依然存活的对象复制到老年代，然后释放To Space区的对象。

### final，finally和finalize的区别
- **final**:用于申明属性，方法和类，表示属性不可变，方法不可以被覆盖，类不可以被继承。
   - java中final也用来修饰3类东西：变量，方法和类。
      - 1.变量
      final修饰变量表示该变量是不可变的。例如 final int i =1;在程序中i的值就不允许改变了。比较容易混淆的是final用来修饰引用变量时，表示该引用变量是不可变的即引用变量指向的内存地址是不变的，但是被指的内存地址中的类是可以改变的。例如：
      final MyClass myClass = new MyClass();
      这样声明myClass后，其所指向的内存地址就固定了，但仍然可以改变myClass所引用对象的成员变量。如果试图重用myClass这个变量，让其引用另一个对象则会出错。
      myClass = new MyClass();//error!!!
      - 2.方法
      final修饰方法时表示该方法是不能被子类重写的。
      - 3.类
      final修饰类时表示该类是不能被继承的，由于java的单继承关系，所以该类是继承关系链中的终端。
      关于final的几个注意事项：
      a、final变量必须在声明的时候初始化或是在构造函数中初始化；
      b、接口中声明的所有变量都是final的；
- **finally**:是异常处理语句结构中，表示总是执行的部分。　
   - 有一点值得注意，那就是尽量不要从try区段中返回(调用return)，因为只要有finally区段存在，它就一定会被执行，那么如果你在finally区段中又调用了一次return语句，则try区段中的返回值将会被遮掩，使得方法调用者得到的是finally区段中的返回值－－这常常又与程序编写的初衷相背。
- **finallize**:表示是object类一个方法，在垃圾回收机制中执行的时候会被调用被回收对象的方法。允许回收此前未回收的内存垃圾。所有object都继承了finalize()方法。

### 抽象和接口的区别
- 抽象类是对一种事物的抽象，即对类抽象，而接口是对行为的抽象。抽象类是对整个类整体进行抽象，包括属性、行为，但是接口却是对类局部（行为）进行抽象。继承是一个 "是不是"的关系，而 接口 实现则是 "有没有"的关系。如果一个类继承了某个抽象类，则子类必定是抽象类的种类，而接口实现则是有没有、具备不具备的关系，比如鸟是否能飞（或者是否具备飞行这个特点），能飞行则可以实现这个接口，不能飞行就不实现这个接口。

### JSP的四种范围？
- 答：**a、** *page*是代表一个页面相关的对象和属性。一个页面由一个编译好的java servlet类（可以带有include指令，但不可以带有include动作）表示。这既包括servlet又包括编译成servlet的jsp页面。
**b、** *request*是代表与web客户机发出的一个请求相关的对象和属性。一个请求可能跨越多个页面，涉及多个web组件（由于forware指令和include动作的关系）
**c、** *session*是代表与用于某个web客户机的一个用户体验相关的对象和属性。一个web回话也可以经常跨域多个客户机请求。
**d、** *application*是代表与整个web应用程序相关的对象和属性。这实质上是跨域整个web应用程序，包括多个页面、请求和回话的一个全局作用域。用于：网站计数器、获取Servelt API版本号、用户之间共享数据、日志的写入。


### [常见Web容器](https://www.cnblogs.com/kaleidoscope/p/9668646.html)
- WebLogic、WebSphere、JBoss、Tomcat；
- Tomcat启动:在windows下：进入bin目录，双击startup.bat
  在Linux下：cd进入bin目录，sh startup.sh


### 四种会话跟踪
**URL重写**：可以在 URL 后面附加参数，和服务器的请求一起发送这些参数为键/值对
**隐藏表单域**：
```java
<input type="hidden" id="xxx" value="xxx">
```
**Cookie**：保存在本地，不安全，服务器首先使用 Set-Cookie 响应头传输多个参数给浏览器，浏览器将其保存为 Cookie，后续对同一服务器的请求都使用
Cookie 请求头将这些参数传输给服务器
**Session**:保存在服务器，较更安全，基于前三种会话跟踪技术之一（一般是基于Cookie技术基础，如果浏览器禁用Cookie则可以采用URL重写技术），在每一次请求中只传输唯一一个参数：JSESSIONID，即会话id，服务器根据此会话id开辟一块会话内存空间，以存放其他参数


### J2EE
- J2EE本身是一个标准，一个为企业分布式应用的开发提供的标准平台。
J2EE也是一个框架，包括JDBC、JNDI、RMI、JMS、EJB、JTA等技术。


### request.getParameter()和request.getAttribute()的区别？
- 答：
```java
a、request.getParameter()获取的类型是String；
request.getAttribute()获取的类型是Object
b、request.getPrameter()获取的是POST/GET传递的参数值和URL中的参数；
request.getAttribute()获取的是对象容器中的数据值/对象
c、request.setAttribute()和request.getAttribute()可以发送、接收对象；
request.getParamter()只能接收字符串，官方不开放request.setParamter()（也就是没有这个方法）
setAttribute()和getAttribute()的传参原理：
etAttribute()是应用服务器把这个对象放在该页面所对应的一块内存中去，当你的页面服务器重定向到另外一个页面时，
应用服务器会把这块内存拷贝到另一个页面所对应的那块内存中。这个就可以通过getAttribute()获取到相应的参数值或者对象,可以实现servlet与jsp之间共享session。
```


### JSP处理运行时异常（run-time）exception？
- 答：可是使用页面的errorPaga属性捕捉没有处理的运行时异常，然后自动转向到一个错误处理页面，代码如下：
```java
<%@page errorPage="错误页面URL"%>
```
- 如果在页面请求时出现运行时异常时，以上代码会将代码转向到错误页面，在错误页面里面，可以通过以下代码定义这个页面是错误处理页面：
```java
<%@page isErrorPage="true"%>
```
- 这样描述错误信息的Throwable对象就可以在错误页面里面访问到。
### EL表达式
- 功能：
a、从四个域对象中取出数据数据显示。
b、取出请求参数数据显示。
- 原因：
在页面中用jsp脚本和jsp表达式来获取数据显示比较麻烦
a、需要判断
b、可能需要强转
    | 属性范围（jstl名称）| EL中的名称       |  范例  |
    | --------------------| :--------------- | :----------: |
    | Page                | PageScope        | ${pageScope.username}  |
    | Request             | RequestScope     | ${requestScope.username}   |
    | Session             | SessionScope     | ${sessionScope.username}   |
    | Application         | ApplicationScope | ${applicationScope.username}  |

### JavaBean
- Java Bean 是可复用的组件，对Java Bean并没有严格的规范，理论上讲，任何一个Java类都可以是一个Bean。但通常情况下，
由于Java Bean是被容器所创建（如Tomcat)的，所以Java Bean应具有一个无参的构造器，另外，通常Java Bean还要实现
Serializable接口用于实现Bean的持久性。Java Bean实际上相当于微软COM模型中的本地进程内COM组件，它是不能被跨进程访问的。


### DI机制(IoC与AOP)
- **依赖注入**（Dependecy Injection）和**控制反转**（Inversion of Control）是同一个概念，具体的讲：当某个角色需要另外一个角色协助的时候，在传统的程序设计过程中，通常由调用者来创建被调用者的实例。但在spring中创建被调用者的工作不再由调用者来完成，因此称为控制反转。创建被调用者的工作由spring来完成，然后注入调用者，因此也称为依赖注入。 
spring以动态灵活的方式来管理对象 ， 注入的两种方式，设置注入和构造注入。 
设置注入的优点：直观，自然 
构造注入的优点：可以在构造器中决定依赖关系的顺序。 

**IoC实现过程**
Spring容器将会根据配置文件来创建调用者对象，同时把被调用的对象的实例化对象通过构造函数或者set()方法的形式注入到调用者对象当中。
```java
<beans>
   <bean id = "sale" class = "Sale" singleton = "false">
      <constrctor-arg>
           <ref bean = "tea"/>
      </constrctor-arg>
   </bean>
   <bean id = "tea" class = "Bluetea" singleton = "false"/>
</beans>
```
在实现sale类的时候，需要按照下面的方式进行
```java
class Sale{
   
    private AbstracTea tea;
    public Sale(AbstracTea tea){
       this.tea = tea;
     }
}
```
当spring容器创建sale对象的时候，就会根据配置文件创建一个BlueTea对象，作为Sale构造函数的参数。当需要把BlueTea改成BlackTea时，只需要修改上述配置文件，而不需要修改代码。
需要使用Sale时候，可以通过下面的方式来创建sale对象
```java
ApplicationContext ttt = new FileSystemXmlApplicationContext("配置文件");
Sale s = （Sale）ctx.getBean（"sale"）;
```
- 什么是**AOP**？ 
面向切面编程（AOP）完善spring的依赖注入（DI），面向切面编程在spring中主要表现为两个方面 
1.面向切面编程提供声明式事务管理 
2.spring支持用户自定义的切面 

- 面向切面编程（aop）是对面向对象编程（oop）的补充， 
面向对象编程将程序分解成各个层次的对象，面向切面编程**将程序运行过程分解成各个切面**。 
AOP从程序运行角度考虑程序的结构，提取业务处理过程的切面，oop是静态的抽象，aop是**动态的抽象**， 
是对应用执行过程中的步骤进行抽象，，从而获得步骤之间的逻辑划分。
## Mysql
### 什么是事务？

- 答:事务时作为一个逻辑单元执行的一系列操作，一个逻辑工作单元必须有四个属性，称为ACID（原子性、一致性、隔离性和持久性）属性，
只有这样才能成为一个事务：
**原子性**：事务必须是原子工作单元，对于其数据修改，要么全都执行，要么全都不执行。
**一致性**：事务在完成时，必须使所有的数据保持一致的状态。在相关数据库中，所有规则都必须应用于事务的修改，以保持所有数据的完整性。事务结束时，所有的内部数据结构（如B树索引或双向链表）都必须是正确的。
**隔离性**：由并发事务所做的修改必须与任何其他并发事务所做的修改隔离。事务查看数据时数据所处的状态，要么是另一并发事务修改它之前的状态，要么是另一并发事务修改它之后的状态，事务不会查看中间状态的数据。这称为可串行性，因为它能够重新装载起始数据，并且重播一系列事务，以使数据结束时的状态与原始事务执行的状态相同。
**持久性**：事务完成后，它对于系统的影响是永久性的。该修改即使出现系统故障也将一直保持。

### 数据库优化
- MySQL的优化可以从这几个方面考虑：
（1）服务器日常运维和配置；  
（2）SQL查询优化（涉及到索引的底层数据结构，各种数据结构和存储引擎的适用场景）；
（3）客户端的优化；
（4）其他一些优化；（如加缓存中间件）