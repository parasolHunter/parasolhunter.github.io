

![](https://raw.githubusercontent.com/parasolHunter/parasolhunter.github.io/master/img/readme-home.jpg)

[![Build Status](https://travis-ci.org/parasolHunter/parasolhunter.github.io.svg?branch=master)](https://travis-ci.org/parasolHunter/parasolhunter.github.io)
[![codebeat badge](https://codebeat.co/badges/5f031df3-f6c1-4ec0-911a-ff6617ca50b9)](https://codebeat.co/projects/github-com-parasolHunter-parasolhunter-github-io-master)
[![GitHub issues](https://img.shields.io/github/issues/parasolHunter/parasolhunter.github.io.svg?style=flat)](https://github.com/parasolHunter/parasolhunter.github.io/issues)
[![License MIT](https://img.shields.io/badge/license-MIT-blue.svg?style=flat)](https://github.com/home-assistant/home-assistant-iOS/blob/master/LICENSE)
[![](https://img.shields.io/github/stars/parasolHunter/parasolhunter.github.io.svg?style=social&label=Star)](https://github.com/parasolHunter/parasolhunter.github.io)
[![](https://img.shields.io/github/forks/parasolHunter/parasolhunter.github.io.svg?style=social&label=Fork)](https://github.com/parasolHunter/parasolhunter.github.io)


>
### [查看博客戳这里 👆](https://parasolhunter.github.io)



## 使用

* 开始
	* [环境](#环境)
	* [开始](#开始)
	* [撰写博文](#撰写博文)
* 组件
	* [侧边栏](#侧边栏)
	* [迷你关于我](#mini-about-me)
	* [推荐标签](#featured-tags)
	* [好友链接](#friends)
	* [HTML5 演示文档布局](#keynote-layout)
* 评论与 Google/Baidu Analytics
	* [评论](#comment)
	* [网站分析](#analytics) 
* 高级部分
	* [自定义](#customization)
	* [标题底图](#header-image)
	* [搜索展示标题-头文件](#seo-title)

###步骤
#####一 ：安装Ruby
#####二 ：安装RubyGems
#####三：用RubyGems安装Jekyll
#####四：cd到博客文件夹，开启服务器
#####五：访问 http://localhost:4000/
#####六：提交代码到远程GitHub上

### 环境

如果你安装了 [jekyll](http://jekyllcn.com/)，那你只需要在命令行输入`jekyll serve` 或 `jekyll s`就能在本地浏览器中输入`http://127.0.0.1:4000/`预览主题，对主题的修改也能实时展示（需要强刷浏览器）。



### 开始

你可以通用修改 `_config.yml`文件来轻松的开始搭建自己的博客:

```
# Site settings
title: Parasol Blog            # 你的博客网站标题
SEOTitle: 邓瑞东的博客 | Parasol Blog  # SEO 标题
header-img: img/home-bg.jpg
email: 879623546@qq.com
description: "All things are difficult before they are easy." # 随便说点，可以写座右铭
keyword: "Parasol, Parasol Blog, 邓瑞东的博客, dengruidong, 邓瑞东, iOS, Apple, iPhone"
url: "http://parasolhunter.github.io"          # your host, for absolute URL
baseurl: ""      # 可以不用填
github_repo: "https://github.com/parasolHunter/parasolhunter.github.io.git" # you code repository

# Build settings
permalink: pretty
paginate: 10                 # 一页你准备放几篇文章
exclude: ["less","node_modules","Gruntfile.js","package.json","README.md"]
anchorjs: true                          # if you want to customize anchor. check out line:181 of `post.html`
```

Jekyll官方网站还有很多的参数可以调，比如设置文章的链接形式...网址在这里：[Jekyll - Official Site](http://jekyllrb.com/) 中文版的在这里：[Jekyll中文](http://jekyllcn.com/).

### 撰写博文

要发表的文章一般以 **Markdown** 的格式放在这里`_posts/`，你只要看看这篇模板里的文章你就立刻明白该如何设置。

yaml 头文件长这样:

```
---
layout:     post
title:      标题
subtitle:   副标题
date:       创作日期
author:     Parasol
header-img: img/home-bg.jpg
catalog: 	 true
tags:
    - css
    - 基础样式
---

```

### 侧边栏

看右边:
![](https://raw.githubusercontent.com/parasolHunter/parasolhunter.github.io/master/img/readme-side.jpg)

设置是在 `_config.yml`文件里面的`Sidebar settings`那块。

```
# Sidebar settings
sidebar: true                           # 添加侧边栏
sidebar-about-description: "简单的描述一下你自己"
sidebar-avatar: /img/about-RD-gentle.jpg      # 你的大头贴，请使用绝对地址.注意：名字区分大小写！后缀名也是
```

侧边栏是响应式布局的，当屏幕尺寸小于992px的时候，侧边栏就会移动到底部。具体请见bootstrap栅格系统 <http://v3.bootcss.com/css/>


### Mini About Me

Mini-About-Me 这个模块将在你的头像下面，展示你所有的社交账号。这个也是响应式布局，当屏幕变小时候，会将其移动到页面底部，只不过会稍微有点小变化，具体请看代码。

### Featured Tags

看到这个网站 [Medium](http://medium.com) 的推荐标签非常的炫酷，所以我将他加了进来。
这个模块现在是独立的，可以呈现在所有页面，包括主页和发表的每一篇文章标题的头上。

```
# Featured Tags
featured-tags: true  
featured-condition-size: 1     # A tag will be featured if the size of it is more than this condition value
```

唯一需要注意的是`featured-condition-size`: 如果一个标签的 SIZE，也就是使用该标签的文章数大于上面设定的条件值，这个标签就会在首页上被推荐。
 
内部有一个条件模板 `{% if tag[1].size > {{site.featured-condition-size}} %}` 是用来做筛选过滤的.

### Social-media Account

在下面输入的社交账号，没有的添加的不会显示在侧边框中。新加入了[简书](https:/www.jianshu.com)链接, <http://www.jianshu.com/u/0d4833dfcc16>

	# SNS settings      
	RSS: false
	weibo_username:     username
	zhihu_username:     username
	github_username:    dengruidong  # 你的github账号
	facebook_username:  username
	jianshu_username:   0d4833dfcc16     # 你的简书ID
	twitter_username:  	username
	
	

![](http://ww4.sinaimg.cn/large/006tKfTcgy1fgrgbgf77aj308i02v748.jpg)

### Friends

好友链接部分。这会在全部页面显示。

设置是在 `_config.yml`文件里面的`Friends`那块，自己加吧。

```
# Friends
friends: [
    {
        title: "Parasol Blog",
        href: "https://parasolhunter.github.io/"
    }
]
```


### Keynote Layout

HTML5幻灯片的排版：

![](https://camo.githubusercontent.com/f30347a118171820b46befdf77e7b7c53a5641a9/687474703a2f2f6875616e677875616e2e6d652f696d672f626c6f672d6b65796e6f74652e6a7067)

这部分是用于占用html格式的幻灯片的，一般用到的是 Reveal.js, Impress.js, Slides, Prezi 等等.我认为一个现代化的博客怎么能少了放html幻灯的功能呢~

其主要原理是添加一个 `iframe`，在里面加入外部链接。你可以直接写到头文件里面去，详情请见下面的yaml头文件的写法。

```
---
layout:     keynote
iframe:     "http://huangxuan.me/js-module-7day/"
---
```

iframe在不同的设备中，将会自动的调整大小。保留内边距是为了让手机用户可以向下滑动，以及添加更多的内容。


### Comment

博客不仅支持 [Disqus](http://disqus.com) 评论系统,还加入了 [Gitalk](https://gitalk.github.io/) 评论系统，[支持 Markdwon 语法](https://guides.github.com/features/mastering-markdown/)，cool~

#### Disqus

优点：国际比较流行，界面也很大气、简洁，如果有人评论，还能实时通知，直接回复通知的邮件就行了；

缺点：评论必须要去注册一个disqus账号，分享一般只有Facebook和Twitter，另外在墙内加载速度略慢了一点。

> Node：有很多人反映 Disqus 插件加载不出来，可能墙又架高了，有条件的话翻个墙就好了~

**使用：**

**首先**，你需要去注册一个Disqus帐号。

**其次**，你只需要在下面的 yaml 头文件中设置一下就可以了。

```
# 评论系统
# Disqus（https://disqus.com/）
disqus_username: dengruidong
```

#### Gitalk

优点：界面干净简洁，利用 Github issue API 做的评论插件，使用 Github 帐号进行登录和评论，最喜欢的支持 Markdown 语法，对于程序员来说真是太 cool 了。

缺点：配置比较繁琐，每篇文章的评论都需要初始化。

**使用：**

```_config.yml
# Gitalk
gitalk:
  enable: true    #是否开启Gitalk评论
  clientID: 4952b119ffafda92c3a8                            #生成的clientID
  clientSecret: a5b954eefd067bc182966fb2c406431a110f53cc    #生成的clientSecret
  repo: parasolhunter.github.io    #仓库名称
  owner: dengruidong    #github用户名
  admin: dengruidong
  distractionFreeMode: true #是否启用类似FB的阴影遮罩
```

```页面引用
<!-- Gitalk 评论 start  -->
{% if site.gitalk.enable %}
<!-- Gitalk link  -->
<link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
<script src="https://unpkg.com/gitalk@latest/dist/gitalk.min.js"></script>

<div id="gitalk-container"></div>
    <script type="text/javascript">
    var gitalk = new Gitalk({
        clientID: '{{site.gitalk.clientID}}',
        clientSecret: '{{site.gitalk.clientSecret}}',
        repo: '{{site.gitalk.repo}}',
        owner: '{{site.gitalk.owner}}',
        admin: ['{{site.gitalk.admin}}'],
        id: 'about',
        distractionFreeMode: '{{site.gitalk.distractionFreeMode}}',
    });
    gitalk.render('gitalk-container');
</script>
{% endif %}
<!-- Gitalk end -->
```

### Customization

如果你喜欢折腾，你可以去自定义这个模板的 Code。

**如果你可以理解 `_include/` 和 `_layouts/`文件夹下的代码（这里是整个界面布局的地方），你就可以使用 Jekyll 使用的模版引擎 [Liquid](https://github.com/Shopify/liquid/wiki)的语法直接修改/添加代码，来进行更有创意的自定义界面啦！**

### Header Image

博客每页的标题底图是可以自己选的，看看几篇示例post你就知道如何设置了。
  
标题底图的选取完全是看个人的审美了。每一篇文章可以有不同的底图，你想放什么就放什么，最后宽度要够，大小不要太大，否则加载慢啊。

> 上传的图片最好先压缩，这里推荐 https://tinypng.com/ 熊猫图片压缩网站，让你的博客起飞。

但是需要注意的是本模板的标题是**白色**的，所以背景色要设置为**灰色**或者**黑色**，总之深色系就对了。当然你还可以自定义修改字体颜色，总之，用github pages就是可以完全的个性定制自己的博客。

### SEO Title

我的博客标题是 **“Parasol Blog”** 但是我想要在搜索的时候显示 **“邓瑞东的博客 | Parasol Blog”** ，这个就需要 SEO Title 来定义了。

### 关于收到"Page Build Warning"的 Email

由于jekyll升级到3.0.x,对原来的 pygments 代码高亮不再支持，现只支持一种-rouge，所以你需要在 `_config.yml`文件中修改`highlighter: rouge`.另外还需要在`_config.yml`文件中加上`gems: [jekyll-paginate]`.

同时,你需要更新你的本地 jekyll 环境.

使用`jekyll server`的同学需要这样：

1. `gem update jekyll` # 更新jekyll
2. `gem update github-pages` #更新依赖的包

使用`bundle exec jekyll server`的同学在更新 jekyll 后，需要输入`bundle update`来更新依赖的包.

> Note：
> 可以使用 `jekyll -s` 命令在本地实时配置博客，提高效率。详见 [Jekyll.com](http://jekyllcn.com/)

参考文档：[using jekyll with pages](https://help.github.com/articles/using-jekyll-with-pages/) & [Upgrading from 2.x to 3.x](http://jekyllrb.com/docs/upgrading/2-to-3/)


## 致谢

1. 这个模板是从这里 [Hux](https://github.com/Huxpro/huxpro.github.io) fork 的, 感谢这个作者。 
2. 感谢 Jekyll、Github Pages 和 Bootstrap!

## License

遵循 MIT 许可证。有关详细,请参阅 [LICENSE](https://github.com/parasolHunter/parasolhunter.github.io/blob/master/LICENSE)。