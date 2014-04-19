---
layout: default
title:dubbo源码编译 
---

首先从wget https://github.com/alibaba/dubbo/archive/master.zip下载最新源码，目前最新版本为2.5.4-SNAPSHOT

    解压文件  
    cd dubbo  
    mvn clean install -Dmaven.test.skip  
    
报错：Non-resolvable parent POM: Could not transfer artifact com.alibaba:opensesame:pom:2.0 from/to opensesame.releases (http://code.alibabatech.com/mvn/releases)


README上已经说明了需要本地安装“由于开源站点因为安全问题被下掉，如果编译时出现找不到opensesame依赖情况的，请先手动下载<a href='https://github.com/alibaba/opensesame'>https://github.com/alibaba/opensesame</a>”，那就自己安装吧
下载opensesame，之后进入opensesame目录，执行：mvn install 等待success吧

再次执行：  

    mvn clean install -Dmaven.test.skip
    
又是一堆的错误啊：
[ERROR] Failed to execute goal on project dubbo-common: Could not resolve dependencies for project com.alibaba:dubbo-common:jar:2.5.4-SNAPSHOT: Failed to collect dependencies for [org.slf4j:slf4j-api:jar:1.6.2 (provided), commons-logging:commons-logging-api:jar:1.1 (provided), log4j:log4j:jar:1.2.16 (compile), org.javassist:javassist:jar:3.15.0-GA (compile), com.alibaba:hessian-lite:jar:3.2.1-fixed-2 (compile), com.alibaba:fastjson:jar:1.1.8 (provided), org.jvnet.sorcerer:sorcerer-javac:jar:0.8 (provided), cglib:cglib-nodep:jar:2.2 (test), junit:junit:jar:4.10 (test), org.easymock:easymock:jar:3.0 (test), org.easymock:easymockclassextension:jar:3.0 (test), com.googlecode.jmockit:jmockit:jar:0.999.8 (test)]: Failed to read artifact descriptor for com.alibaba:hessian-lite:jar:3.2.1-fixed-2: Could not transfer artifact com.alibaba:hessian-lite:pom:3.2.1-fixed-2 from/to opensesame.releases (http://code.alibabatech.com/mvn/releases): Connection to http://code.alibabatech.com refused: Connection refused -> [Help 1]

很多文件从阿里的仓库中都找不到了，到处找解决方法啊。在这个贴子上有提到了更改配置仓库：<a   href="https://github.com/alibaba/dubbo/issues/22">https://github.com/alibaba/dubbo/issues/22</a>

    <mirror>
    <id>kafeitu</id>
    <mirrorOf>central</mirrorOf>
    <name>Human Readable Name for this Mirror.</name>
    <url>http://maven.kafeitu.me/nexus/content/repositories/public</url>
    </mirror>
    <mirror>
    <id>ibiblio.org</id>
    <name>ibiblio Mirror of http://repo1.maven.org/maven2/</name>
    <url>http://mirrors.ibiblio.org/pub/mirrors/maven2</url>
    <mirrorOf>*</mirrorOf>
    </mirror>
    <mirror>
    <id>lvu.cn</id>
    <name>lvu.cn</name>
    <url>http://lvu.cn/nexus/content/groups/public</url>
    <mirrorOf>*</mirrorOf>
    </mirror>

将上面的配置加入maven配置文件setting.xml中

再次执行： 

    mvn clean install -Dmaven.test.skip  
    
依然报错：
[ERROR] Failed to execute goal on project dubbo-common: Could not resolve dependencies for project com.alibaba:dubbo-common:jar:2.5.4-SNAPSHOT: Failed to collect dependencies for [org.slf4j:slf4j-api:jar:1.6.2 (provided), commons-logging:commons-logging-api:jar:1.1 (provided), log4j:log4j:jar:1.2.16 (compile), org.javassist:javassist:jar:3.15.0-GA (compile), com.alibaba:hessian-lite:jar:3.2.1-fixed-2 (compile), com.alibaba:fastjson:jar:1.1.8 (provided), org.jvnet.sorcerer:sorcerer-javac:jar:0.8 (provided), cglib:cglib-nodep:jar:2.2 (test), junit:junit:jar:4.10 (test), org.easymock:easymock:jar:3.0 (test), org.easymock:easymockclassextension:jar:3.0 (test), com.googlecode.jmockit:jmockit:jar:0.999.8 (test)]: Failed to read artifact descriptor for com.alibaba:hessian-lite:jar:3.2.1-fixed-2: Could not find artifact com.alibaba:opensesame:pom:1.0 in ibiblio.org (http://mirrors.ibiblio.org/pub/mirrors/maven2) -> [Help 1]

在这个错误文件中发现了Could not find artifact com.alibaba:opensesame:pom:1.0，不是2.0么，怎么1.0也要。既然需要，那咱就给呗。
还记得上面自己下载的opensesame源码不，修改下面的pom.xml，将 <version>2.0</version>中2.0修改为1.0
执行 mvn install 等待success吧

再次执行: 

    mvn clean install -Dmaven.test.skip
    
依然出错
[ERROR] Failed to execute goal on project dubbo-common: Could not resolve dependencies for project com.alibaba:dubbo-common:jar:2.5.4-SNAPSHOT: Could not find artifact com.alibaba:fastjson:jar:1.1.8 in ibiblio.org (http://mirrors.ibiblio.org/pub/mirrors/maven2) -> [Help 1]

不过看到那么多的错误一下便少了，心理还是暗爽的，哈哈。  
到http://maven.kafeitu.me/nexus/content/repositories/public/这个上面看了下fastjson可用版本有1.1.39，于是修改pom.xml,找到 <fastjson_version>1.1.8</fastjson_version>，将1.1.8修改为1.1.39

再次执行：  

    mvn clean install -Dmaven.test.skip  
    
我靠还是有错，这次居然是  
    java.lang.OutOfMemoryError: PermGen space  
就是不让人省心啊，继续解决，采用粗暴直接的方式，直接修改maven安装目录下bin/mvn，在上面加上这个  
    
    MAVEN_OPTS="$MAVEN_OPTS -Xms256m -Xmx1024m -XX:MaxPermSize=128m -XX:ReservedCodeCacheSize=64m" 

具体大小，可根据自己的情况调整

再次执行：

    mvn clean install -Dmaven.test.skip
    
经过一段漫长的过程，终于看到了build success

好吧，至此整个源码的编译过程已经完成，本身没有什么难度，只是因为缺少相关的jar和环境配置的一些问题，导致一系列的问题。

<p>{{ page.date | date_to_string }}</p>

