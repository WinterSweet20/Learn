# Servlet

[菜鸟](https://www.runoob.com/servlet/servlet-tutorial.html):https://www.runoob.com/servlet/servlet-tutorial.html

##Servlet简介

Servlet其实是Server与Applet的缩写，所以Servlet就是，**服务端小应用**

Servlet本质上也是Java类，但要遵循Servlet的规范进行编写，没有main()方法，它的创建、使用和销毁都是由Servlet容器（例如Tomcat）进行管理的<br/>提供了Servlet功能的服务器，叫做Servlet容器，常见的容器有：Tomcat, Jetty, resin, Oracle Application server, WebLogic Server, Glassfish, Websphere, JBoss等

Servlet和HTTP协议是紧密联系的，其可以处理HTTP协议相关的所有内容，这也是Servlet应用广泛的原因之一

使用 Servlet，可以收集来自网页表单的用户输入，呈现来自数据库或者其他源的记录，动态创建网页

Java Servlet 通常情况下与使用 CGI（Common Gateway Interface，公共网关接口）实现的程序可以达到异曲同工的效果。<br/>相比于 CGI，Servlet 有以下几点优势：

> 性能更好
> Servlet在Web服务器的地址空间内执行（没必要创建一个单独的进程处理用户请求）
> Servlet是基于Java编写的，所以独立于平台
> 服务器上的Java安全管理器保护计算机上的资源，所以Servlet是可信的<br/>Java类库的全部对Servlet都是可用的，可以通过sockets和RMI机制与applets、数据库或者其他软件交互

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

==注意==：

destroy被调用后，Servlet被销毁，但是并没有立即被回收，再次请求时，并没有重新初始化



## Servlet实例

```java
// 导入必需的 java 库
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class HelloWorld extends HttpServlet {
  private String message;
  public void init() throws ServletException{
      // 执行必需的初始化
      message = "Hello World";
  }

  public void doGet(HttpServletRequest request,HttpServletResponse response)
      throws ServletException, IOException{
      // 设置响应内容类型
      response.setContentType("text/html");
      // 实际的逻辑是在这里
      PrintWriter out = response.getWriter();
      out.println("<h1>" + message + "</h1>");
  }
  public void destroy(){
      // 什么也不做
  }
}
```

### 编译

### 部署

比较没有兴趣，[链接](https://www.runoob.com/servlet/servlet-first-example.html)

## Servlet表单数据

### GET方法

向用户发送已编码的用户信息，页面和已编码的信息中间用?隔开：

```http
http://www.test.com/hello?key1=value1&key2=value2
```

浏览器向服务器传递信息默认使用GET方法
如果传递的是密码或者其他敏感信息，不要使用GET方法，会出现在地址栏中
GET方法有大小限制，最多只能有1024个字符
这些信息使用QUERY_STRING头传递，可通过QUERY_STRING环境变量访问，使用doGet()方法处理

### POST请求

打包消息的方法与GET基本相同
不是把信息放在URL中进行发送，而是把这些信息作为单独的一个消息
消息以标准输出的形式传到后台程序，您可以解析和使用这些标准输出
使用doPost()处理这些请求

###读取表单数据

方法：

- `getParameter()`:你可以调用`request.getParameter()`来获取表单参数的值
- `getParameterValues()`：如果参数出现一次以上，则调用该方法，并返回多个值，例如复选框
- `getParameterNames()`：如果想得到完整的所有参数列表，使用该方法

### 使用GET方法实例

请求的url：

`http://localhost:8080/TomcatTest/HelloForm?name=菜鸟教程&url=www.runoob.com`

处理的service方法：

```java
 protected void doGet(HttpServletRequest request, HttpServletResponse response)
     throws ServletException, IOException {
        // 设置响应内容类型
     response.setContentType("text/html;charset=UTF-8");
     PrintWriter out = response.getWriter();
     String title = "使用 GET 方法读取表单数据";
     // 处理中文
     String name =new String(request.getParameter("name").getBytes("ISO8859-1"),"UTF-8");
     String docType = "<!DOCTYPE html> \n";
     out.println(docType +
                 "<html>\n" +
                 "<head><title>" + title + "</title></head>\n" +
                 "<body bgcolor=\"#f0f0f0\">\n" +
                 "<h1 align=\"center\">" + title + "</h1>\n" +
                 "<ul>\n" +
                 "  <li><b>站点名</b>："
                 + name + "\n" +
                 "  <li><b>网址</b>："
                 + request.getParameter("url") + "\n" +
                 "</ul>\n" +
                 "</body></html>");
    }
```

需要注意的：`response.getWriter().println()`、`request.getParameter().getBytes()`

## 客户端HTTP请求

当客户端请求网页时，它会像Web服务器发送特定的信息（HTTP头），这些信息不能被直接读取

部分重要的头信息：

| 头信息              | 描述                                                         |
| :------------------ | :----------------------------------------------------------- |
| Accept              | 这个头信息指定浏览器或其他客户端可以处理的 MIME 类型。值 **image/png** 或 **image/jpeg** 是最常见的两种可能值。 |
| Accept-Charset      | 这个头信息指定浏览器可以用来显示信息的字符集。例如 ISO-8859-1。 |
| Accept-Encoding     | 这个头信息指定浏览器知道如何处理的编码类型。值 **gzip** 或 **compress** 是最常见的两种可能值。 |
| Accept-Language     | 这个头信息指定客户端的首选语言，在这种情况下，Servlet 会产生多种语言的结果。例如，en、en-us、ru 等。 |
| Authorization       | 这个头信息用于客户端在访问受密码保护的网页时识别自己的身份。 |
| Connection          | 这个头信息指示客户端是否可以处理持久 HTTP 连接。持久连接允许客户端或其他浏览器通过单个请求来检索多个文件。值 **Keep-Alive** 意味着使用了持续连接。 |
| Content-Length      | 这个头信息只适用于 POST 请求，并给出 POST 数据的大小（以字节为单位）。 |
| Cookie              | 这个头信息把之前发送到浏览器的 cookies 返回到服务器。        |
| Host                | 这个头信息指定原始的 URL 中的主机和端口。                    |
| If-Modified-Since   | 这个头信息表示只有当页面在指定的日期后已更改时，客户端想要的页面。如果没有新的结果可以使用，服务器会发送一个 304 代码，表示 **Not Modified** 头信息。 |
| If-Unmodified-Since | 这个头信息是 If-Modified-Since 的对立面，它指定只有当文档早于指定日期时，操作才会成功。 |
| Referer             | 这个头信息指示所指向的 Web 页的 URL。例如，如果您在网页 1，点击一个链接到网页 2，当浏览器请求网页 2 时，网页 1 的 URL 就会包含在 Referer 头信息中。 |
| User-Agent          | 这个头信息识别发出请求的浏览器或其他客户端，并可以向不同类型的浏览器返回不同的内容。 |

获取HTTP头的方法：

| 序号 | 方法 & 描述                                                  |
| :--- | :----------------------------------------------------------- |
| 1    | **Cookie[] getCookies()** 返回一个数组，包含客户端发送该请求的所有的 Cookie 对象。 |
| 2    | **Enumeration getAttributeNames()** 返回一个枚举，包含提供给该请求可用的属性名称。 |
| 3    | **Enumeration getHeaderNames()** 返回一个枚举，包含在该请求中包含的所有的头名。 |
| 4    | **Enumeration getParameterNames()** 返回一个 String 对象的枚举，包含在该请求中包含的参数的名称。 |
| 5    | **HttpSession getSession()** 返回与该请求关联的当前 session 会话，或者如果请求没有 session 会话，则创建一个。 |
| 6    | **HttpSession getSession(boolean create)** 返回与该请求关联的当前 HttpSession，或者如果没有当前会话，且创建是真的，则返回一个新的 session 会话。 |
| 7    | **Locale getLocale()** 基于 Accept-Language 头，返回客户端接受内容的首选的区域设置。 |
| 8    | **Object getAttribute(String name)** 以对象形式返回已命名属性的值，如果没有给定名称的属性存在，则返回 null。 |
| 9    | **ServletInputStream getInputStream()** 使用 ServletInputStream，以二进制数据形式检索请求的主体。 |
| 10   | **String getAuthType()** 返回用于保护 Servlet 的身份验证方案的名称，例如，"BASIC" 或 "SSL"，如果JSP没有受到保护则返回 null。 |
| 11   | **String getCharacterEncoding()** 返回请求主体中使用的字符编码的名称。 |
| 12   | **String getContentType()** 返回请求主体的 MIME 类型，如果不知道类型则返回 null。 |
| 13   | **String getContextPath()** 返回指示请求上下文的请求 URI 部分。 |
| 14   | **String getHeader(String name)** 以字符串形式返回指定的请求头的值。 |
| 15   | **String getMethod()** 返回请求的 HTTP 方法的名称，例如，GET、POST 或 PUT。 |
| 16   | **String getParameter(String name)** 以字符串形式返回请求参数的值，或者如果参数不存在则返回 null。 |
| 17   | **String getPathInfo()** 当请求发出时，返回与客户端发送的 URL 相关的任何额外的路径信息。 |
| 18   | **String getProtocol()** 返回请求协议的名称和版本。          |
| 19   | **String getQueryString()** 返回包含在路径后的请求 URL 中的查询字符串。 |
| 20   | **String getRemoteAddr()** 返回发送请求的客户端的互联网协议（IP）地址。 |
| 21   | **String getRemoteHost()** 返回发送请求的客户端的完全限定名称。 |
| 22   | **String getRemoteUser()** 如果用户已通过身份验证，则返回发出请求的登录用户，或者如果用户未通过身份验证，则返回 null。 |
| 23   | **String getRequestURI()** 从协议名称直到 HTTP 请求的第一行的查询字符串中，返回该请求的 URL 的一部分。 |
| 24   | **String getRequestedSessionId()** 返回由客户端指定的 session 会话 ID。 |
| 25   | **String getServletPath()** 返回调用 JSP 的请求的 URL 的一部分。 |
| 26   | **String[] getParameterValues(String name)** 返回一个字符串对象的数组，包含所有给定的请求参数的值，如果参数不存在则返回 null。 |
| 27   | **boolean isSecure()** 返回一个布尔值，指示请求是否使用安全通道，如 HTTPS。 |
| 28   | **int getContentLength()** 以字节为单位返回请求主体的长度，并提供输入流，或者如果长度未知则返回 -1。 |
| 29   | **int getIntHeader(String name)** 返回指定的请求头的值为一个 int 值。 |
| 30   | **int getServerPort()** 返回接收到这个请求的端口号。         |
| 31   | **int getParameterMap()** 将参数封装成 Map 类型。            |

###实例

```java
public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{
        // 设置响应内容类型
        response.setContentType("text/html;charset=UTF-8");
        PrintWriter out = response.getWriter();
        String title = "HTTP Header 请求实例 - 菜鸟教程实例";
    	String docType = "<!DOCTYPE html> \n";
    	out.println(docType +
            "<html>\n" +
            "<head><meta charset=\"utf-8\"><title>" + title + "</title></head>\n"+
            "<body bgcolor=\"#f0f0f0\">\n" +
            "<h1 align=\"center\">" + title + "</h1>\n" +
            "<table width=\"100%\" border=\"1\" align=\"center\">\n" +
            "<tr bgcolor=\"#949494\">\n" +
            "<th>Header 名称</th><th>Header 值</th>\n"+
            "</tr>\n");
        Enumeration headerNames = request.getHeaderNames();
        while(headerNames.hasMoreElements()) {
            String paramName = (String)headerNames.nextElement();
            out.print("<tr><td>" + paramName + "</td>\n");
            String paramValue = request.getHeader(paramName);
            out.println("<td> " + paramValue + "</td></tr>\n");
        }
        out.println("</table>\n</body></html>");
    }

```

上面是实例的doGet()方法

主要是：

> `request.getHeaderName()`：获取所有的请求头名字
> `headerNames.nextElement()`：枚举的下一条数据
> `request.getHeader(paramName)`：通过头名称获取头的信息



## 服务器HTTP响应

当一个Web服务器响应一个HTTP请求时，响应一般包括：

**一个状态行、一些响应报头、一个空行和文档**

一个典型的响应：

```http
HTTP/1.1 200 OK
Content-Type: text/html
Header2: ...
...
HeaderN: ...
  (Blank Line)
<!doctype ...>
<html>
<head>...</head>
<body>
...
</body>
</html>
```

状态行应该包括：**HTTP版本、一个状态码和一个状态码对应的短消息**

###常用的响应报头：

| 头信息              | 描述                                                         |
| :------------------ | :----------------------------------------------------------- |
| Allow               | 这个头信息指定服务器支持的请求方法（GET、POST 等）。         |
| Cache-Control       | 这个头信息指定响应文档在何种情况下可以安全地缓存。可能的值有：**public、private** 或 **no-cache** 等。Public 意味着文档是可缓存，Private 意味着文档是单个用户私用文档，且只能存储在私有（非共享）缓存中，no-cache 意味着文档不应被缓存。 |
| Connection          | 这个头信息指示浏览器是否使用持久 HTTP 连接。值 **close** 指示浏览器不使用持久 HTTP 连接，值 **keep-alive** 意味着使用持久连接。 |
| Content-Disposition | 这个头信息可以让您请求浏览器要求用户以给定名称的文件把响应保存到磁盘。 |
| Content-Encoding    | 在传输过程中，这个头信息指定页面的编码方式。                 |
| Content-Language    | 这个头信息表示文档编写所使用的语言。例如，en、en-us、ru 等。 |
| Content-Length      | 这个头信息指示响应中的字节数。只有当浏览器使用持久（keep-alive）HTTP 连接时才需要这些信息。 |
| Content-Type        | 这个头信息提供了响应文档的 MIME（Multipurpose Internet Mail Extension）类型。 |
| Expires             | 这个头信息指定内容过期的时间，在这之后内容不再被缓存。       |
| Last-Modified       | 这个头信息指示文档的最后修改时间。然后，客户端可以缓存文件，并在以后的请求中通过 **If-Modified-Since** 请求头信息提供一个日期。 |
| Location            | 这个头信息应被包含在所有的带有状态码的响应中。在 300s 内，这会通知浏览器文档的地址。浏览器会自动重新连接到这个位置，并获取新的文档。 |
| Refresh             | 这个头信息指定浏览器应该如何尽快请求更新的页面。您可以指定页面刷新的秒数。 |
| Retry-After         | 这个头信息可以与 503（Service Unavailable 服务不可用）响应配合使用，这会告诉客户端多久就可以重复它的请求。 |
| Set-Cookie          | 这个头信息指定一个与页面关联的 cookie。                      |

### 设置报头的常用方法



| 序号 | 方法 & 描述                                                  |
| :--- | :----------------------------------------------------------- |
| 1    | **String encodeRedirectURL(String url)** 为 sendRedirect 方法中使用的指定的 URL 进行编码，或者如果编码不是必需的，则返回 URL 未改变。 |
| 2    | **String encodeURL(String url)** 对包含 session 会话 ID 的指定 URL 进行编码，或者如果编码不是必需的，则返回 URL 未改变。 |
| 3    | **boolean containsHeader(String name)** 返回一个布尔值，指示是否已经设置已命名的响应报头。 |
| 4    | **boolean isCommitted()** 返回一个布尔值，指示响应是否已经提交。 |
| 5    | **void addCookie(Cookie cookie)** 把指定的 cookie 添加到响应。 |
| 6    | **void addDateHeader(String name, long date)** 添加一个带有给定的名称和日期值的响应报头。 |
| 7    | **void addHeader(String name, String value)** 添加一个带有给定的名称和值的响应报头。 |
| 8    | **void addIntHeader(String name, int value)** 添加一个带有给定的名称和整数值的响应报头。 |
| 9    | **void flushBuffer()** 强制任何在缓冲区中的内容被写入到客户端。 |
| 10   | **void reset()** 清除缓冲区中存在的任何数据，包括状态码和头。 |
| 11   | **void resetBuffer()** 清除响应中基础缓冲区的内容，不清除状态码和头。 |
| 12   | **void sendError(int sc)** 使用指定的状态码发送错误响应到客户端，并清除缓冲区。 |
| 13   | **void sendError(int sc, String msg)** 使用指定的状态发送错误响应到客户端。 |
| 14   | **void sendRedirect(String location)** 使用指定的重定向位置 URL 发送临时重定向响应到客户端。 |
| 15   | **void setBufferSize(int size)** 为响应主体设置首选的缓冲区大小。 |
| 16   | **void setCharacterEncoding(String charset)** 设置被发送到客户端的响应的字符编码（MIME 字符集）例如，UTF-8。 |
| 17   | **void setContentLength(int len)** 设置在 HTTP Servlet 响应中的内容主体的长度，该方法设置 HTTP Content-Length 头。 |
| 18   | **void setContentType(String type)** 如果响应还未被提交，设置被发送到客户端的响应的内容类型。 |
| 19   | **void setDateHeader(String name, long date)** 设置一个带有给定的名称和日期值的响应报头。 |
| 20   | **void setHeader(String name, String value)** 设置一个带有给定的名称和值的响应报头。 |
| 21   | **void setIntHeader(String name, int value)** 设置一个带有给定的名称和整数值的响应报头。 |
| 22   | **void setLocale(Locale loc)** 如果响应还未被提交，设置响应的区域。 |
| 23   | **void setStatus(int sc)** 为该响应设置状态码。              |

### 实例

```java
public void doGet(HttpServletRequest request,
                        HttpServletResponse response)
                throws ServletException, IOException
      {
          // 设置刷新自动加载时间为 5 秒
          response.setIntHeader("Refresh", 5);
          // 设置响应内容类型
          response.setContentType("text/html;charset=UTF-8");
         
          //使用默认时区和语言环境获得一个日历  
          Calendar cale = Calendar.getInstance();  
          //将Calendar类型转换成Date类型  
          Date tasktime=cale.getTime();  
          //设置日期输出的格式  
          SimpleDateFormat df=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");  
          //格式化输出  
          String nowTime = df.format(tasktime);
          PrintWriter out = response.getWriter();
          String title = "自动刷新 Header 设置 - 菜鸟教程实例";
          String docType =
          "<!DOCTYPE html>\n";
          out.println(docType +
            "<html>\n" +
            "<head><title>" + title + "</title></head>\n"+
            "<body bgcolor=\"#f0f0f0\">\n" +
            "<h1 align=\"center\">" + title + "</h1>\n" +
            "<p>当前时间是：" + nowTime + "</p>\n");
      }
```

主要是`response.set......`等一系列方法
（*可以看一下获取时间格式化输出的实例*）

## HTTP状态码

###HTTP状态码

状态码列表及部分原因移步，[Http状态码](./Http状态码.md)

###设置HTTP状态码方法

常用方法（通过`HttpServletResponse`对象可用）：

| 序号 | 方法 & 描述                                                  |
| :--- | :----------------------------------------------------------- |
| 1    | **public void setStatus ( int statusCode )** 该方法设置一个任意的状态码。setStatus 方法接受一个 int（状态码）作为参数。如果您的响应包含了一个特殊的状态码和文档，请确保在使用 *PrintWriter* 实际返回任何内容之前调用 setStatus。 |
| 2    | **public void sendRedirect(String url)** 该方法生成一个 302 响应，连同一个带有新文档 URL 的 *Location* 头。 |
| 3    | **public void sendError(int code, String message)** 该方法发送一个状态码（通常为 404），连同一个在 HTML 文档内部自动格式化并发送到客户端的短消息。 |

实例：

```java
public void doGet(HttpServletRequest request,HttpServletResponse response)
            throws ServletException, IOException
  {
      // 设置错误代码和原因
      response.sendError(407, "Need authentication!!!" );
  }
```

主要是：`response.sendError()`，其他的方法自己找



## 编写过滤器



#web.xml

https://blog.csdn.net/m751075306/article/details/9452893

https://blog.csdn.net/believejava/article/details/43229361

https://www.jianshu.com/p/285ad45f60d1