## 使用*gitpress* 


### 为什么用github来写博客？

理由很简单，首先github支持比较好用的markdown语法，而且它自带的编辑器对程序员博客比较友好，最重要是写代码比较方便，而且生成的代码比较好看，如果不喜欢在线编写的同学，还可以在自己本地写好之后push到服务器上去。



### 如何用github写博客？

事实上github本身提供的一套[Github Pages](http://pages.github.com/)的服务可以很方便地生成项目文档，当然也可以用来写博客 [阮一峰](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html) 童鞋写的这个教程很详尽地介绍了如何使用Github Pages来生成博客。

### 优缺点

用Git Pages生成博客的确有使用github写博客的部分优点，但是它也有一些明显的缺点。首先是Github Pages是通过静态生成的方式从你的模板生成静态页面，它通常需要一些编译时间，而且github对它的生成更新比较慢（意味着你写完博客需要10分钟左右看到它的更新），其次，在文章中你需要写模板和HTML代码，这就不适合只想要配几下参数然后发布文章的懒人使用了。

### 介绍 Git Press

于是，[akira-cn](https://github.com/akira-cn)参考Github Pages，设计了一套新的系统叫做[Git Press](http://www.gitpress.org)。顾名思义，这套系统是希望使用者利用github来管理博客的同时能像使用WordPress一样方便。

首先，你使用它很容易，只需要在你需要托管的项目中添加一个gitpress.json，文件内容为一个空的json { }，当你把这个文件push到你的项目根目录之后，你就已经可以通过`http://项目名.你的用户名.gitpress.org`看到你的网站了（如果你的项目没有README，有可能看不到，所以需要添加README.md）。当然，现在整个网站仅仅是生成了一个“壳”还远远谈不上是一个“博客”。

将网站变成博客，你需要了解一部分gitpress.org的配置。

这是gitpress.org配置：

```
{
	"docs"      : {
		"about": "README.md",
		"study": "posts/study",
		"work": "posts/work",
		"life": "posts/life",
		"emot": "posts/emot",
		"essay": "posts/essay"
	},	
	"perpage"   : 10,
	"template"  : "pithiness",
	"types"     : {
		"\\.(md||markdown)$"   : "markdown", 
		"\\.(js||css||json)$"  : "code",
		"\\.html?$"            : "html",
		".*"                   : "text"		
	},
	"title"  : "Che Field",
	"comment"  : {"type": "duoshuo", "short_name":"chefield"},
	"friends"  : [
		{
			"name"  : "Akira's Blog",
			"title"  : "gitpress月影",
			"url"  : "http://blog.silverna.org/"		  
		},
		{
			"name"  : "Yanping's Blog",
			"title"  : "gitpage雁平",
			"url"  : "http://yanping.me/"		  
		},
		{
			"name"  : "FReehao123",
			"title"  : "免费资源部落",
			"url"  : "http://www.freehao123.com/"		  
		}
	]

}

```

`docs`属性指定了博客文章放在`posts`目录下，分成几个子目录，每个目录为一个分类。`perpage`属性指定了每一页显示几篇文章，`types`属性是文档类型的对应保持默认就好，可以不用管它。`title`如果不写的话，那么系统自动获取你的项目的`title`，`comment`属性是`on`，表示允许文章的评论，`domain_alias`属性把自己的域名指向博客，这样就可以用自己的域名来访问。如果你有属于自己的域名，可以先将你想使用的域名配置DNS解析到gitpress.org（设置CNAME到gitpress.org或者A记录到162.243.44.85），设置好之后等待两三分钟，先用原本的gitpress.org域名访问一次（为了让服务器更新配置），然后就可以用你的新域名访问了。`friends`是博客右侧的友情链接。


### gitpress 文章分类

可以把`gitpress.json`的`docs`参数配置成json格式的，自定义分类名，例如：

```json
{
    "docs" : {"技术文档": "posts/tech", "生活随笔": "posts/life"}
}
```

好了，要介绍的就这些，剩下的大家慢慢体验吧，有什么不明白的，[gitpress.org](http://gitpress.org) 提供了一份更详细的文档。你也可以到博客的[项目地址](https://github.com/akira-cn/blog)去看一下目录和文件结构。

