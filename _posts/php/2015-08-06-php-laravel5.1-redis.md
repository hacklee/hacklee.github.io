---
layout: post
title: 在Laravel5.1中使用Reids
category: php
tags: [redis , laravel , redis , laravel5.x]
description: 在Laravel5.1中使用Redis。
---

### Redis
Redis是一款以BSD开源协议发行的高性能键值(key-value)存储系统（cache and store）。通常把Redis称为数据结构服务器，因为值（value）可以是 字符串(strings), 哈希(hashes), 列表(list), 集合(sets),有序集合(sorted sets),位图(bitmaps)和HyperLogLogs等类型。

### Laravel5中使用Redis
在Laravel5.1中，Redis操作实现依赖于predis/predis这个composer包。
同时支持把Redis配置为Laravel项目默认的会话和缓存驱动。


### 安装[predis/predis][1]依赖

```
composer require predis/predis
```

### 配置使用Redis

- 使用Redis作为缓存驱动

> 在config/cache.php配置Redis作为缓存驱动
> 其中connection的值对应config/database.php下redis选项的server配置键值（key）

```
'default' => 'redis',

'stores' => [
    'redis' => [
        'driver' => 'redis',
        'connection' => 'default',
    ]
]
```

- 在config/session.php配置Redis作为会话驱动

```
driver => 'redis'
```

- 在config/database.php配置Redis实例信息

> Laravel的Redis操作默认连接default实例
> cluster配置是否是集群
> 以下包含两个Redis实例的配置，defaut和server2,default连接本地数据库0，server2连接本地数据库1

```
'redis' => [

        'cluster' => false,

        'default' => [
            'host'     => '127.0.0.1',
            'port'     => 6379,
            'database' => 0,
        ],

    	'server2' => [
    			'host'     => '127.0.0.1',
    			'port'     => 6379,
    			'database' => 1,
    	],    	
    ],
```

### Redis使用

- 首先检测当前php环境是否安装了redis扩展

```
php -m | grep redis
```

- 若发现安装了redis扩展，要修改Laravel的Redis别名，不然操作的是扩展类

> Redis的别名配置在config/app.php文件，例如以下修改别名为PRedis

```
'PRedis'     => Illuminate\Support\Facades\Redis::class
```

### 操作Redis

- 在使用的文件头加上命名空间PRedis,这是Laravel框架的亮点之一IOC,这样操作等同于$app->make('redis')，直接创建别名中配置的类对象使用。

```
use PRedis;
```

- 获取Redis实例

> 获取默认实例

```
\$defaultServer = PRedis::connection(); //等价于\$defaultServer = PRedis::connection('default');
```

> 获取其他特定实例，如果获取server2

```
$server2Redis = PRedis::connection('server2');
```

- 直接用静态方法操作默认实例

```
PRedis::set('key','value');
$val = PRedis::get('key');
$values = PRedis::command('lrange', ['name', 5, 10]);
PRedis::pipeline(function($pipe){
    for ($i = 0; $i < 10; $i++){
        $pipe->set("key:$i", $i);
    }
});
```

**若服务器php没有安装redis扩展，则可以直接使用Redis**


  [1]: https://github.com/nrk/predis