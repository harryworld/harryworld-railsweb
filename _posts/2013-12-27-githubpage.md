---
layout: post
title: "如何使用Github的page做博客"
description: ""
category: ""
tags: ["blog"]
comments: true
---

之前一直想在github上面弄个blog，但是总觉得很麻烦，今天抽了点时间整了一下，还是蛮顺利的

###1.在Github上面创建你的Repository

你需要创建一个```#{username}.github.com```的项目

我们借助[Jekyll-Bootstrap](http://jekyllbootstrap.com/)，这是一个基于Jekyll的解析引擎，支持模块化的主题(modular theming)。

可以参考我下面的做法

```
git clone https://github.com/plusjade/jekyll-bootstrap.git sanvibyfish.github.io
cd sanvibyfish.github.io
git remote set-url origin https://github.com/sanvibyfish/sanvibyfish.github.com.git
git add *
git commit -m "first commit"
git push -u origin master
```

###安装Jekyll

Jekyll是基于Ruby的，所以清楚的话请Google 怎么使用Ruby Gem

```
gem install jekyll
```

###使用主题
这里我使用的是一款叫做Hyde的主题

```
git clone https://github.com/mdo/hyde/
```
把里面的文件覆盖你自己的文件


###如何使用
我们可以通过Rakefile使用命令

```
rake post title="hi"
```

>记得把jekyll-bootstrap的Rakefile复制过来修改


###加入Disqus
- 首先去[Disqus](disqus.com) 申请个账户，并绑定个域名

- 在***_includes***的文件夹里面新建***disqus.html***

```

 <div id="disqus_thread"></div>

 <script type="text/javascript">
    /* * * CONFIGURATION VARIABLES: EDIT BEFORE PASTING INTO YOUR WEBPAGE * * */
var disqus_shortname = 'sanvibyfish'; // required: replace example with your forum shortname

/* * * DON'T EDIT BELOW THIS LINE * * */
(function() {
 var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
 dsq.src = 'http://' + disqus_shortname + '.disqus.com/embed.js';
 (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
 })();
</script>
 <noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
 <a href="http://disqus.com" class="dsq-brlink">blog comments powered by <span class="logo-disqus">Disqus</span></a>

```

- 在***_layout***的文件下的***post.html***插入相关include代码



