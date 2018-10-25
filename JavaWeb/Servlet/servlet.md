# Servlet
## 一、什么是servlet
Servlet（Server Applet）是Java Servlet的简称，称为**小服务程序或服务连接器**，用Java编写的服务器端程序，主要功能在于交互式地浏览和修改数据，生成动态Web内容。  
狭义的Servlet是指Java语言实现的一个接口，广义的Servlet是指任何实现了这个Servlet接口的类，一般情况下，人们将Servlet理解为后者。Servlet运行于支持Java的应用服务器中。从原理上讲，Servlet可以响应任何类型的请求，但**绝大多数情况下Servlet只用来扩展基于HTTP协议的Web服务器**。  
## 二、执行过程
![servlet执行过程][servlet_process]
## 三、Servlet生命周期（重要）
实例化 --> 初始化 --> 服务 --> 销毁  

* 出生：（实例化-->初始化）第一次访问servlet就出生（默认情况下） 
初始化执行一次`init()`  
* 活着：（服务）应用活着，servlet就活着  
每次请求都调用`service()`  
* 死亡：（销毁）应用卸载了servlet就销毁  
应用卸载时调用`destroy()`  
<<<<<<< HEAD
```Java
public class ServletDemo1 implements Servlet{
	//Servlet生命周期的方法
	//在servlet第一次被访问时调用
	//实例化
	public ServletDemo1(){
		System.out.println("***********ServletDemo1执行了*********");
	}
	//Servlet生命周期的方法
	//在servlet第一次被访问时调用
	//初始化
	public void init(ServletConfig arg0) throws ServletException {
		System.out.println("***********init执行了*********");
		
	}
	//Servlet生命周期的方法
	//服务
	//每次访问时都会被调用
	public void service(ServletRequest arg0, ServletResponse arg1)
			throws ServletException, IOException {
		//System.out.println("hello servlet");
		System.out.println("***********service执行了*********");
	}
	
	//Servlet生命周期的方法
	//销毁
	public void destroy() {
		System.out.println("***********destroy执行了*********");
	}
```
## 四、Servlet的三种创建方式
1. 实现javax.servlet.Servlet接口  
![][implements_Servlet1]
![][implements_Servlet2]
2. 继承javax.servlet.GenericServlet类（适配器模式）  
![][GenericServlet]
3. 继承javax.servlet.http.HttpServlet类（模板方法设计模式）（开发中常用模式）  
![][HttpServlet]

=======
## 四、Servlet的三种创建方式
>>>>>>> 6dbc0e77889b6454b0b970723480da1adf697e60


--------
[servlet_process]:img/Servlet的执行过程.jpg "servlet执行过程"
[implements_Servlet1]:img/Servlet1.png
[implements_Servlet2]:img/Servlet2.png
[GenericServlet]:img/GenericServlet.png
[HttpServlet]:img/HttpServlet.png