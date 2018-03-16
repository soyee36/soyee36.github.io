---
tags: jekyll YAML github
title: Jekyll Writing Skills
excerpt: 备忘
---


Also published on [S&C](https://soandcandy.us)

CC 4.0 BY-SA


----


## 基于jekyll写作的若干手记 ##


Github Pages搭建的BLOG是基于jekyll，在jekyll上书写有一些特别的地方。

### 头信息 ###


头信息是jekyll跟其他同类，如WordPress区别开来，而且让jekyll显得特别炫酷的地方。**任何只要包含YAML头信息的文件，在jekyll中都能被当作一个特殊文件来处理。**

意思就是说，只要在开头处指定，这个文件就能特殊处理。例如本文开头：

```php
   ---
   tags: jekyll YAML github
   title: Jekyll Writing Skills
   ---
```

可以指定文章的**标签**、**题目**。而YAML的变量通常有：

- layout        模板文件，需要放在_layouts目录下；
- permalink     固定链接；
- published     true or false，发表与否；
- categories    分类，也可以是category；
- tags          标签。



### 引用图片和其它资源 ###

经常地，我们在文章中需要引用图片、下载资料等等。jekyll支持的Markdown和Textile语言在链接这类内容时，用法是不同的。但是，我们只需要关系这类内容究竟放在我们站点的什么位置。

以Markdown为例，我们如果在文章中引用一个图片，通常是这样：

```
   ![截图](/路径/assets/图片.jpg)
```

如此一来，我们在站点的assets目录下，存放的`图片.jpg`即可被链接显示。

或者，我们链接一个读者们能够下载的PDF文件：

```bash
   [下载]({{site.url}}/assets/mydoc.pdf)
```

其中`{{site.url}}`可定义变量。如果你的站点只在域名的根URL下展示，你可以不使用该变量，而直接使用`/path/file`。


### 语法高亮 ###

jekyll自带语法高亮，如下面的例子所示：

{% highlight ruby %}
```
   def show
    @widget = Widget(params[:id])
    respond_to do |format|
      format.html # show.html.erb
      format.json { render json: @widget }
    end
  end
```
{% endhighlight %}


**注：貌似和Markdown语法冲突了。**


暂时这么多。


