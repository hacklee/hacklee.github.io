---
layout: post
title: 在linux上配置node.js&npm环境
category: tools
tags: [node , npm, linux]
description: 在linux上配置node.js&npm环境
---

### node.js && NPM
[NPM][1]是node的包管理工具。grunt,bower,yo,io.js等都是基于npm管理的工具包。

- 下载已经编译好的文件安装node环境

> 从官网[https://nodejs.org/download/][2]下载相应的最新版本文件

```
wget https://nodejs.org/dist/v0.12.7/node-v0.12.7-linux-x64.tar.gz -O node-v0.12.7.tar.gz
```
- 解压下载的文件

```
tar -xvf node-v0.12.7.tar.gz
```

- 进入解压的bin目录，可以发现已经有已经编译好的node和npm文件

```
cd node-v0.12.7-linux-x64/bin/ && ls -l
```

- 移动命令文件node到bin目录下全局使用

```
ln -s node /usr/local/bin/
```

- 安装npm

```
curl -L https://npmjs.org/install.sh | sh
```

- 查看版本

```
node -v
npm -v
```

- 配置npm使用其他镜像资源库

> 默认资源地址：http://registry.npmjs.org
> 淘宝镜像地址：https://registry.npm.taobao.org (有10分钟同步时间差)

```
npm config set registry "https://registry.npm.taobao.org" 
```

- 查看某个工具包信息(以grunt为例)

> 从dist: tarball可以看到资源使用的镜像地址

```
npm info grunt
```

- 安装、更新、移除工具包

> 工具包的使用安装分为全局和局部（当前目录）,-g全局 --save安装且保存，并把依赖加入到package.json

```
npm install xxx
npm update
npm uninstall xxx
npm outdated : 查看当前依赖包版本更新情况
```



  [1]: https://www.npmjs.com/
  [2]: https://nodejs.org/download/