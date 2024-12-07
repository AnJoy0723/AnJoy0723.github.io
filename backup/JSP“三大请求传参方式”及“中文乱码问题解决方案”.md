# 一、访问请求参数的方法
Java web中进行值传递的方法常用的有三种，分别是：

使用JSP的forward或include动作，利用传参数子动作实现参数的传递，‘

在JSP或HTML页面中，利用表单传递参数，

利用追加在网址后的参数传递或追加在超链接后的参数传递
需注意的是：在上述的三种传参方式中，方式1和方式3属于get类型的参数提交方式，而方式2属于get或post方式的参数提交方式，它可以通过form的method属性进行参数的选择，

get请求与post请求的不同之处是前者参数会显示在地址栏。

GET请求：
GET方法将请求的编码信息添加在网址后面，网址与编码信息通过"?"号分隔。如下所示：

http://www.runoob.com/hello?key1=value1&key2=value2

GET方法是浏览器默认传递参数的方法，一些敏感信息，如密码等建议不使用GET方法。

用get时，传输数据的大小有限制 （注意不是参数的个数有限制），最大为1024字节。

POST请求：
一些敏感信息，如密码等我们可以通过POST方法传递，POST提交数据是隐式的。

POST提交数据是不可见的，GET是通过在url里面传递的（可以看一下你浏览器的地址栏）。

JSP使用getParameter()来获得传递的参数，getInputStream()方法用来处理客户端的二进制数据流的请求。

同时，request对象的getParameter()方法可以接收不同的来自于JSP页面或JSP动作传递给request对象的参数信息。该方法的使用格式如下：

String 字符串变量 = request.getParameter("客户端提供参数的name属性名");

其中需要注意的是：参数name与客户端提供参数的name属性名应该相同，同时request对象的getParameter()方法返回的是string类型的参数，如果参数name的值不存在，则会返回空值null

# 二、form表单传参
表单界面代码：
```
<body>
    <form action="myjsp.jsp" method="post">
     姓名：<input type="text" name="name"><br>
     电话：<input type="text" name="tel"><br>
     <input type="submit" value="提交">
     </form>
</body>
```

接收界面代码：
```
<body>
<% String name = request.getParameter("name");
    String tel = request.getParameter("tel");
     %>
     <font facr="楷体" size=5>
     获取到的信息是：<br>
     姓名：<%=name %>
     电话：<%=tel %>
     </font>
</body>
```

## 中文乱码解决：
1、在接收界面的代码中，在获取参数值之前增加如下代码：
`**request.setCharacterEncoding("utf-8");**`
**2、在提交表单的action后的method属性需设置为“post”。**
```
 <body>
    <%
    request.setCharacterEncoding("utf-8");
    String name = request.getParameter("name");
    String tel = request.getParameter("tel");
     %>
     <font facr="楷体" size=5>
     获取到的信息是：<br>
     姓名：<%=name %>
     电话：<%=tel %>
     </font>
  </body>
```
  **3、在传递过来的中文参数中存在乱码问题，原因是中文参数采用了页面原有的“ISO-8859-1”编码，因此我们可能需要将传递过来的参数的编码格式修改为“UTF-8”格式，格式转换的代码如下：**
`  String name = new String(request.getParameter("name").getBytes("ISO-8859-1"),"UTF-8");`

# 三、网址或超链接传参
利用网址或超链接传参的格式如下：
`<a href=”超链接或网址?参数名1=参数值1&参数名2=参数值2....”>点击跳转</a>`

传值界面代码：
```
 <body>
   <a href="myjsp.jsp?name=zhangsan&tel=123456">点击传值</a>
 </body>
```
 接收界面代码：
```
  <body>
<%
    String name = request.getParameter("name");
    String tel = request.getParameter("tel");
     %>
     <font facr="楷体" size=5>
     获取到的信息是：<br>
     姓名：<%=name %>
     电话：<%=tel %>
     </font>
  </body>
  
```
**中文错误解决：**
原因是因为在超链接或网址传参中，参数属于网址的一部分，同时这一部分是属于URL编码的，不支持中文的utf-8，因此在传递中文时会显示网址错误，解决办法是将我们要传递的中文转成URL编码即可：
`java.net.URLEncoder.encode("中文","utf-8")将中文转换成URL编码`

提交界面代码：
```
<body>
   <a href="myjsp.jsp?name=<%=java.net.URLEncoder.encode("张三","utf-8") %>&tel=123456">点击传值</a>
  </body>
```

# 四、JSP子动作传参
使用JSP的forward或include动作，利用传参数子动作实现参数的传递的方式，较其他两种方式有所不同，在该方式中用户可以根据需要在request对象中添加属性，然后在另一个JSP程序中获取到添加的数据，

1、page范围
pageContext:只在一个页面中保存属性，跳转之后失效。
2、request范围
request：只在一个请求中保存，服务跳转后依然有效。
3、session范围
session:在一次会话范围内，无论何种跳转都可以使用。
4、application范围
application:在整个服务器上保存。

具体的使用方法如下：
在传值页面使用request对象的setAttribute(“name”,obj)方法，可以把数据设定在request范围内，设置数据的方法格式为：
void request.setAttribute(“key”,object);
其中key为键，string类型，是要保存的数值的属性名。Object是要保存的参数值，属于object类型，
使用上面的方法在传值页面进行请求转发之后，在接收页面使用getAttribute(“name”)方法就可以获取到name属性下的值，获取数据的方法格式如下：
Object request.getAttribute(string name);
其中的参数name表示键名，与setAttribute(“name”,obj)方法中的name相对应，获取的参数类型由obj的类型决定。

参数传递界面代码：
```
<%
//设置page范围的域对象
pageContext.setAttribute("name1", "zhangSan");
//设置request范围的域对象
request.setAttribute("name2", "liSi");
//设置session范围的域对象
session.setAttribute("name3", "wangWu");
//设置application范围的域对象
application.setAttribute("name4", "zhaoLiu");

//获取域对象中的值
out.print("page范围:"+pageContext.getAttribute("name1")+"&nbsp;&nbsp;&nbsp;&nbsp;");
out.print("request范围:"+request.getAttribute("name1")+"&nbsp;&nbsp;&nbsp;&nbsp;");
out.print("session范围:"+session.getAttribute("name1")+"&nbsp;&nbsp;&nbsp;&nbsp;");
out.print("application范围:"+application.getAttribute("name1")+"&nbsp;&nbsp;&nbsp;&nbsp;");
%>
```

**中文乱码解决**
**要在页面的最前端将整个页面的编码设置为“UTF-8”或“GBK”的编码格式，**
```
<%@ page language="java" contentType="text/html; charset=GBK"

pageEncoding="GBK"%>

```
```
<%@ page language="java" contentType="text/html; charset=UTF－８"

pageEncoding="UTF－８"%>
```