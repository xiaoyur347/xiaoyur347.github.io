---
title: 从头开始
layout: post
---

# 1. 打算迁移空间
百度空间这鬼，终于倒闭了。是时候整理自己的东西，重新来过了。
不打算浪费时间在各种博客空间的选择上，git这东西好，不管网站是否还在，本地的总是在的。所以我打算试试github io。
不知道为何，我的github io配置总是失败，暂时还没找到原因。github有过一封邮件说是index.html的编码问题，可是我修改后还是如此。那就试试装jeyll找找问题吧。

# 2. 参考
* http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html 阮一峰的jekyll入门
* https://help.github.com/articles/using-jekyll-with-pages/ github官方的帮助文档
* http://verynix.com/ubuntu1204-install-jekyll.html 如果一开始看这篇，可以省很多安装时间。

# 3. 404问题确认 —— 文件编码问题
按照阮一峰的jekyll入门配置，在github上提示有问题，确认是没有使用UTF8编码的问题，转换编码后一切正常，除了不好看。

# 4. ubuntu 12.04安装jeyll

## 问题1：墙 —— 改用taobao源
https://help.github.com/articles/using-jekyll-with-pages/

按照上面链接来安装ruby。可是总是卡在第2步上，卡很久后我以为是ruby版本问题，于是尝试装Ubuntu 14.04，还是如此。接着我想到了墙。。
于是我按照 http://ruby.taobao.org 的做法进行修改。

```shell
$ gem sources -l
*** CURRENT SOURCES ***
http://rubygems.org/
$ gem sources --remove http://rubygems.org/
$ gem sources -a https://ruby.taobao.org/
```

然后

```shell
$ sudo gem install bundler
$ sudo gem install github-pages -V
```

-V是为了看调试信息的，github-pages安装较慢，看看调试信息才能心不慌。
结果：

```shell
ERROR:  Error installing github-pages:
public_suffix requires Ruby version >= 2.0.
```

安装悲剧了。

## 问题2：ruby 1.9.3不支持github-pages —— 改用ruby 2.1

好吧，安装ruby 2.0吧。
按照http://askubuntu.com/questions/261329/package-for-ruby-2-0-on-precise 中

```shell
$ sudo add-apt-repository ppa:brightbox/ruby-ng-experimental
$ sudo apt-get update
$ sudo apt-get install -y ruby2.1 ruby2.1-dev ruby2.1-doc
```

我去https://launchpad.net/~brightbox/+archive/ubuntu/ruby-ng-experimental 上看到支持ruby 2.1，那就跳过2.0，直接上2.1吧。不需要删除ruby 1.9。
额，发现支持ruby 2.2……

## 问题3：配置时提示没有js引擎 —— 安装nodejs
问题信息：

```shell
$ bundle exec jekyll serve
/var/lib/gems/2.1.0/gems/execjs-2.5.2/lib/execjs/runtimes.rb:48:in `autodetect': 
Could not find a JavaScript runtime. 
See https://github.com/rails/execjs for a list of available runtimes. (ExecJS::RuntimeUnavailable)
```

处理方法：

```shell
$ sudo apt-get install nodejs
$ sudo gem install execjs
```