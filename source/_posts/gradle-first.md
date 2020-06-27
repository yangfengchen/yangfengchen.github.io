---
title: gradle初步配置使用
categories:
  - java
date: 2020-06-27 17:04:24
tags:
---

​	在某篇文章上看到spring boot后续开始使用gradle，心血来潮准备多学一手压身，暂时没有时间翻《gradle实战》，为防止老人痴呆症特此记录。

# 1、官网下载gradle

​	https://gradle.org/

​	官网已经有详细的下载安装说明

# 2、配置环境变量

​	GRADLE_HOME: gradle跟目录

​	PATH:%GRADLE_HOME%\bin

​	GRADLE_USER_HOME:gradle 下载包目录(个人不喜欢东西放c盘,所以习惯性配置这个，不配置默认在c盘user的某个目录)

# 3、idea 使用gradle

​	settings->build,execution,deployment->gradle

​	3.1、use auto-import //勾选（或者创建项目时勾选），习惯了自动导入添加包

​	3.2、delegate settings 两个都选择idea//如果不选择，单独建立一个测试main方法运行是提示错误

​	3.3、service directory path// 设置下载包路径

# 4、build.gradle配置

```java
sourceCompatibility = 1.8// source使用的jdk版本
targetCompatibility = 1.8// 编译时使用的jdk版本或者更新的java虚拟机兼容
compileJava.options.encoding = 'UTF-8'
compileTestJava.options.encoding = 'UTF-8'


repositories {
    mavenLocal()
    maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
    mavenCentral()
    jcenter()
    maven { url "https://repo.spring.io/snapshot" }
    maven { url "https://repo.spring.io/milestone" }
    maven { url 'http://oss.jfrog.org/artifactory/oss-snapshot-local/' }  //转换pdf使用
}

dependencies {
    compile 'org.apache.commons:commons-lang3:3.8.1'
    compile 'com.google.guava:guava:29.0-jre'
    testCompile group: 'junit', name: 'junit', version: '4.12'
}
```

​	mavenLocal()：指定使用maven本地仓库，而本地仓库在配置maven时setting文件指定的仓库位置。gradle查找jar包顺序如下：`gradle默认会按以下顺序去查找本地的仓库：USER_HOME/.m2/settings.xml >> M2_HOME/conf/settings.xml >> USER_HOME/.m2/repository。` 

mavenCentral()：这是Maven的中央仓库，无需配置，直接声明就可以使用 
jcenter():JCenter中央仓库，实际也是是用的maven搭建的，但相比Maven仓库更友好，通过CDN分发，并且支持https访问。 

gradle按配置顺序寻找jar文件。如果本地存在就不会再去下载。不存在的再去maven仓库下载，这里注意下载下来的jar文件不在maven仓库里，而是在gradle的主工作目录下。

# 5、扩展知识：

1、gradle下载包根目录建立gradle.properties文件，内容如下：

#开启线程守护，第一次编译时开线程，之后就不会再开了
org.gradle.daemon=true
#配置编译时的虚拟机大小
org.gradle.jvmargs=-Xmx1024m -XX:MaxPermSize=512m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8
#开启并行编译，相当于多条线程再走
org.gradle.parallel=true
org.gradle.configureondemand=true

2、依赖第三方包（可以依赖本地包）

**依赖本地包**

compile fileTree(dir:'/lib',include:['*.jar'])

**compile**

从依赖上讲，用compile修饰的配置会传递依赖，而大多数的依赖冲突都是由compile产生的，什么是传递依赖？
 打个比方：我们现在有libA，然后libB用compile依赖libA，最后libC依赖libB。那这个时候，libC自然能够使用libA的内容，因为libA的内容跟随这个libB而传递到了libC中。

**api**

api是Gradle4.1(Android studio3.0)新增的依赖方式,其作用于compile基本一致。

testcompile和androidTestcompile

这两个依赖项配置和compile是差不多的，也会产生传递依赖，唯一不同的是，testcompile和androidTestcompile不会参与源码打包，只会参与测试包的打包，并且只有在测试模式下启动才会生效，debug和release包不生效。

**debugCompile和releaseCompile**

- debugCompile
   只在buildType为debug的时候参与打包，release不参与打包，比方说我们的内存泄露检测工具-LeakCanary，其实我们也只是需要在debug模式下打包调试，而发布release版本就需要进行打包了，所以用debugCompile来进行配置
- releaseCompile
   releaseCompile和debugCompile完全相反，只在release模式下参与打包，应用场景不是很多。

**implementation**

implementation 是Gradle4.1(Android studio3.0)新增的依赖方式，implementation和compile不同，该依赖方式不会产生传递依赖，implementation有点像provided和、debugCompile和releaseCompile的集合体。
 来个具体场景，例如：有libA公共库，libB通过implementation依赖libB，然后app无论通过什么方式依赖libB，lib1的依赖都不会传递过来，必须要在app中重新依赖一次。



如有涉及侵权请及时联系，谢谢~~ QQ：377681869 

参考网站：

https://www.cnblogs.com/wangsongbai/p/9206940.html

https://www.jianshu.com/p/a69daa09a218