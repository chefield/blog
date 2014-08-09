## 使用*github pages* 

###Github Pages 是什么？

如果你对编程有所了解，就一定听说过[github](https://github.com/)。它号称程序员的Facebook，有着极高的人气，许多重要的项目都托管在上面。
简单说，它是一个具有版本管理功能的代码仓库，每个项目都有一个主页，列出项目的源文件。

但是对于一个新手来说，看到一大堆源码，只会让人头晕脑涨，不知何处入手。他希望看到的是，一个简明易懂的网页，说明每一步应该怎么做。因此，github就设计了[Pages](http://pages.github.com/)功能，允许用户自定义项目首页，用来替代默认的源码列表。**所以，github Pages可以被认为是用户编写的、托管在github上的静态网页。**

github提供模板，允许[站内生成](https://help.github.com/articles/creating-pages-with-the-automatic-generator)网页，但也允许用户自己编写网页，然后上传。有意思的是，这种上传并不是单纯的上传，而是会经过Jekyll程序的再处理。

###Jekyll是什么？

Jekyll（发音/'dʒiːk əl/，"杰克尔"）是一个静态站点生成器，它会根据网页源码生成静态文件。它提供了模板、变量、插件等功能，所以实际上可以用来编写整个网站。

整个思路到这里就很明显了。你先在本地编写符合Jekyll规范的网站源码，然后上传到github，由github生成并托管整个网站。

####这种做法的好处是：

- 免费，无限流量。
- 享受git的版本管理功能，不用担心文章遗失。　　
- 你只要用自己喜欢的编辑器写文章就可以了，其他事情一概不用操心，都由github处理。

####它的缺点是：

- 有一定技术门槛，你必须要懂一点git和网页开发。
- 它生成的是静态网页，添加动态功能必须使用外部服务，比如评论功能用[duoshuo](http://duoshuo.com/)。
- 它不适合大型网站，因为没有用到数据库，每运行一次都必须遍历全部的文本文件，网站越大，生成时间越长。

但是，综合来看，它不失为搭建中小型Blog或项目主页的最佳选项之一。

###一个实例

通常有两种方式在github上建立[blog](https://pages.github.com/)，user site和project site。

####user site

#####在windows下安装ruby环境。

推荐安装[RailsInstaller](http://railsinstaller.org/)，里面包含了[Ruby](http://ruby-lang.org/)、[Rails](http://rubyonrails.org/)、[Bundler](http://gembundler.com/)、[Git](http://git-scm.com/)、[Sqlite](http://sqlite.org/)、[TinyTDS](https://github.com/rails-sqlserver/tiny_tds)、[SQL Server support](https://github.com/rails-sqlserver/activerecord-sqlserver-adapter)和[DevKit](https://github.com/oneclick/rubyinstaller/wiki/Development-Kit)。

不过最近的RailsInstaller里包含的ruby版本升到了1.9.3，如果以后要使用[Octopress](http://www.octopress.org/)的话必须使用ruby1.9.2，建议使用以前的版本，以前的版本在[这里](http://inwake.com/ypchen/files/upload/railsinstaller-2.0.1.exe)。

#####配置git和github

在RailsInstaller安装结束时安装程序会提示是否配置Git环境（这样的话给配置git和github带来极大的方便，又减少了几条命令）。选择”是”。

填写github注册时的用户名和邮箱，就完成了公钥和密钥的生成，在`C:\Documents and Settings\`用户名下，有个隐藏目录名为.ssh，id_rsa.pub文件就是公钥，id_rsa就是密钥。

在Github网站找到 “Account Settings” > Click “SSH Keys” > Click “Add SSH key”



用文本编辑器打开id_rsa.pub文件，并把里面的内容（包括空格和新行）复制出来，填到Github的账户设置的SSH设置里。



在开始菜单里找到RailsInstaller –> Git Bash，执行它，就打开了下面的命令窗口，以后的操作都是在这个窗口下进行的。



注意: 在窗口里我们可以看到当前路径显示为/c/Sites，其实它表示的是C:\Sites，这个目录是RailsInstaller在安装的时候建的。

用下面的命令测试一下git是否连接正常

```

 ssh -T git@github.com

```

#####安装jekyll和相关的包

稍微对配置做一下修改，把淘宝的镜像加到gem的镜像列表里

```

gem sources --remove http://rubygems.org/
gem sources -a http://ruby.taobao.org/

```

然后用gem sources -l看看现在源列表

```

*** CURRENT SOURCES ***

http://ruby.taobao.org

```

如果是上面这样就可以安装jekyll了

```

gem install jekyll

```

Jekyll需要用到directory_watcher、liquid、open4、maruku和classifier这几个包，用上面的命令可以自动安装。Jekyll默认用maruku来解析markdown语言，你也可以用别的程序来解析，比如rdiscount或kramdown，都给装上吧：

```

gem install rdiscount kramdown

```

以上命令涉及到gem install的时候，如果你用的是linux系统，就要用sudo gem install代替。

#####建立github pages

这一步是本文的重点，也是本文异于网络上其他文章的地方，我在这里用到了Github提供的Github pages generator的功能，减少了使用的命令数量，也绕开了远程代码库这个概念（省略了与git remote相关的操作，不过随着github使用的加深，这些概念也是不能避免的）

在github.com上创建代码库，比如新建一个名为example的代码库：登录到自己的Github账户，选择New repository






在线生成pages: 点上图中的Admin




接下来的页面可以不用填，直接点Create Page，马上会转到一个404页面，不要慌，要过一会系统才会帮你把网页生成好。


克隆自己的代码库
```
  git clone git@github.com:yanping/example.git
```
这样git会把存放在github上的代码库文件下载到本地的，生成名为example的目录。删除该目录下的index.html，这是系统生成的，不是我想要的页面，注意不要把.git目录删除，这是个隐藏目录，里面包含这个代码库的配置信息，以上的步骤都是为了得到这些配置信息且避免了使用命令。

克隆别人的代码库。在本地另一位置，克隆别人的代码库，比如
```
  git clone git@github.com:mojombo/mojombo.github.com.git
```
删掉.git目录，然后把文件都复制到自己的本地代码库example下

删除_post下的文件（可保留一两篇作为模板），修改example里的文件，尤其是配置信息，比如_config.yml、disqus的配置，CNAME文件等，更进一步，按照自己的喜好修改网页的布局和样式，这些都可在后期慢慢摸索。然后执行下面的操作

-git add .表示添加当前目录下的所有文件

-git commit -am "message" 表示提交所有更改，这是提交到本地，”message” 换成自己的注释信息

-git push 把在本地的更改提交到远程服务器

要写博客的时候，在_post里新建一个markdown文件，文件名和文件里面的头部信息（学名叫YAML front matter）按照模板的格式改，编辑好内容后，再依次执行上面三条命令。

如果你不熟悉markdown语法，请看[这里](http://wowubuntu.com/markdown/)。

进一步阅读:

-Github Pages的[官方说明文件](http://pages.github.com/)

-[jekyll主页上提供的示例网站](https://github.com/mojombo/jekyll/wiki/sites)，可以clone他们的网站折腾一翻

-[在github上建立pages的过程](http://www.pizn.me/2011/09/22/create-github-page.html)

-关于jekyll静态网站的介绍，请看[文章](http://chen.yanping.me/cn/blog/2011/12/15/building-static-sites-with-jekyll/)

-[像黑客一样写博客——Jekyll入门](http://www.soimort.org/tech-blog/2011/11/19/introduction-to-jekyll_zh.html)

其他技巧：

-[优化Jekyll站点的SEO技巧](http://www.pizn.me/2012/01/16/the-seo-for-jekyll-blog.html)

-[为Jekyll博客添加category 分类](http://www.pizn.me/2012/02/23/use-category-plugin-for-jekyll-blog.html)

-[搭建Jekyll博客的一些小技巧](http://www.pizn.me/2012/03/01/some-tips-for-jekyll-blog.html)

#####关于代码高亮（如果你不贴代码，请跳过这步）

-用js插件：[DlHightLight](http://mihai.bazon.net/projects/javascript-syntax-highlighting-engine)或[Google Code Prettify](http://code.google.com/p/google-code-prettify/)

-用[gist](https://gist.github.com/)：强烈推荐菜鸟使用，省心省事，支持语言多

-用[pygment](http://pygments.org/)：要安装python以及python的包管理软件，又是个大坑，不建议菜鸟使用，尤其是使用windows的

#####关于公式（如果你不贴公式，请跳过）

使用maruku来解析markdown文件，可以把LaTeX解析成图片，优点是网页加载速度快。但是在windows下安装复杂，且需要安装有LaTeX
[Mathjax](http://www.mathjax.org/)，请看我博文的[介绍](http://chen.yanping.me/cn/blog/2012/03/10/octopress-with-latex/)，缺点是动态加载，速度慢。

#####评论

国外的[Disqus](http://disqus.com/)和国内的[友言](http://uyan.cc/)

其他社会化服务

-分享：国内的[jiathis](http://jiathis.com/)和国外的[addthis](http://addthis.com/)

-图片：国内的[yupoo](http://www.yupoo.com/) 、[poco](http://www.poco.cn/)，国外的[Flickr](http://www.flickr.com/)、[imgur](http://imgur.com/)

#####关于域名

在本地代码库里新建名为CNAME的文本文件，把域名地址放进去。假设你的域名是domain.com，可以用命令
```
echo 'domain.com' > CNAME
```
然后
```
git add CNAME
git commit -am "CNAME file added"
git push
```
接着在自己的域名注册商那里改一下指向就行了。如果想对github域名绑定的机制有更多的了解。

#####其他可供选择的模板，推荐两款比较好用的

-[Octopress](http://www.octopress.org/)：windows下的[教程1](http://sinosmond.github.com/blog/2012/03/12/install-and-deploy-octopress-to-github-on-windows7-from-scratch/)、[教程2](http://tonytonyjan.heroku.com/2012/03/01/install-octopress-on-windows/)。

-[Jekyll Bootstrap](http://jekyllbootstrap.com/)

#####常犯的错误

-明明要给是要做项目主页，却在master分支下上传页面。只有名为username.github.com的是个人主代码库，username是你的github用户名，向这个代码库推送的网页默认的是master分支，直接就可以浏览。其他代码库都是项目代码库
-clone别人的代码库到本地后，没有把它的.git目录删除
-没有把别人页面里的配置部分彻底改掉，比如disqus的配置，CNAME文件，<title>等

####说明

在github根目录下创建名字为`username.github.io`的repository，勾选`Initialize this repository with a README`。然后将repository clone到本地，可以通过`Github for Windows`和`git bash terminal`。terminal命令如下：

```

git clone https://github.com/username/username.github.io

```

接下来进入project创建index.html并上传：

```

cd username.github.io

echo "Hello World" > index.html

git add --all

git commit -m "Initial commit"

git push

```


####project site

在你的电脑上，建立一个目录，作为项目的主目录。我们假定，它的名称为jekyll_demo。

```

$ mkdir jekyll_demo

$ cd jekyll_demo

$ git init

```

然后，创建一个没有父节点的分支`gh-pages`。因为github规定，只有该分支中的页面，才会生成网页文件。

```

$ git checkout --orphan gh-pages

```

以下所有动作，都在该分支下完成。

####第二步，创建设置文件。

在项目根目录下，建立一个名为`_config.yml`的文本文件。它是jekyll的设置文件，我们在里面填入如下内容，其他设置都可以用默认选项，具体解释参见官方网页。

```
baseurl: /jekyll_demo

目录结构变成：

/jekyll_demo

|--　_config.yml

```

####第三步，创建模板文件。

在项目根目录下，创建一个_layouts目录，用于存放模板文件。
　　`$ mkdir _layouts`
进入该目录，创建一个default.html文件，作为Blog的默认模板。并在该文件中填入以下内容。
```
　　<!DOCTYPE html>
　　<html>
　　<head>
　　　　<meta http-equiv="content-type" content="text/html; charset=utf-8" />
　　　　<title>{{ page.title }}</title>
　　</head>
　　<body>

　　　　{{ content }}

　　</body>
　　</html>
```
Jekyll使用Liquid模板语言，`{{ page.title }}`表示文章标题，`{{ content }}`表示文章内容，更多模板变量请参考官方文档。
目录结构变成：
```
　　/jekyll_demo
　　　　|--　_config.yml
　　　　|--　_layouts
　　　　|　　　|--　default.html
```
####第四步，创建文章。
回到项目根目录，创建一个_posts目录，用于存放blog文章。
　`　$ mkdir _posts`
进入该目录，创建第一篇文章。文章就是普通的文本文件，文件名假定为2012-08-25-hello-world.html。(注意，文件名必须为"年-月-日-文章标题.后缀名"的格式。如果网页代码采用html格式，后缀名为html；如果采用markdown格式，后缀名为md。）

在该文件中，填入以下内容：（注意，行首不能有空格）
```
　　---
　　layout: default
　　title: 你好，世界
　　---
　　<h2>{{ page.title }}</h2>
　　<p>我的第一篇文章</p>
　　<p>{{ page.date | date_to_string }}</p>
```
每篇文章的头部，必须有一个yaml文件头，用来设置一些元数据。它用三根短划线"---"，标记开始和结束，里面每一行设置一种元数据。"layout:default"，表示该文章的模板使用_layouts目录下的default.html文件；"title: 你好，世界"，表示该文章的标题是"你好，世界"，如果不设置这个值，默认使用嵌入文件名的标题，即"hello world"。
在yaml文件头后面，就是文章的正式内容，里面可以使用模板变量。`{{ page.title }}`就是文件头中设置的"你好，世界"，`{{ page.date }}`则是嵌入文件名的日期（也可以在文件头重新定义date变量），`| date_to_string`表示将page.date变量转化成人类可读的格式。
目录结构变成：
```
　　/jekyll_demo
　　　　|--　_config.yml
　　　　|--　_layouts
　　　　|　　　|--　default.html 
　　　　|--　_posts
　　　　|　　　|--　2012-08-25-hello-world.html
```
####第五步，创建首页。
有了文章以后，还需要有一个首页。
回到根目录，创建一个index.html文件，填入以下内容。

```
　　---
　　layout: default
　　title: 我的Blog
　　---
　　<h2>{{ page.title }}</h2>
　　<p>最新文章</p>
　　<ul>
　　　　{% for post in site.posts %}
　　　　　　<li>{{ post.date | date_to_string }} <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
　　　　{% endfor %}
　　</ul>
```
它的Yaml文件头表示，首页使用default模板，标题为"我的Blog"。然后，首页使用了{% for post in site.posts %}，表示对所有帖子进行一个遍历。这里要注意的是，Liquid模板语言规定，输出内容使用两层大括号，单纯的命令使用一层大括号。至于{{site.baseurl}}就是_config.yml中设置的baseurl变量。
目录结构变成：
```
　　/jekyll_demo
　　　　|--　_config.yml
　　　　|--　_layouts
　　　　|　　　|--　default.html 
　　　　|--　_posts
　　　　|　　　|--　2012-08-25-hello-world.html
　　　　|--　index.html
```
####第六步，发布内容。
现在，这个简单的Blog就可以发布了。先把所有内容加入本地git库。
```
　　$ git add .
　　$ git commit -m "first post"
```
然后，前往github的网站，在网站上创建一个名为jekyll_demo的库。接着，再将本地内容推送到github上你刚创建的库。注意，下面命令中的username，要替换成你的username。
```
　　$ git remote add origin https://github.com/username/jekyll_demo.git
　　$ git push origin gh-pages
```
上传成功之后，等10分钟左右，访问http://username.github.com/jekyll_demo/就可以看到Blog已经生成了（将username换成你的用户名）。



####第七步，绑定域名。
如果你不想用http://username.github.com/jekyll_demo/这个域名，可以换成自己的域名。
具体方法是在repo的根目录下面，新建一个名为CNAME的文本文件，里面写入你要绑定的域名，比如example.com或者xxx.example.com。
如果绑定的是顶级域名，则DNS要新建一条A记录，指向204.232.175.78。如果绑定的是二级域名，则DNS要新建一条CNAME记录，指向username.github.com（请将username换成你的用户名）。此外，别忘了将_config.yml文件中的baseurl改成根目录"/"。
至此，最简单的Blog就算搭建完成了。进一步的完善，请参考Jekyll创始人的示例库，以及其他用Jekyll搭建的blog。


