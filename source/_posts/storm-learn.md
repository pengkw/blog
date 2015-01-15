title: storm学习
date: 2014-11-19 03:39:23
tags: storm
categories: storm
description: 主要介绍storm环境的搭建以及发现好的博客
---

## 了解storm
近期打算看看storm，于是搭建了环境来试试。主要记录搭建的过程，以备之后需要。
[storm github](https://github.com/apache/storm)上有源码，也可以到[官网](http://storm.apache.org/documentation/Home.html)上下载编译后的。
可以到[这个博客](http://xumingming.sinaapp.com/category/storm/)看看相关的中文文档，非常详细。


## 搭建环境
虽然上面那个博客有介绍环境的搭建，但是感觉不够详细，推荐看这个[博客](http://idoall.org/home.php?mod=space&uid=1&do=blog&id=546)

## 问题
1. 编译的时候，有些依赖的jar找不到，更改maven仓库地址重新编译。http://repo.gozap.com/repos/ https://clojars.org/repo/
2. 运行storm-starter出现java.lang.NoSuchMethodError: org.yaml.snakeyaml.Yaml错误，修改pom文件
``` bash
 <dependency>
      <groupId>org.testng</groupId>
      <artifactId>testng</artifactId>
      <version>6.8.5</version>
      <scope>test</scope>
      <!--增加这部分避免该错误-->
      <exclusions>
          <exclusion>
              <artifactId>snakeyaml</artifactId>
              <groupId>org.yaml</groupId>
          </exclusion>
       </exclusions>
    </dependency>
```
