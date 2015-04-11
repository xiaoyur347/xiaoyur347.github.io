---
title: 从头开始
layout: default
---

# 打算迁移空间
百度空间这鬼，终于倒闭了。是时候整理自己的东西，重新来过了。
不打算浪费时间在各种博客空间的选择上，git这东西好，不管网站是否还在，本地的总是在的。所以我打算试试github io。
不知道为何，我的github io配置总是失败，暂时还没找到原因。github有过一封邮件说是index.html的编码问题，可是我修改后还是如此。那就试试装jeyll找找问题吧。

# ubuntu 12.04设置jeyll
https://help.github.com/articles/using-jekyll-with-pages/
按照上面链接来安装ruby。可是总是卡在第2步上，卡很久后我尝试装Ubuntu 14.04，还是如此。接着我想到了墙。。
于是我按照http://ruby.taobao.org进行修改。
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
```
ERROR:  Error installing github-pages:
public_suffix requires Ruby version >= 2.0.
```
安装悲剧了。
好吧，安装ruby 2.0吧。
按照http://askubuntu.com/questions/261329/package-for-ruby-2-0-on-precise 中
```shell
sudo add-apt-repository ppa:brightbox/ruby-ng-experimental
sudo apt-get update
sudo apt-get install -y ruby2.1 ruby2.1-dev ruby2.1-doc
```
我去https://launchpad.net/~brightbox/+archive/ubuntu/ruby-ng-experimental 上看到支持ruby 2.1，那就跳过2.0，直接上2.1吧。不需要删除ruby 1.9。
额，发现支持ruby 2.2……
