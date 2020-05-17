---
title: "Fix Jekyll Ordered List Restart | 解决Jekyll有序列表重新开始"  
date: "2020-05-12 15:59:01 -0700"
template: "post"
draft: false
slug: "fix-jekyll-ordered-list-restart"
category: "other"
description: "Fix Jekyll Ordered List Restart | 解决Jekyll有序列表重新开始"  
---

##  问题描述

在写完一篇文章之后，通过git提交上去，结果发现有序列表的序号都不对了：

1. 有序列表在代码段之后会自动重新开始计数
2. 希望从0开始计数但是缺从1开始

具体的描述基本就和这个帖子一样：[stackoverflow](https://stackoverflow.com/questions/18088955/markdown-continue-numbered-list)

你以为的输出

0. item 0

1. item 1

   ```
   Code
   ```

2. item 2

实际的输出

1. item 0

2. item 1

   ```
   Code
   ```

1\. item 2

## 解决方法

### 方案1

给代码块加4个空格的缩进，这个可以有效的解决重新计数的问题，但是如何让开始数字是0呢？

通过一番搜索， 发现Jekyll使用的是Kramdown来解析Markdown，结果查阅文档发现：不支持从0开始。

[Ordered and Unordered lists](https://kramdown.gettalong.org/syntax.html#ordered-and-unordered-lists)

> The numbers used for ordered lists are irrelevant, an ordered list always starts at 1.

### 方案2

好吧，Kramdown不支持，但为什么Typora可以支持呢？

这是因为Typora实现了[GitHub Flavored Markdown(GFM)](https://github.github.com/gfm/)。通过查阅GFM的文档，可以看到人家的[Ordered List](https://github.github.com/gfm/#ordered-list-marker)是支持从零开始的，并且不加4个空格的缩进也不会把列表的序号弄乱。那么这么好的GFM，怎么样才能够拥有呢？

Jekyll可以让你[选择Markdown的render](https://github.github.com/gfm/#ordered-list-marker)！这里我们选择[CommonMark](https://github.com/github/jekyll-commonmark-ghpages)，就完成啦！

安装十分容易，在Gemfile中添加

```ruby
group :jekyll_plugins do
  gem 'jekyll-commonmark-ghpages'
end
```

再在_config.yml中设置使用CommonMark

```yml
markdown: CommonMarkGhPages
```

就可以了！

撒花✿✿ヽ(°▽°)ノ✿
