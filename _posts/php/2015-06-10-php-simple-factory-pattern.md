---
layout: post
title: php设计模式之简单工厂模式
category: php
tags: [php , simple-factory , pattern]
description: php简单工厂模式简单介绍与使用
---

## 什么是简单工厂模式
简单工厂模式是属于创建型模式，又叫做静态工厂方法（Static Factory Method）模式，简单工厂模式是由一个工厂对象决定创建出哪一种产品类的实例。

## UML图
![单例模式UML图][1]

## 组成要素

- 工厂类

> 工作模式的核心，负责产品类实例的创建返回

- 产品抽象类或接口

> 抽象所有产品类的共同属性或方法

- 产品类

> 每个产品的具体实现

## 简单代码实现（充值接口操作实例）

```php
abstract class PayBaseApi {
	abstract function getPayUrl();
}
final class AliPayApi extends PayBaseApi {
	public function getPayUrl() {
		return "支付宝支付";
	}
}
final class YeePayApi extends PayBaseApi {
	public function getPayUrl() {
		return "易宝支付";
	}
}
final class PayFactory {
	public static function getPayProduct($type) {
		$objClassName = ucfirst ( $type ) . "PayApi";
		switch ($type) {
			case 'ali' :
			case 'yee' :
				return new $objClassName ();
			default :
				throw new \Exception ( "no product" );
		}
	}
}
```

  [1]: http://chuantu.biz/t2/10/1433916782x1822614171.png