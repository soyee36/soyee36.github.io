---
tags: IT
title: 笔记-更新bundle使插件支持新特性
excerpt: 譬如Github Pages旧版本jekyll并不支持emoji，新版本才可以。
---

Also published on [S&C](https://soandcandy.us)

CC 4.0 BY-SA

----

RubyGems官方源有时很难连，无法update。尤其在中国，`RubyGems`一直以来在国内都非常难访问到，在本地你或许可以翻墙，当你要发布上线的时候，你就很难搞了！以前淘宝运营过一个镜像网站，后来运营日益困难，现在转由[Ruby China](https://gems.ruby-china.org/)进行运营。

这是一个完整`RubyGems`镜像，你可以用此代替官方版本。


### 如何使用非官方源 ###

尽可能用比较新的RubyGems版本。

```ruby
    gem update --system
    gem -v
    2.6.3
```

换源：

```
   gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
   gem sources -l
   https://gems.ruby-china.org
```

确保出来的结果只有https://gems.ruby-china.org一个。

### 如果你使用Gemfile和bundle ###

可以使用bundler的Gem源代码镜像命令：

```
   bundle config mirror.https://rubygems.org https://gems.ruby-china.org
```

这样一来，就不需要改Gemfile的源。建议github pages搭建博客的童鞋用这种方式。

### 更新 ###

**进入目录**

例如，我们要更新github pages博客的插件，bundle。
Terminal进入clone回来的网站目录后，运行`bundle update`即可更新。


#### 例子： 更新emoji支持 ####

Add the following to your Gemfile, you could edit on your github repo:

`gem 'jemoji'`

And add the following to your site's `_config.yml`:

```
   plugins:
     - jemoji
```

旧版本的jekyll可能使用`gems`来代替`plugins`，意义一样。

这时候，进入网站目录，进行`bundle update`即可更新。

在文章中书写emoji的语法：

```
   :+1:
```

两个冒号之间放入emoji代码即可显示。emoji的代码可以参阅[Emoji Cheat Sheet](https://www.webpagefx.com/tools/emoji-cheat-sheet/)。


:bowtie:




