---
layout: post
title: PHP-CS-Fixer
category: tools
tags: [php , php-cs-fixer, psr]
description: 使用php-cs-fixer规范php代码
---

## PHP-CS-Fixer
[PHP-CS-Fixer][1]是一个分析php源代码，然后将代码按照某种编码标准规范化的工具。
在平常工作中，我们写代码的编辑器或者IDE已经提供快捷格式化代码的操作，但是由于IDE的不一样，格式化后的代码格式也有所出入，为了整个项目的编码规范统一，若统一要求用PSR0、PSR1、PSR2、Symfony等标准，PHP-CS-Fixer就派上用场了（某些公司可能制定一套自己的编码标准）。

## 安装

#### 下载文件

- 直接下载文件[php-cs-fixer.phar][2]到本地电脑上

- 使用curl下载

> curl http://get.sensiolabs.org/php-cs-fixer.phar -o php-cs-fixer.phar

- 使用wget下载

> wget http://get.sensiolabs.org/php-cs-fixer.phar 

#### 移动文件

- 为文件增加执行权限

>  sudo chmod a+x php-cs-fixer

- 移动文件到PATH目录并重命名为php-cs-fixer

> sudo mv php-cs-fixer /usr/local/bin/php-cs-fixer

#### 更新

- sudo php-cs-fixer self-update

#### 使用

- php php-cs-fixer.phar fix /要规范的代码目录路径 --level=psr2

> 
--level : psr0,psr1,psr2,symfony (代码规范化的标准，默认为psr2加上作者的一些默认标准)


#### 与composer结合

- composer global require fabpot/php-cs-fixer

- composer global update fabpot/php-cs-fixer

## more
php-cs-fixer的使用细节还有很多，详细请参考[项目github地址][1]


  [1]: https://github.com/FriendsOfPHP/PHP-CS-Fixer
  [2]: http://get.sensiolabs.org/php-cs-fixer.phar