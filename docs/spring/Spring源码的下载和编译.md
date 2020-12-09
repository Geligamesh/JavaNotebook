

## 什么是 Spring 框架?

Spring 是一种轻量级开发框架，旨在提高开发人员的开发效率以及系统的可维护性。Spring 官网：https://spring.io/。

我们一般说 Spring 框架指的都是 Spring Framework，它是很多模块的集合，使用这些模块可以很方便地协助我们进行开发。这些模块是：核心容器、数据访问/集成,、Web、AOP（面向切面编程）、工具、消息和测试模块。比如：Core Container 中的 Core 组件是Spring 所有组件的核心，Beans 组件和 Context 组件是实现IOC和依赖注入的基础，AOP组件用来实现面向切面编程。

Spring 官网列出的 Spring 的 6 个特征:

- **核心技术** ：依赖注入(DI)，AOP，事件(events)，资源，i18n，验证，数据绑定，类型转换，SpEL。
- **测试** ：模拟对象，TestContext框架，Spring MVC 测试，WebTestClient。
- **数据访问** ：事务，DAO支持，JDBC，ORM，编组XML。
- **Web支持** : Spring MVC和Spring WebFlux Web框架。
- **集成** ：远程处理，JMS，JCA，JMX，电子邮件，任务，调度，缓存。
- **语言** ：Kotlin，Groovy，动态语言。

## Spring的一些比较重要的模块：

![Spring主要模块](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-6/Spring%E4%B8%BB%E8%A6%81%E6%A8%A1%E5%9D%97.png)

- **Spring Core：** 基础,可以说 Spring 其他所有的功能都需要依赖于该类库。主要提供 IoC 依赖注入功能。
- **Spring Aspects** ： 该模块为与AspectJ的集成提供支持。
- **Spring AOP** ：提供了面向切面的编程实现。
- **Spring JDBC** : Java数据库连接。
- **Spring JMS** ：Java消息服务。
- **Spring ORM** : 用于支持Hibernate等ORM工具。
- **Spring Web** : 为创建Web应用程序提供支持。
- **Spring Test** : 提供了对 JUnit 和 TestNG 测试的支持。

好，那么介绍到这里差不多该进入正题了,首先我们进入GitHub，下载Spring的源代码，这里我们选择下载Spring-5.2.0，直接下载zip包就行，因为我们没必要提交代码所以直接下载就行

![yqnjDU](https://my-images-bed.oss-cn-hangzhou.aliyuncs.com/uPic/yqnjDU.png)

下载完毕之后我们发现Spring框架是使用gradle进行构建的，因此我们还需要下载一个gradle ,官网地址：https://gradle.org/，我这边下载最新版本的gradle-6.6.1，下载好gradle之后接着配置一下它的环境变量，在.bash_profile文件中输入如下命令：

```shell
#GRADLE
GRADLE_HOME=/usr/local/gradle-6.6.1
PATH=$PATH:$GRADLE_HOME/bin
export GRADLE_HOME GRADLE_USER_HOME PATH
```

最后别忘了***source  .bash_profile***一下，下载完Spring源码和gradle工具之后，我们使用InteliJ Idea来打开Spring工程，**File -> New -> Project from Existing Sources **， 导入完成之后，gradle开始编译整个工程，编译过程中gradle会根据Spring工程目录下的**gradle/wrapper/gradle-wrapper.properties**文件中的distributionUrl去重新下载一个5.6.2版本的gradle，这是Spring官方推荐的gradle版本

```shell
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-5.6.2-bin.zip
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists

```

当然你可以改成使用自己本地目录下的gradle，不用gradle-wrapper.properties配置文件

![VuTZsA](https://my-images-bed.oss-cn-hangzhou.aliyuncs.com/uPic/VuTZsA.png)但是我推荐还是老老实实下载下来吧，因为如果使用过高的版本可能在编译的时候会出现一些依赖问题，比说下面这种情况，百度说是`kotlin`版本不一致导致的，所以为了不出现奇奇怪怪的问题还是老老实实下载下来比较好。

![s8Gcpz](https://my-images-bed.oss-cn-hangzhou.aliyuncs.com/uPic/s8Gcpz.png)

编译完成之后我们就可以测试代码了

![WbiO8E](https://my-images-bed.oss-cn-hangzhou.aliyuncs.com/uPic/WbiO8E.png)

我们在Spring根目录下创建一个模块，使用gradle构建，点击Next，命名一个spring-demo的模块

![cqkSqr](https://my-images-bed.oss-cn-hangzhou.aliyuncs.com/uPic/cqkSqr.png)

创建完成之后，我们发现gradle构建的工程结构跟maven构建的工程结构是基本一样的，唯一区别就是gradle工程的配置文件是build.gradle，maven的是pom.xml，gradle工程的build.gradle就相当于maven的pom.xml。相比之下maven的配置文件是XML格式的，假如你的项目依赖的包比较多，那么XML文件就会变得非常长，而且XML文件不够灵活，假如需要在构建过程中添加一个自定义的逻辑，搞起来非常麻烦，gradle就显得更加灵活，清晰明了。

![HQZGlF](https://my-images-bed.oss-cn-hangzhou.aliyuncs.com/uPic/HQZGlF.png)

接下来我们测试一下spring源码是否可以正常使用，我们在spring-demo模块下的build.gradle目录下添加spring-context依赖

![fDuMRI](https://my-images-bed.oss-cn-hangzhou.aliyuncs.com/uPic/fDuMRI.png)

然后定义一个WelcomeService接口

```java
public interface WelcomeService {

	String sayHello(String name);
}
```

再创建一个WelcomeServiceIml实现它的接口

```java
public class WelcomeServiceImpl implements WelcomeService {
	@Override
	public String sayHello(String name) {
		System.out.println("welcome " + name);
		return "return success!";
	}
}
```

在resouces目录下创建一个名为spring的目录，然后在spring目录下再创建一个名为spring-config,xml的核心配置文件，并定义一个bean

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="welcomeService" class="com.gxb.service.impl.WelcomeServiceImpl">
	</bean>
</beans>
```

![zWRlcw](https://my-images-bed.oss-cn-hangzhou.aliyuncs.com/uPic/zWRlcw.png)

创建一个Entrance类，加载spring配置文件，将bean加载进Ioc容器

```java
public class Entrance {

	public static void main(String[] args) {
		System.out.println("hello spring");
		String classPath = "spring/spring-config.xml";
		ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext(classPath);
		WelcomeService welcomeService = (WelcomeService) applicationContext.getBean("welcomeService");
		System.out.println(welcomeService.sayHello("gxb"));
	}
}
```

执行main方法，BUILD SUCCESSFUL ，大功告成，接下来就可以开始边调试边研读源码了

![F3vcYL](https://my-images-bed.oss-cn-hangzhou.aliyuncs.com/uPic/F3vcYL.png)

