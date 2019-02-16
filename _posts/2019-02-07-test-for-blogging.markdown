---
layout: post
title: "Test for blogging"
date: 2019-02-07T12:55:13+08:00
categories: [Jekyll, Tech]
author: cdz
excerpt_separator: <!--more-->
---

该文用于说明写 blog 的一些格式要求。
<!--more-->

<br/>
### 新建 blog

当需要创建一个新的 blog 时(假设名字为：Test for blogging), 在 blog 的主目录中输入以下命令：

```
octopress new post Test for blogging
```

该命令会在文件夹 `_post` 中生成文件 `2019-02-07-test-for-blogging.markdown`, 文件名前面的时间是自动添加的，编辑该文件为以下格式：

```ruby
---
layout: post
title: "Test for blogging"
date: 2019-02-07T12:55:13+08:00
categories: [Jekyll, Tech]
author: cdz
excerpt_separator: <!--more-->
---
这里编写 blog 正文。中间可以用 <!--more--> 分割。
```

以上：
+ `layout`: 表示该 blog 属于哪个 layout，不同的 layout 会有不同的显示，具体显示的格式规定在文件夹 `_layouts` 中，当为 `post` 时表示按照 `_layouts/post.html` 的格式显示。
+ `date`: 表示该 blog 的生成时间，是自动生成的。
+ `categories`: 表示该 blog 属于那一类，可以自己定义，当属于多个类的时候需要使用 `[category1, category2 ]`
+ `author`: 表示该 blog 的博主。
+ `excerpt_separator: <!--more-->`: 表示在该 blog 中使用 `<!--more-->` 分割， `<!--more-->` 的前一半内容会显示在home中的预览中，后一半只有点进博客才能看到。 

这里是插入网址的方法：

```
// 格式为：[文中显示内容][网页地址记号]
// 在文本的最后：[网页地址记号]：网页地址
[CDZ's Blog][cdz-blog]

[cdz-blog]: https://blog.dianzhangchen.com
```

<br/>
### 预览 blog

在主目录中输入：
```
jekyll build
jekyll serve
```

这时候在浏览器中输入：[http://localhost:4000/][local-addr] 就可以去看效果。



<br/>
### 部署 blog 到网站


```
octopress deploy
```

<br/>
### 编写 markdown 的一些技巧


#### (1)、插入空行

插入空行使用 html 的语法，只需要在相应的位置插入 `<br/>`

#### (2)、插入图片

```
![image-title-here](/path/to/image.jpg){:class="img-responsive"}

// 或者按照以下方法，使用 html 语法, 可以调节图片的大小
<p><img src="/image/DongBei/barbecue.jpg" width="500"></p>

```

第一种方法效果：

![test_image](/image/test/icon.ico){:class="img-responsive"}

第二种方法效果：
<p><img src="/image/test/icon.ico" width="500"></p>

[local-addr]: http://localhost:4000/
