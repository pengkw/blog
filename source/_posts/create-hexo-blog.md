title: 使用hexo、github搭建免费博客
date: 2014-11-13 18:28:50
tags: hexo
description: 使用hexo、github搭建免费博客，将hexo环境搭建在云端，可实现不同环境下编写，所需要的只是一个浏览器即可
---


折腾了这么久，总算告一段落。重新整理下建立这个博客的步骤。如果你也有这种不同环境写作的需求，可以参考本文。

## 注册[nitrous](https://www.nitrous.io/)  
注册过程比较简单，不多说。

## nitrous上配置hexo环境
注册登录成功，进入操作面板，如下所示  
![](/img/content/nitrous_wk.png)

### 安装hexo环境
``` bash
	
npm install hexo -g
```

### 创建博客目录
``` bash

hexo init blog
```

### 进入博客目录
``` bash

cd blog
```

### 安装依赖
``` bash

npm install
```

## 准备github环境

### 注册github帐号
到[github](https://github.com)上注册帐号

### 在github上新建一个仓库（repository）
仓库的名字为：yourname.github.io，例如我的就是pengkw.github.io

## hexo与github关联
### 修改配置
打开nitrous，修改blog目录下的_config.yml，找到deploy配置
``` bash

deploy:
  type: github
  repo: https://github.com/pengkw/pengkw.github.io.git
```
只需要将repo的地址改成你自己的即可

### 配置github帐号
``` bash

git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```

### 生成文章
``` bash

hexo generate
```

### 发布到github
``` bash

hexo deploy
```
过程中会提示输入github的帐号和密码，输入正确即可。

## 查看成果
访问yourname.github.io看看效果吧

## 总结
这里只是简单的搭建起整个博客，至于如何更好的使用hexo网上已经有了很多的参考资料，可以自行搜索，自己定制。


搭建过程中的心酸：  
这个博客的搭建过程参考文章  
[Hexo搭建免费静态博客](http://aceking10.github.io/2014/11/11/hexo-handbook/)  
[Hexo在github上构建免费的Web应用](http://blog.fens.me/hexo-blog-github/)
[hexo你的博客](http://ibruce.info/2013/11/22/hexo-your-blog/)

博客搭建之后我就发现如果我需要在家和公司都可以写的话，需要又整一套环境，虽然搭建过程不复杂，但是还是觉得不够方便，于是我就想能不能有其他的途经来解决这个问题。随手问了下google，就找到了[这篇文章](http://segmentfault.com/a/1190000000452627)，经测试，网络连接不是很好，有些慢，暂时先这样处理，有好的替代方案继续试用。

基于这个思路，重新有找了一个在线vm [nitrous](https://www.nitrous.io/)，nodejs、git的环境都有，只需要安装hexo即可。



