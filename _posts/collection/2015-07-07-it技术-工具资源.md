---
layout: post
title: php设计模式之单例模式
category: php
tags: [php , singleton , pattern]
description: php单例模式简单介绍与使用
---

## 什么是单例模式
单例模式是一种常用的软件设计模式。在它的核心结构中只包含一个被称为单例类的特殊类。通过单例模式可以保证系统中一个类只有一个实例而且该实例易于外界访问，从而方便对实例个数的控制并节约系统资源。如果希望在系统中某个类的对象只能存在一个，单例模式是最好的解决方案。

## UML图
![单例模式UML图][1]

## 注意事项

- 用静态变量保存类中的唯一一个实例

- 用公开的静态方法为整个系统提供实例

- 防止外部通过new 、clone等复制实例

## 简单代码实现（数据库操作）

```php
final class Mysql {
    private static $_instance = array ();
    private $_mysqli;
    private $_last_error;
    private function __construct($config) {
        $this->_mysqli = new \mysqli ( 
                $config ['ip'], 
                $config ['user'], 
                $config ['pwd'], 
                $config ['dbname'] 
        );
        if ($this->_mysqli->connect_errno) {
            $this->_last_error = $this->_mysqli->connect_errno;
        }
        $this->_mysqli->select_db ( $config ['dbname'] );
        $this->_mysqli->query ( "SET NAMES '{$config ['charset']}'" );
    }
    public static function getInstance($db, $config) {
        if (! isset ( static::$_instance [$db] )) {
            static::$_instance [$db] = new static ( $config );
        }
        return static::$_instance [$db];
    }
    public function getError() {
    	return $this->_last_error;
    }
    public function __destruct() {
        $this->_mysqli->close ();
    }
    private function __clone() {
        throw new \Exception('class is singleton');
    }
}
```

  [1]: http://chuantu.biz/t2/10/1433907163x1822611417.png
