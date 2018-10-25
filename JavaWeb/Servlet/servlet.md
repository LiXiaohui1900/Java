Servlet
======
## 目录
* [什么是servlet](#什么是servlet)
* [执行过程](#执行过程)
* [Servlet生命周期（重要）](#Servlet生命周期)
* [Servlet的三种创建方式](#Servlet的三种创建方式)
* [Servlet获取配置信息](#五、Servlet获取配置信息)
* [ServletContext（重要）](#六、ServletContext（重要）)
* [核心类图](#七、核心类图)  

什么是servlet
------
Servlet（Server Applet）是Java Servlet的简称，称为**小服务程序或服务连接器**，用Java编写的服务器端程序，主要功能在于交互式地浏览和修改数据，生成动态Web内容。  
狭义的Servlet是指Java语言实现的一个接口，广义的Servlet是指任何实现了这个Servlet接口的类，一般情况下，人们将Servlet理解为后者。Servlet运行于支持Java的应用服务器中。从原理上讲，Servlet可以响应任何类型的请求，但**绝大多数情况下Servlet只用来扩展基于HTTP协议的Web服务器**。  

执行过程
------
![servlet执行过程][servlet_process]

Servlet生命周期（重要）
------
实例化 --> 初始化 --> 服务 --> 销毁  

* 出生：（实例化-->初始化）第一次访问servlet就出生（默认情况下） 
初始化执行一次`init()`  
* 活着：（服务）应用活着，servlet就活着  
每次请求都调用`service()`  
* 死亡：（销毁）应用卸载了servlet就销毁  
应用卸载时调用`destroy()`  
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

Servlet的三种创建方式
------
1. 实现javax.servlet.Servlet接口（同上）  

2. 继承javax.servlet.GenericServlet类（适配器模式）  
```Java
public class ServletDemo2 extends GenericServlet{

	@Override
	public void service(ServletRequest arg0, ServletResponse arg1)
			throws ServletException, IOException {
		System.out.println("hello ServletDemo2");
	}

}
```
3. 继承javax.servlet.http.HttpServlet类（模板方法设计模式）（开发中常用模式）  
```Java
public class ServletDemo3 extends HttpServlet{

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp)
			throws ServletException, IOException {
		System.out.println("*******doGet *******");
		System.out.println(req.getRemoteAddr());
	}

	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp)
			throws ServletException, IOException {
		System.out.println("**********doPost**********");
	}
	
}
```


## 五、Servlet获取配置信息
方式1：  
```Java
private ServletConfig config;
//使用初始化方法恢复到ServletConfig对象，此对象由服务器创建
public void init(ServletConfig config) throws ServletException {
	this.config = config;
}

public void doGet(HttpServletRequest request, HttpServletResponse response)
		throws ServletException, IOException {
	String encoding = config.getInitParameter("encoding");//获得配置文件中的信息的
	System.out.println(encoding);
```
方式2：  
```Java
	String encoding = this.getServletConfig().getInitParameter("encoding");
	System.out.println(encoding);
```
方式3：  
```Java
	String encoding = this.getInitParameter("encoding");
	System.out.println(encoding);
```
## 六、ServletContext（重要）
ServletContext: 代表的是整个应用。一个应用只有一个ServletContext对象。
### 作用
* 域对象：在一定范围内（当前应用），使多个Servlet共享数据。  
	常用方法：  
	```Java
	void setAttribute(String name,object value);//向ServletContext对象的map中添加数据
	Object getAttribute(String name);//从ServletContext对象的map中取数据
	void rmoveAttribute(String name);//根据name去移除数据
	```
* 获取全局配置信息：
	配置当前应用的全局信息：  
	```xml
	<context-param>
		<param-name>encoding</param-name>
		<param-value>UTF-8</param-value>
	</context-param>
	```
	获取全局配置信息：  
	```Java
	String encoding = application.getInitParameter("encoding");
	```
* 获取资源路径：  
	`String getRealPath(String path);`  
	根据文件在项目中的相对路径（`/WEB-INF/...`）获取文件的绝对路径  
	```Java
	String path = this.getServletContext().getRealPath("/WEB-INF/classes/b.properties");
	Properties pro = new Properties();
	pro.load(new FileInputStream(path));
	
	System.out.println(pro.get("key"));
	```
* 实现Servlet的转发
	```Java
	ServletContext application = this.getServletContext();
	RequestDispatcher rd = application.getRequestDispatcher("/ServletContextDemo1");
	rd.forward(request,response);
	```
## 七、核心类图
![servlet核心类图][servlet_class]
	
--------
[servlet_process]:img/Servlet的执行过程.jpg "servlet执行过程"
[servlet_class]:img/Servlet规范的核心类图.jpg "servlet规范的核心类图"
