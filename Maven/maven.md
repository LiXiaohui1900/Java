# Maven  

## 介绍
* Maven是基于POM（Project Object Model），通过一小段描述来对项目的代码、报告、文件进行管理的工具。  
* Maven是一个跨平台的项目管理工具，它是使用Java开发的，依赖于jdk1.6及以上。  
* Maven主要有两大功能：管理依赖、项目构建。  
构建过程：  
![maven构建过程][maven_process]  
## 安装
* 下载地址：[http://maven.apache.org][maven_web]{:target="_blank"}，下载后直接解压即可  
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

#### 可选依赖  
```xml
    <dependency>
      <groupId>com.itheima.maven</groupId>
      <artifactId>MavenFirst</artifactId>
      <version>0.0.1-SNAPSHOT</version>
      <optional>true</optional>
    </dependency>
```
Optional标签标示该依赖是否可选，默认是false。可以理解为，如果为true，则表示该依赖不会传递下去，如果为false，则会传递下去。  

#### 排除依赖  
```xml
    <dependency>
      <groupId>com.itheima.maven</groupId>
      <artifactId>MavenFirst</artifactId>
      <version>0.0.1-SNAPSHOT</version>
      <exclusions>
        <exclusion>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context-support</artifactId>
        </exclusion>
      </exclusions>
    </dependency>
```
排除第一依赖中<exclusions>标签下的依赖  

### 生命周期  
Maven有三个生命周期：clean生命周期、default生命周期、site生命周期  
生命周期可以理解为项目构建的步骤集合  
在maven中，只要在同一个生命周期，你执行后面的阶段，那么前面的阶段也会被执行，而且不需要额外去输入前面的阶段，这样大大减轻了程序员的工作。  

#### Clean生命周期  
```
pre-clean 执行一些需要在clean之前完成的工作 
clean 移除所有上一次构建生成的文件 
post-clean 执行一些需要在clean之后立刻完成的工作
```
mvn clean命令，等同于 mvn pre-clean clean。只要执行后面的命令，那么前面的命令都会执行，不需要再重新去输入命令。  

#### Default生命周期（重点）
```diff
validate 
generate-sources 
process-sources 
generate-resources 
process-resources 复制并处理资源文件，至目标目录，准备打包。 
- compile 编译项目的源代码。 
process-classes 
generate-test-sources 
process-test-sources 
generate-test-resources 
process-test-resources 复制并处理资源文件，至目标测试目录。 
test-compile 编译测试源代码。 
process-test-classes 
- test 使用合适的单元测试框架运行测试。这些测试代码不会被打包或部署。 
prepare-package 
- package 接受编译好的代码，打包成可发布的格式，如 JAR 。 
pre-integration-test 
integration-test 
post-integration-test 
verify 
- install 将包安装至本地仓库，以让其它项目依赖。 
deploy 将最终的包复制到远程的仓库，以让其它开发人员与项目共享。
```

#### Site生命周期  
```
pre-site 执行一些需要在生成站点文档之前完成的工作 
site 生成项目的站点文档 
post-site 执行一些需要在生成站点文档之后完成的工作，并且为部署做准备 
site-deploy 将生成的站点文档部署到特定的服务器上 
```

### 插件  
插件（plugin），每个插件都能实现一个阶段的功能。Maven的核心是生命周期，但是生命周期相当于主要指定了maven命令执行的流程顺序，而没有真正实现流程的功能，功能是有插件来实现的。  
比如：compile就是一个插件实现的功能。

#### 编译插件  
```xml
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <configuration>
          <source>1.7</source> <!-- 源代码使用的开发版本 -->
          <target>1.7</target> <!-- 需要生成的目标class文件的编译版本 -->
          <encoding>UTF-8</encoding> <!-- 指定项目编码格式 -->
        </configuration>
      </plugin>
    </plugins>
  </build>
```

#### Tomcat插件  
如果使用maven的tomcat插件的话，那么本地则不需要安装tomcat。  
```xml
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.3.2</version>
        <configuration>
          <port>8080</port>
          <path>/</path>
        </configuration>
      </plugin>
    </plugins>
  </build>
```
使用tomcat7来运行web工程，它的命令是：tomcat7:run  

#### 设定插件仓库  
```xml
<pluginRepositories>
    <pluginRepository>
        <id>repos</id>
        <name>Repository</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </pluginRepository>
</pluginRepositories>
```

### 继承  
在maven中的继承，指的是pom文件的继承  
* 创建父工程  
![创建父工程][parent]  
* 创建子工程  
![创建子工程][son]  
子工程的pom文件  
```xml
  <parent>
    <groupId>com.exercise.parent</groupId>
    <artifactId>MavenParent</artifactId>
    <version>0.0.1-SNAPSHOT</version>
  </parent>
```
* 父工程统一依赖jar包  
在父工程中对jar包进行依赖，在子工程中都会继承此依赖。 
* 父工程统一管理版本号  
dependencyManagement标签管理的依赖，其实没有真正依赖，它只是管理依赖的版本  

### 聚合  
在真实项目中，一个项目有表现层、业务层、持久层，对于业务层和持久层，它们可以在多个工程中被使用，所以一般会将业务层和持久单独创建为java工程，为其他工程依赖。  
* 创建聚合工程，packaging：pom，与创建继承中的父工程类似  
* 创建Module（Controller，Service）  
New --> Maven Module  

## Nexus  
nexus是一种常见的maven私服软件，为所有来自中央仓库的构建安装提供本地缓存  

### 下载及安装  
* 下载  
下载地址：[http://maven.apache.org][nexus]{:target="_blank"}  
下载好以后解压会有两个文件夹：nexus的和sonatype-work。前者是功能的实现，后者负责存储数据。  
* 安装  
进入nexus的bin目录下，以管理员身份运行以下命令：  
安装：`nexus.exe /install`  
卸载：`nexus.exe /unistall`  
启动：`nexus.exe /start`  
停止：`neuxs.exe /stop`  

### 配置  
启动后，直接本地访问http://localhost:8081  
登录用户名、密码为 admin admin123  
只有在登陆上去之后，才能进行进一步的配置  
在etc下有一个nexus-defauly.properties文件，是nexus的配置文件  

### 主要仓库  
* maven-central(proxy)：这是maven的中心仓库，nexus这里只是做一个代理，最后会转发到maven的central库  
* maven-public(group)：这是一个仓库组，默认包含maven-central、maven-releases、maven-snapshots  
* maven-releases(hosted)：这是nexus提供的一个默认的存放release版本jar包的仓库  
* maven-snapshots(hosted)：这是nexus提供的一个默认的存放snapshot版本jar包的仓库  

### 仓库类型  
* group(仓库组)：一组仓库的集合  
* hosted(宿主)：配置第三方仓库 （包括公司内部私服 ）  
* proxy(代理)：私服会对中央仓库进行代理，用户连接私服，私服自动去中央仓库下载jar包或者插件  
* virtual(虚拟)：兼容Maven1 版本的jar或者插件  

### 上传jar包  
* maven的settings.xml配置  
```xml
  <servers>
    <server>
        <id>release_user</id>
        <username>admin</username>
        <password>admin123</password>
    </server>
    <server>
        <id>snapshot_user</id>
        <username>admin</username>
        <password>admin123</password>
    </server>
  </servers>
```
这里配置两个用户，一个部署release类型jar包的，一个是部署snapshot类型jar包的。  
id用于唯一指定一条认证配信息，之后会在pom中使用。  
* 项目pom配置  
```xml
  <distributionManagement>
    <repository>
      <id>release_user</id>
      <name>Release Deploy</name>
      <url>http://localhost:8081/repository/maven-releases/</url>
    </repository>
    <snapshotRepository>
      <id>snapshot_user</id>
      <name>Snapshot Deploy</name>
      <url>http://localhost:8081/repository/maven-snapshots/</url>
    </snapshotRepository>
  </distributionManagement>
```
这里配置了上传的url，具体的url可以在nexus的仓库浏览界面下点击仓库的url copy获得。使用刚才的两个认证信息，把jar包存在nexus提供的默认仓库下。id对应了setting.xml里配置的信息，name随意。  
运行`mvn clean deploy`即可部署到nexus（在eclipse中直接运行`clean deploy`）  

### 拉取jar包  
* pom文件配置单个项目  
```xml
    <repositories>
        <repository>
            <id>nexus-public</id>
            <name>Nexus Public</name>
            <url>http://localhost:8081/repository/maven-public/</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </repository>
    </repositories>
```
* 也可在settings.xml中配置所有项目  
```xml
<mirrors>
    <mirror>
        <!--此处配置所有的构建均从私有仓库中下载 *代表所有，也可以写central -->
        <id>nexus</id>
        <mirrorOf>*</mirrorOf>
        <url>http://localhost:8080/nexus-2.7.0-06/content/groups/public/</url>
    </mirror>
</mirrors>

```

## 其他属性

### properties标签，可用于管理版本  
```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <mysql.driver.version>5.1.30</mysql.driver.version>
    <spring.version>4.1.9.RELEASE</spring.version>
    <mybatis.version>3.2.8</mybatis.version>
    <mybatis-spring.version>1.2.3</mybatis-spring.version>
    <shiro.version>1.3.2</shiro.version>
    <jackson.version>2.2.3</jackson.version>
</properties>
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>${spring.version}</version>
    </dependency>
```

### repositories标签，设定主仓库，按顺序进行查找  
```xml
<repositories>
    <repository>
        <id>repos</id>
        <name>Repository</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </repository>
</repositories>
```


[maven_web]:http://maven.apache.org "Maven官网"
[nexus]:https://www.sonatype.com/download-oss-sonatype "nexus下载地址"
[maven_process]:img/maven构建过程.jpg "maven构建过程"
[scope]:img/scope.jpg "Maven依赖范围"
[dependency]:img/dependency.png "Maven依赖关系"
[scope_dependency]:img/scope_dependency.jpg "Maven依赖范围传递"
[parent]:img/parent.png "创建父工程"
[son]:img/son.png "创建子工程"