---
layout: post
title: yaml,markdown,swagger入门分享
category: tools
tags: [yaml , markdown, swagger]
description: yaml，markdown，swagger入门基础知识分享
---

# 目录

## 标记语言
1. Yaml (YAML Ain't Markup Language)
> [YAML](http://yaml.org/)是一个可读性高，用来表达资料序列的编程语言。

2. Markdown
> [Markdown](http://daringfireball.net/projects/markdown) 是一种轻量级标记语言,它允许人们“使用易读易写的纯文本格式编写文档，然后转换成有效的XHTML(或者HTML)文档”

## 文档工具
1. Swagger Editor
> [Swagger Editor](http://swagger.io/swagger-editor/) ,让我们可以利用浏览器基于YAML（json）语法定义API，然后它会自动生成一篇排版优美的API文档，并且提供实时预览，同时文档可供代码工具使用生成客服端或服务端代码

---

# Yaml规则简介（1.2）

**注意事项**

1.  用一个或多个空格描述文档嵌套（同层元素一定左对齐）
2. 不能使用Tab代替空格
3. 支持json语法
4. 简单数据可以不使用引号括起来，包括字符串数据（双引号字符串支持转义字符）
5. 以三个并排短横线（---）表示文档开始，三个联系点（...）表示文档结束，结束符可选（一个yaml文件，可以包含多个文档）
6. 以字符（#）表示注释开始，结束到行尾（单行注解，不支持多行）

---

# Yaml语法介绍

## 序列 （ Sequence ）
__短横线后必须有空格__
    
    - php
    - java
    - oc

*等同于以下json语法 （多个成功之间用逗号+空格分开）*

    ['php', 'java', 'oc']

---

## 键值对（Map）
__冒号后必须有空格__

    php: server
    java: android
    oc: ios

*等同于以下json语法（多个成功之间用逗号+空格分开）*

    {php: "server", java: "android", oc: "ios"}


**简单序列与键值对可以相互嵌套**
    
    #序列包含序列
    - ["php"] 
    
    #序列包含键值对
    - 
      name: lgy
      dev: php
      
    #键值对包含序列
    android: 
      - tc
      - ljj
        
    # 键值对包含键值对
    server: 
      php: lgy

**复杂键值对 （有点像三元表达式）**
__问号加空格表示复杂键值对的键，键值对的值可以在破折号、冒号或者问号后面开始__
    
    #序列键值对
    ? 
      - php
      - go
    : 
      - server
    
    #map键值对
    ? 
      first: Sammy
      last: Sosa
    : 
      avg: 0.278
      hr: 65

---

## 简单内容可以用块符号（竖线：|或尖括号：>）进行块显示，内容不会转义

**竖线（|）：所有的换行符会被保留**

    name: yaml块
    description: |
        这里属于竖线的内容，
        表示是一个块，
        里面包含多行，
        换行符会保留。

**尖括号（>）：换行符会被替换为空格，直到出现空行或者缩进不同的行表示结束**

    name: yaml块
    description: >
        这里属于尖括号的内容，
        表示一个块，这行的内容会拼接到上一行内容。
        
          这行有缩进，会变成换行显示。同时由于上一行是空行，会正常多一个换行输出
          这行有缩进，会变成换行显示。

        这行没有有缩进，由于上一行是空行，也会换行输出

__简单内容本身支持跨行（注意解析器程序是否兼容）__

    description:
        这里是第一行
        这里是第二行

---

## 双引号模式提供转义序列（斜杠：\），单引号不会转义

    unicode: "Sosa did fine.\u263A"
    control: "\b1998\t1999\t2000\n"
    hexesc: "\x0d\x0a is \r\n"
    doublequotes: '"Howdy!" he cried.'
    singlequotes: ' # Not a ''comment''.'
    tie-fighter: '|\-*-/|'

---

## 重复节点，首先用符号逻辑与(&)标记+id，然后用星号（*）引用+id

    lgy: 
      ev: php
      dep: &id-dep 应用中心
    tc:
      dev: java
      dep: *id-dep
    hdl:
      dev: oc
      dep: *id-dep

---

## tags （感叹号+名称），显式标识内容类型

**未标记tags的类型会依据应用程序决定**
    
    # Integers
    canonical: 12345
    hexadecimal: 0xC
    # Floating Point
    fixed: 1230.15
    negative infinity: -.inf
    #Timestamps
    date: 2002-12-14
    iso8601: 2001-12-14t21:59:43.10-05:00
    #booleans
    booleans: [ true, false ]
    #null (空或~或null)
    null:
    #string
    string: '0123456'

**用感叹号显式标识类型，双感叹号（!!）一般用于标识简单类型+扩展类型（binary，omap，set），单感叹号（!）一般用于标识自定义对象类型**

    not-date: !!str 2002-04-28
    - !circle
      center: {x: 73, y: 129}
      radius: 7
      
---

#Markdown 规则简介

**注意事项**

1. 兼容HTML（md文档可以直接写html标签，但是html代码块内不解析markdown语法）
2. 用两个以上空格+回车强制插入&lt;br/&gt;
3. 使用特殊字符原型要使用实体形式书写，如&lt; 和 &amp; 变成 `&lt;` 和 `&amp;`

---

### Markdown 语法介绍

## 标题 (行首插入 1 到 6 个 \# ，对应到标题 H1 到 H6 )

    # 这里是H1
    ## 这里是H2
    
*以上会转换为以下html*
    
    <h1>这里是H1</h1>
    <h2>这里是H2</h2>
    
---

## 列表（无序列表ul：用符号(*)或符号(+)或符号(-)后加空格表示；有序列表ol：用数字加符号点(.)后加空格表示）

*无序列表，相当于html的&lt;ul&gt;&lt;li&gt;&lt;/li&gt;&lt;/ul>&gt;*

    + 这里是列表项1
    * 这里是列表项2
    - 这里是列表项3
    
*以上会转换为以下html*
    
    <ul>
    <li>这里是列表项1</li>
    <li>这里是列表项2</li>
    <li>这里是列表项3</li>
    </ul>
    
*有序列表，相当于html的&lt;ol&gt;&lt;li&gt;&lt;/li&gt;&lt;/ol>&gt;*

    1. 这里是有序列表项1
    2. 这里是有序列表项2
    
*以上会转换为以下html*
    
    <ol>
        <li>这里是有序列表项1</li>
        <li>这里是有序列表项2</li>
    </ol>
    
*列表嵌套，缩进多两个空格*    
    
    + 这里是外层列表项
      - 这里是嵌套列表项
      
*以上会转换为以下html*

    <ul>
        <li>这里是外层列表项</li>
        <ul>
            <li>这里是嵌套列表项</li>
        </ul>
    </ul>    

---

## 区块引用 Blockquotes(符号：&gt;后加空格)

    > 这里是区块演示
    > > 这里是区块嵌套演示

*以上会转换为以下html*
    
    <blockquote>
        <p>这里是区块演示</p>
        <blockquote><p>这里是区块嵌套演示</p></blockquote>
    </blockquote>

---

## 代码块（缩进 4 个空格或是 1 个制表符）
    
    这里是一个

         这是一个代码区块。

*以上会转换为以下html*

    <p>这是一个普通段落：</p>

    <pre><code>这是一个代码区块。
    </code></pre>

__行内代码用反引号（\`）包起来__

    这里有一个输出代码`print("<code></code>")`

*以上会转换为以下html*

    <p>这里有一个输出代码<code>print("<code></code>")</code></p>
    
> __若代码块中包含反引号（\`），则使用多个反引号（\`）包起来__

    这里代码中包含一个反引号``print("<code>`</code>")``
    
*以上会转换为以下html*

    <p>这里代码中包含一个反引号<code>print("<code>`</code>")</code></p>
    
---

## 超链接（包含：行内式和参考式两种形式），**不管是哪一种，链接文字都是用第一个 [方括号] 来标记**

1. 行内式格式：[](),括号()内第一个内容是链接地址，空格后是title内容（title内容可选），其次，若链接地址是同域名下，可以直接用简写相对路径
        
        [这里是链接文字](链接url地址 "可选的title内容")
        [这里是链接文字](/相对路径)

*以上会转换为以下html*
    
    <a href="链接url地址" title="可选的title内容">这里是链接文字</a>
    <a href="/相对路径" title="">这里是链接文字</a>

2. id参考式：[][]，方括号中间可以有空格，第二个方括号是id标识（辨识链接的标记）

        [这里是链接文字][id1]
        [这里是链接文字] [id2]

        [id1]: 链接url地址 "可选的title1"
        [id2]: 链接url地址 (可选的title2)

*以上会转换为以下html*

    <a href="链接url地址" title="可选的title1">这里是链接文字</a>
    <a href="链接url地址" title="可选的title2">这里是链接文字</a>

3. 隐式参考式：[][]，第二个方括号为空，第一个方括号的内容同时代表id标识（辨识链接的标记）

        [这里是链接文字] []

        [这里是链接文字]: 链接url地址

*以上会转换为以下html*
        
    <a href="链接url地址" title="">这里是链接文字</a>

__链接id内容定义的形式为:__

1. 方括号（前面可以选择性地加上至多三个空格来缩进），里面输入链接文字
2. 接着一个冒号
3. 接着一个以上的空格或制表符
4. 接着链接的网址
5. 选择性地接着 title 内容，可以用单引号、双引号或是括弧包着

---

## 图片（目前不能定义图片的宽高）（包含：行内式和参考式两种形式），**不管是哪一种形式，第一个 [方括号] 都是图片alt属性的内容，方括号前是一个感叹号(!)**

1. 行内式：![]()

        ![图片alt](图片地址 "可选title内容")

*以上会转换为以下html*

    <img alt="图片alt" src="图片地址" title="可选title内容"/>

2. 参考式：![][]

        ![图片alt][id1]
        [id1]: 图片地址 "可选title内容"

*以上会转换为以下html*

    <img alt="图片alt" src="图片地址" title="可选title内容"/>

---

## 强调（使用符合\*、_包着），单个符号会转换为html标签：&lt;em&gt;，两个符号会转换为html标签：&lt;strong&gt，**注意符号两边不能有空白，否则会解析为普通字符**

    *单个星号*
    _单个下划线_
    **两个星号**
    __两个下划线__

*以上会转换为以下html*   

    <em>*单个星号*</em>
    <em>_单个下划线_</em>
    <strong>**两个星号**</strong>
    <strong>__两个下划线__</strong>

---

## 分割线（用三个以上的星号、减号、底线来建立一个分隔线，**行内不能有其他东西，星号或减号中间插入空格**）

    * * *
    ***
    *****
    - - -
    ---------------------------------------
    
---

## 转义字符（利用反斜杠来插入一些在语法中有其它意义的符号）

    \   反斜线
    `   反引号
    *   星号
    _   底线
    {}  花括号
    []  方括号
    ()  括弧
    #   井字号
    +   加号
    -   减号
    .   英文句点
    !   惊叹号

## 第三方扩展语法，如表格，图表，公式等参考第三方规则

---

# Swagger-Editor 规则简介（2.0）

__文档格式使用yaml或json，description节点支持GFM（Github Flavored Markdown）语法__

## 注意事项

1. 如果参数的in节点值是'path'，'required'节点必须有，且等于true
2. 如果参数的in节点值是'body'，'schema'节点必须有
3. 如果参数的type节点值是'array'，'items'节点必须有

---

## 支持的数据类型

Common Name | [`type`](#dataTypeType) | [`format`](#dataTypeFormat) | Comments
----------- | ------ | -------- | --------
integer | `integer` | `int32` | signed 32 bits
long | `integer` | `int64` | signed 64 bits
float | `number` | `float` | |
double | `number` | `double` | |
string | `string` | | |
byte | `string` | `byte` | base64 encoded characters
binary | `string` | `binary` | any sequence of octets
boolean | `boolean` | | |
date | `string` | `date` | As defined by `full-date` - [RFC3339](http://xml2rfc.ietf.org/public/rfc/html/rfc3339.html#anchor14)
dateTime | `string` | `date-time` | As defined by `date-time` - [RFC3339](http://xml2rfc.ietf.org/public/rfc/html/rfc3339.html#anchor14)
password | `string` | `password` | Used to hint UIs the input needs to be obscured.

---

## 必须节点

+ swagger
    - 类型：string
    - 描述：必须填写2.0
+ info
    - 描述：API基础数据描述
    - 类型：
    
            {
                "title": "应用标题，必须", 
                "version": "应用API版本，必须"
            }
 
+ paths
    - 描述：每个api的路径
    - 类型：
    
            {
                "get|put|post|delete|options|head|patch": {
                    "parameters": {
                        "name": "参数名称，必须", 
                        "in": "query|header|path|formData|body5个值之一，必须",
                        "type": "参数数据类型，非必须",
                        "description": "参数描述，非必须"
                        "required": false
                    }, 
                    "responses": {
                        "http状态码": {
                            "description": "描述，必须"
                        }, 
                        "default": {
                            "description": "描述，必须"
                        }
                    }
                }
            }
    
---

## 常用根节点

+ host
    - 描述：api服务器请求地址（域名或IP），*不能加上传输协议或子路径*
    - 类型：string
+ schemes
    - 描述：api传输协议
    - 类型：string，(http|https|ws|wss)
+ consumes
    - 描述：请求MIME类型，可以放在根节点（所有paths共享），也可以独立作为每个paths节点的子节点（会覆盖根节点的值）
    - 类型：[mime-type](http://www.freeformatter.com/mime-types-list.html "content-type")数组
+ produces
    - 描述：接受MIME类型，可以放在根节点（所有paths共享），也可以独立作为每个paths节点的子节点（会覆盖根节点的值）
    - 类型：[mime-type](http://www.freeformatter.com/mime-types-list.html "content-type")数组
+ definitions
    - 描述：定义对象数据类型，可用于请求或返回操作
    - 类型：
        
            {
                "model1": {
                    "type": "object",
                    "properties": {
                      "id": {
                        "type": "integer",
                        "format": "int64"
                      },
                      "name": {
                        "type": "string",
                        "description": "昵称"
                      }
                }
            }
+ parameters
    - 描述：定义请求参数对象，并非指定所有请求都有此参数
    - 类型：

            {
                "param1": {
                    "name": "id",
                    "in": "query",
                    "description": "主键id",
                    "required": true,
                    "type": "integer",
                    "format": "int32"
                  }
            }
+ responses
    - 描述：定义返回对象，并非指定所有返回都有此内容
    - 类型：

            {
                "GeneralError": {
                    "description": "General Error",
                    "schema": {
                        "$ref": "#/definitions/GeneralError"
                    }
                }
            }

---

## 几个特殊节点

+ $ref : 引用当前文件或外部文件的对象
    - 当前定义对象obj1：\#/definitions/obj1
    - 外部对象文件obj：http://url/obj\.json
            
+ allOf：一个对象包含某个对象的所有属性
    - model2 包含 model1
    
            {
                "model2": {
                    "allOf": [
                        {
                          "$ref": "#/definitions/model1"
                        },
                        {
                          "type": "object",
                          "required": [
                            "requiredProperty"
                          ],
                          "properties": {
                            "requiredProperty": {
                              "type": "string"
                            }
                          }
                        }
                    ]
                }
            }

+ additonalProperties : 用于对象中包含字典属性（*UI目前不能显示*）
    - string to string 字典例子

            {
              "type": "object",
              "additionalProperties": {
                "type": "string"
              }
            }
            
    - string to model 字典例子

            {
              "type": "object",
              "additionalProperties": {
                "$ref": "#/definitions/object"
              }
            }
            
# 推荐几个有意思的tool

+ [chocolatey](http://chocolatey.org/)
+ [landslide](https://github.com/adamzap/landslide)
+ [pandoc](https://github.com/jgm/pandoc)
+ [vagrant](http://downloads.vagrantup.com/)