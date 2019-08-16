# Servlet

##Servlet简介

Servlet其实是Server与Applet的缩写，所以Servlet就是，**服务端小应用**

Servlet本质上也是Java类，但要遵循Servlet的规范进行编写，没有main()方法，它的创建、使用和销毁都是由Servlet容器（例如Tomcat）进行管理的<br/>提供了Servlet功能的服务器，叫做Servlet容器，常见的容器有：Tomcat, Jetty, resin, Oracle Application server, WebLogic Server, Glassfish, Websphere, JBoss等

Servlet和HTTP协议是紧密联系的，其可以处理HTTP协议相关的所有内容，这也是Servlet应用广泛的原因之一

使用 Servlet，可以收集来自网页表单的用户输入，呈现来自数据库或者其他源的记录，动态创建网页

Java Servlet 通常情况下与使用 CGI（Common Gateway Interface，公共网关接口）实现的程序可以达到异曲同工的效果。<br/>相比于 CGI，Servlet 有以下几点优势：

> 性能更好<br/>Servlet在Web服务器的地址空间内执行（没必要创建一个单独的进程处理用户请求）<br/>Servlet是基于Java编写的，所以独立于平台<br/>服务器上的Java安全管理器保护计算机上的资源，所以Servlet是可信的<br/>Java类库的全部对Servlet都是可用的，可以通过sockets和RMI机制与applets、数据库或者其他软件交互

###Servlet构架：

![Servlet æ¶æ](https://www.runoob.com/wp-content/uploads/2014/07/servlet-arch.jpg)

### Servlet任务

- 读取客户端发送的显式消息

  > HTML上的表单，来自applet或者自定义的HTTP表单

- 读取用户发送的隐式的HTTP数据请求

  > 包括cookie、媒体类型和浏览器能够理解的压缩格式等

- 处理数据并生成结果

  > 这个过程可能需要访问数据库，执行CMI或者CORBA调用，调用Web服务，进行计算等

- 发送显式的数据

  > 包括html，xml，gif，excel等

- 发送隐式的数据

  > 包括告诉客户端返回的数据类型，设置cookies和缓存参数或者其他类似的任务

###Servlet包

Java Servlet 是运行在带有支持 Java Servlet 规范的解释器的 web 服务器上的 Java 类

### Servlet的创建

创建Servlet有三种方法：

1. 实现Servlet接口
2. 继承GenericServlet类
3. 继承HTTPServlet类

#### 实现Servlet接口

```java
public class ServletDemo1 implements Servlet {
    //public ServletDemo1(){}
    public void init(ServletConfig arg0) throws ServletException {
                System.out.println("=======init=========");
        }
    public void service(ServletRequest arg0, ServletResponse arg1)
            throws ServletException, IOException {
        System.out.println("hehe");

    }
    public void destroy() {
        System.out.println("******destroy**********");
    }
    public ServletConfig getServletConfig() {
        return null;
    }
    public String getServletInfo() {
        return null;
    }
}
```

需要实现init(),service(),destroy()

完全可以理解：

> init()是在开始的时候执行，生命周期内只执行一次<br/>service()是请求的时候的服务逻辑，每次请求执行一次<br>destroy()是在销毁的时候执行，生命周期内执行一次

#### 继承GenericServlet类

```java
public class ServletDemo2 extends GenericServlet {
    @Override
    public void service(ServletRequest arg0, ServletResponse arg1)
            throws ServletException, IOException {
        System.out.println("heihei");

    }
}
```

只需要重写service()方法，**这种方法很少用**

#### 继承HTTPService类

```java
public class ServletDemo3 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        System.out.println("haha");
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        System.out.println("ee");
        doGet(req,resp);
    }
}
```

需要重写doGet()方法和doPost()方法<br/>这种方法是最常用的<br/>也可以理解，就是处理请求方式不同的情况

#### 三个之间的关系

**Servlet**：

> Servlet 类提供了五个方法，其中三个生命周期方法和两个普通方法

**GenericServlet**

> GenericServlet是一个实现了Servlet的抽象类<br/>对其中的init(),destroy(),service()提供了默认实现
> 主要是：
> > 将 init() 中的 ServletConfig 赋给一个类级变量，可以由 getServletConfig 获得
> > 实现了空的service方法，并且声明为抽象方法，以强制要求子类实现
> > 实现了空的destroy方法
>
>大概就是强制实现service，其他方法大多需要重写，虽然没有强制

**HTTPServlet**

> HTTP进一步继承并封装了GenericServlet，使用更加简单了
> 因为拓展了Http的内容，所以还需要HTTPServiceRequest和HTTPServiceResponse
> 主要是：
>
> > 对原始的Servlet中的方法都进行了默认的操作已经不需要显示的初始化和销毁
> > 自定义了service方法，对请求进行了处理
> > 通过getMethod方法判断请求的类型，然后调用doGet或者doPost就好了
>
> 所以：只需要重写doGet和doPost方法
> 一般使用继承HttpServlet来定义一个Servlet

## Servlet的环境设置

https://www.runoob.com/servlet/servlet-environment-setup.html

## Servlet的生命周期

Servlet 生命周期可被定义为从创建直到毁灭的整个过程。以下是 Servlet 遵循的过程：

- Servlet 通过调用 **init ()** 方法进行初始化。
- Servlet 调用 **service()** 方法来处理客户端的请求。
- Servlet 通过调用 **destroy()** 方法终止（结束）。
- 最后，Servlet 是由 JVM 的垃圾回收器进行垃圾回收的。

生命周期的方法：

### init()

在第一次创建Servlet的时候调用一次，用于一次性初始化，后续不再调用
Servlet创建于用户第一调用对应该Servlet的URL时
当调用一个Servlet的时候，就会创建一个Servlet实例，每一个用户的请求会产生一个新的线程
init()简单创建或者加载一些数据，用于Servlet的整个生命周期

```java
public void init() throws ServletException{
    //初始化的代码
}
```

###service()

Service()是执行任务的实际方法
Servlet调用Service来处理客户端的请求，并把响应的数据返回
Service检查HTTP请求的类型，然后调用适当的方法：doGet,doPost,doDelete,doPut等
service由容器调用，do...方法由service调用
所以我们只需要实现do...方法即可

图解：

![Servlet çå½å¨æ](https://www.runoob.com/wp-content/uploads/2014/07/Servlet-LifeCycle.jpg)

- ​	第一个到达服务器的HTTP请求被委派到Servlet容器
- Servlet容器在调用方法之前加载Servlet
- 然后Servlet使用多线程处理多个请求，每个线程单一地执行service()方法



####doGet()

处理：

>  GET请求来自一个URL的正常请求
> 或者来自一个未指定METHOD的HTML表单

```java
public void doGet(ServletRequst request,ServletResponse response) throws ServletException IOException{
    //处理逻辑
}
```

#### doPost()

处理：

> 指定了METHOD为POST的请求

```java
public void doPost(ServletRequst request,ServletResponse response) throws ServletException IOException{
    //处理逻辑
}
```

### destroy()

destroy()只会被调用一次，在Servlet生命周期终止的时候调用

需要（可以）做的操作：

> 关闭数据库连接
> 停止后台线程
> 把Cookie列表或者点击计数器写入磁盘
> 执行其他类似的清理活动

在调用destroy之后，Servlet对象被标记为垃圾回收（**由JVM进行回收**）

```java
public void destroy(){
    //销毁逻辑
}
```



## Servlet实例

