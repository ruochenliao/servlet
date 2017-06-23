# servlet
servlet code
只使用servlet 来创建web front page，在url 输人:http://localhost:8080/b1_/my
将会得到页面_
Hello from MyServlet

在url 输入http://localhost:8080/b1_/servletConfigDemo
将会得到页面
Admin:Harry Taciak
Email:admin@example.com

1、前期准备

    jdk、tomcat、EditPlus（eclipse）安装成功并且设置好环境变量。

2、由于jdk（JavaSE）是无法直接编译servlet的，所以需要将tomcat安装目录\lib\servlet-api.jar复制到java安装目录\lib下，并且为了确保没有问题，
可以在系统变量CLASSPATH后面加上“;%CATALINA_HOME%\lib\servlet-api.jar”（前提是CATALINA_HOME要设置好啊。。）

3、创建文件结构

    我在tomcat的webapps目录下新建了一个b1文件夹，下面又新增了src文件夹（放所有的java文件包括servlet）、WEB-INF文件夹（放所有的classes类、
    jar包、静态页面和web.xml），WEB-INF文件夹里新建两个文件夹：classes和lib。

4、现在可以写代码了，在src文件夹下新建文件夹servlet，在servlet文件夹内新建一个java文件，命名为MyServlet.java。
package b1;
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.Servlet;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebServlet;

@WebServlet(name = "MyServlet", urlPatterns = { "/my" })
public class MyServlet implements Servlet {
    
    private transient ServletConfig servletConfig;

    @Override
    public void init(ServletConfig servletConfig)
            throws ServletException {
        this.servletConfig = servletConfig;
    }
    
    @Override
    public ServletConfig getServletConfig() {
        return servletConfig;
    }

    @Override
    public String getServletInfo() {
        return "My Servlet";
    }

    @Override
    public void service(ServletRequest request,
            ServletResponse response) throws ServletException,
            IOException {
        String servletName = servletConfig.getServletName();
        response.setContentType("text/html");
        PrintWriter writer = response.getWriter();
        writer.print("<html><head></head>"
                + "<body>Hello from " + servletName 
                + "</body></html>");
    }

    @Override
    public void destroy() {
    }    
}

5 编译好这个java 文件，它会生成.class 文件

6 5、别以为这就完事了。。配置web.xml同样是件大事。如果没有配置这个，100%报404（我才不会告诉你我蛋疼了好长时间）

如果你用的是eclipse或者其他的工具的话，可以在建立项目时就选择创建默认的web.xml（内面的内容非常少，只有一些欢迎的页面设置）。
我是用的tomcat里example项目中改的，将这个web.xml放在WEB-INF 目录下
<?xml version="1.0" encoding="GBK"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
	http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
	version="3.1">

	<!-- 配置Servlet的名字 -->
	<servlet>
		<!-- 指定Servlet的名字，
			相当于指定@WebServlet的name属性 -->
		<servlet-name>firstServlet</servlet-name>
		<!-- 指定Servlet的实现类 -->
		<servlet-class>b1.MyServlet</servlet-class>
	</servlet>
	<!-- 配置Servlet的URL -->
	<servlet-mapping>
		<!-- 指定Servlet的名字 -->
		<servlet-name>firstServlet</servlet-name>
		<!-- 指定Servlet映射的URL地址，
			相当于指定@WebServlet的urlPatterns属性-->
		<url-pattern>/my</url-pattern>
	</servlet-mapping>

</web-app>

7 最后把b1 这个文件整个放到tomcat 的/webapps 目录下


