---
layout: post
title:  Readability of large integer literals
date:   2017-02-28 22:17:00
categories: beginner
---

Sometimes we write programs which contain literals numeric. Large numbers can be very difficult to read.

我们写作或写程序的时候，时常碰到数字。一些很大的数字看起来比较困难（太多位数）。

How many zeroes are there?

不信你看看，这数字一看上去究竟有几个零？

{% highlight ruby %}
  billion = 1000000000
  ten_billion = 10000000000
{% endhighlight %}

In Ruby we can add underscore to large numeric literals to improve their readability.

在Ruby中，我们可以对数字加上下划线，以方便阅读。

{% highlight ruby %}
  billion = 1_000_000_000
  ten_billion = 10_000_000_000
{% endhighlight %}

As you can see it was much easier to read.

你看，是不是看起来容易多了。
