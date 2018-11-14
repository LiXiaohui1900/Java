# Maven
## 介绍
* Maven是基于POM（Project Object Model），通过一小段描述来对项目的代码、报告、文件进行管理的工具。  
* Maven是一个跨平台的项目管理工具，它是使用Java开发的，依赖于jdk1.6及以上。  
* Maven主要有两大功能：管理依赖、项目构建。  
构建过程：  
![maven构建过程][maven_process]  
## 安装
* 下载地址：[maven官网][maven_web]，下载后直接解压即可  
* 配置环境变量 MAVEN_HOME（与配置JAVA_HOME方法相同）  
* 测试Maven是否安装成功，在系统命令行执行命令：mvn -v  
## 配置
* 在maven安装目录的conf里面有一个settings.xml文件，这个文件就是maven的全局配置文件。在该文件中配置Maven本地仓库地址  
```
  <!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
  -->
  <localRepository>E:\mvn\repository</localRepository>
```
* 用户配置文件的地址：~/.m2/settings.xml。该文件默认是没有，需要将全局配置文件拷贝一份到该目录下。  
## Maven工程结构
* Project  
	* src  
		* main  
			* java  
			* resources  
		* test  
			* java  
			* resources  
	* target  
	* pom.xml  
## Maven命令的使用
* 清楚命令，清除已经编译好的class文件，具体说清除的是target目录中的文件  
`mvn clean`
* 编译的命令  
`mvn compile`
* 测试命令  
`mvn test`
* 打包命令  
`mvn package`
* 安装命令，会将打好的包安装到本地仓库  
`mvn install`
* 组合命令  
`mvn clean compile`
## M2Eclipse
* 配置  
Window --> Preferences --> Maven --> Installations --> Add --> Directory --> 选中maven安装目录  
可在 Maven --> User Settings 中选择配置文件  
* 创建Maven工程  
quickstart：创建Java工程  
webapp：创建web工程  
## Maven的核心概念
### 坐标
* groupId：定义当前Maven组织名称  
* artifactId：定义实际项目名称  
* version：定义当前项目的当前版本  
### 依赖管理  
#### 依赖范围  
![Maven依赖范围][scope]  
其中依赖范围scope 用来控制依赖和编译，测试，运行的classpath的关系. 主要的是三种依赖关系如下：  
1. compile： 默认编译依赖范围。对于编译，测试，运行三种classpath都有效
2. test：测试依赖范围。只对于测试classpath有效
3. provided：已提供依赖范围。对于编译，测试的classpath都有效，但对于运行无效。因为由容器已经提供，例如servlet-api
4. runtime:运行时提供。例如:jdbc驱动
#### 依赖传递  
![Maven依赖传递][dependency]  
#### 依赖范围传递  
![Maven依赖范围传递][scope_dependency]  
左边第一列表示第一直接依赖范围  
上面第一行表示第二直接依赖范围  
中间的交叉单元格表示传递性依赖范围  

总结：  
1. 当第二依赖的范围是compile的时候，传递性依赖的范围与第一直接依赖的范围一致。
2. 当第二直接依赖的范围是test的时候，依赖不会得以传递。
3. 当第二依赖的范围是provided的时候，只传递第一直接依赖范围也为provided的依赖，且传递性依赖的范围同样为 provided；
4. 当第二直接依赖的范围是runtime的时候，传递性依赖的范围与第一直接依赖的范围一致，但compile例外，此时传递的依赖范围为runtime






[maven_web]:http://maven.apache.org "Maven官网"
[maven_process]:img\maven构建过程.jpg "maven构建过程"
[scope]:img\scope.jpg "Maven依赖范围"
[dependency]:img\dependency.png "Maven依赖关系"
[scope_dependency]:img\scope_dependency.jpg "Maven依赖范围传递"