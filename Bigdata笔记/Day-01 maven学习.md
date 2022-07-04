# Day-01 maven学习

### 1、依赖的原则：解决jar包冲突

**1）路径最短者优先**

**2）路径相同时先声明者优先**

这里“声明”的先后顺序指的是**dependency标签配置的先后顺序**。



### 2、继承 

1) 子工程继承父工程，在pom文件中添加

```xml
<!--继承-->
<parent>
    <groupId>com.atguigu.maven</groupId>
    <artifactId>Parent</artifactId>
    <version>1.0-SNAPSHOT</version>
<!--指定从当前pom.xml文件出发寻找父工程的pom.xml文件的相对路径-->
<relativePath>../Parent/pom.xml</relativePath>
</parent>
```

此时如果子工程的<groupId>和<version>标签，和父工程重复则可以删除。

2) 父工程管理依赖：将Parent项目中的<dependencies>标签，用<dependencyManagement>标签括起来

```xml
<!--依赖管理-->
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.0</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

如果在子项目中重新指定需要的依赖，***删除范围和版本号***

```xml
<!--依赖管理-->
<dependencyManagement>
    <dependencies>
		<dependency>
    		<groupId>junit</groupId>
    		<artifactId>junit</artifactId>
		</dependency>
    </dependencies>
</dependencyManagement>
```

并且

![](C:\Users\14864\Desktop\Bigdata笔记\picture\Snipaste_2022-07-04_01-33-42.png)

### 3、聚合

在父工程中pom文件里添加<modules>和<module>标签

```xml
<!--聚合-->
<modules>
    <module>../MakeFriend</module>
    <module>../OurFriends</module>
    <module>../HelloFriend</module>
    <module>../Hello</module>
</modules>
```



### 4、继承和聚合总结

继承注重于从父工程获取资源，聚合注重于统一管理多个子工程。



### 5、依赖管理

依赖范围：scope

```java
	    -- compile（默认就是这个范围）
				（1）main目录下的Java代码可以访问这个范围的依赖
				（2）test目录下的Java代码可以访问这个范围的依赖
				（3）部署到Tomcat服务器上运行时要放在WEB-INF的lib目录下
				例如：对Hello的依赖。主程序、测试程序和服务器运行时都需要用到。
	    -- test 
		    	（1）main目录下的Java代码不能访问这个范围的依赖
				（2）test目录下的Java代码可以访问这个范围的依赖
				（3）部署到Tomcat服务器上运行时不会放在WEB-INF的lib目录下
				例如：对junit的依赖。仅仅是测试程序部分需要。
			
		-- provided
		    	（1）main目录下的Java代码可以访问这个范围的依赖
				（2）test目录下的Java代码可以访问这个范围的依赖
				（3）部署到Tomcat服务器上运行时不会放在WEB-INF的lib目录下
				例如：servlet-api在服务器上运行时，Servlet容器会提供相关API，
			      所以部署的时候不需要。
```