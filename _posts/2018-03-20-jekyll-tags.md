---
tags: IT Jekyll
title: 基于Jekyll的tags免插件使用指南
excerpt: Jekyll有大量优秀的Plugins提供各种功能。若果我们不想使用太多插件以免造成各类兼容问题，怎样才能免插件实现tags？
---


Also published on [S&C](https://soandcandy.us)

CC 4.0 BY-SA

* TOC
{:toc}

----

关于Jekyll上tags（或者categories）的实现，有很多方式。譬如最常用的**Plugins**法，通过Gem Plugin如**jekyll-tagging**配置实现。但是，jekyll-tagging并非Github的官方插件。这意味着你需要run on your own risk。

亦有不使用plugin的方法，以本站为例简单说说。


![Jekyll](https://i.imgur.com/V1YEOqk.png)


### Jekyll tags on Github ###

我们设置tags的目的，就是为了给站点博客的内容进行分类、标签。这里说的实现方法适应于Markdown或textile。


#### 1. 给文章加标签 ####

前面的文章简单介绍过YAML头信息，[点击这里](http://soyee.me/Writing-with-jekyll)进行温故。因此，实现tags的第一步，是在YAML头信息**给文章加标签**，在你文章的开头处：


```bash

  ---
  layout: post
  title: 基于Jekyll的tags免插件使用指南
  excerpt: 爱简介啥就简介啥
  tags: Jekyll IT
  ---
```


如上述代码所示，`tags`就是给该文章**标签**起来，以便全局搜寻到。在本例里，tags标签之间，使用**空格**进行分隔，如果你的标签是两个字以上的，务必用连接号如`-`或`_`连接起来。


#### 2. 搜集所有文章的标签 ####

这一步是用[Liquid Templating](https://jekyllrb.com/docs/templates/)实现的。在你站点的根目录，建一个`_includes`的文件夹，在里面建一个`collecttags.html`的文件，当然你也可以建其它名字，内容如下：


{% raw %}

```
   {% assign rawtags = "" %}
   {% for post in site.posts %}
     {% assign ttags = post.tags | join:'|' | append:'|' %}
       {% assign rawtags = rawtags | append:ttags %}
       {% endfor %}
       {% assign rawtags = rawtags | split:'|' | sort %}

       {% assign site.tags = "" %}
       {% for tag in rawtags %}
         {% if tag != "" %}
             {% if tags == "" %}
                   {% assign tags = tag | split:'|' %}
                       {% endif %}
                           {% unless tags contains tag %}
                                 {% assign tags = tags | join:'|' | append:'|' | append:tag | split:'|' %}
                                     {% endunless %}
                                       {% endif %}
                                       {% endfor %}
```
{% endraw %}



这个文件的作用，就是建立起站点的tags list.


#### 3. 执行搜集行为 ####

通常地，我们把标签放在文首，或者文末。这里以放在文首为例。在刚刚的文件夹`_includes`里再建一个`head.html`的文件，里面放入以下内容：

{% raw %}
```bash
{% if site.tags != "" %}
  {% include collecttags.html %}
  {% endif %}
```
{% endraw %}

    
如此一来，我们就定义了Jekyll博客的文首，而它会指引执行上述`collecttags.html`的文件代码，而且只执行一次。


#### 4. 显示标签 ####

我们每篇文章都有一个模板，放在`_layouts`目录下。我们希望每篇文章都显示标签，则需要在`post`模板里加入以下语法：

{% raw %}
```bash
<span>[
  {% for tag in page.tags %}
      {% capture tag_name %}{{ tag }}{% endcapture %}
          <a href="/tag/{{ tag_name }}"><code class="highligher-rouge"><nobr>{{ tag_name }}</nobr></code>&nbsp;</a>
            {% endfor %}
            ]</span>
```
{% endraw %}



![文首标签](https://i.imgur.com/IChEDB1.png)

需要注意的是，以上的语法需要在站点根目录下有一个`tag`的文件夹，而每一个使用的标签，都要建立一个对应的`标签名称.md`的文件。

这个`标签名称.md`的文件，里面只需要有一个YAML头信息被读取到即可：

```bash


  ---
  layout: tagpage
  title: "Tag: 标签名称"
  tag: 标签名称
  ---

```



#### 5. 定义标签页面模板 ####

在`_layouts`文件夹里新增`tagpage.html`文件：

{% raw %}
```bash
---
layout: default
---
<div class="post">
<h1>Tag: {{ page.tag }}</h1>
<ul>
{% for post in site.tags[page.tag] %}
  <li><a href="{{ post.url }}">{{ post.title }}</a> ({{ post.date | date_to_string }})<br>
      {{ post.description }}
        </li>
        {% endfor %}
        </ul>
        </div>
        <hr>
```
{% endraw %}



需要注意的是，如果你要新增一个标签，你需要在`tag`目录下新建一个`标签名称.md`的文件，这个需要再强调一下。


#### 6. 简单的标签云 ####

我们不一定想在页面把标签罗列出来，而是：

读者点击某标签时，跳转标签页，在该标签页顺便显示`标签云`。


![很丑的标签云](https://i.imgur.com/OtijFbF.png)

在`_includes`目录下新建一个`archive.html`文件：

{% raw %}
```bash
<h2>Archive</h2>
{% capture temptags %}
  {% for tag in site.tags %}
      {{ tag[1].size | plus: 1000 }}#{{ tag[0] }}#{{ tag[1].size }}
        {% endfor %}
        {% endcapture %}
        {% assign sortedtemptags = temptags | split:' ' | sort | reverse %}
        {% for temptag in sortedtemptags %}
          {% assign tagitems = temptag | split: '#' %}
            {% capture tagname %}{{ tagitems[1] }}{% endcapture %}
              <a href="/tag/{{ tagname }}"><code class="highligher-rouge"><nobr>{{ tagname }}</nobr></code></a>
              {% endfor %}
```
{% endraw %}




语法可以随意按照你希望的样子进行调整。

然后，为了实现上面说的方式，我们把如下语句放在`_layouts/tagpage.html`里：

{% raw %}
```bash
{% include archive.html %}
```
{% endraw %}



上传到你的仓库，收工！如果你希望自动生成`标签名称.md`，可以写一段JS代码实现。


:bowtie:


P.S. 以上部分`Liquid`代码，为了不被jekyll执行，需要用`raw`and`endraw`包起Markdown语法。


