# servlet
servlet code
1 在url 输人:http://localhost:8080/b1/my
将会得到页面_
Hello from MyServlet

2 在url 输入http://localhost:8080/b1/servletConfigDemo
将会得到页面
Admin:Harry Taciak
Email:admin@example.com

3 在url 输入 http://localhost:8080/b1/generic
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



6 最后把b1 这个文件整个放到tomcat 的/webapps 目录下


