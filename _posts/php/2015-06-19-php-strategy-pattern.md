---
layout: post
title: php设计模式之策略模式
category: php
tags: [php , strategy , pattern]
description: php策略模式简单介绍与使用
---

## 什么是策略模式
策略模式是对算法的包装，是把使用算法的责任和算法本身分割开来，委派给不同的对象管理。策略模式通常把一个系列的算法包装到一系列的策略类里面，作为一 个抽象策略类的子类。用一句话来说，就是：“准备一组算法，并将每一个算法封装起来，使得它们可以互换”。下面就以一个示意性的实现讲解策略模式实例的结构

## UML图
![策略模式][1]

## 组成部分

 1. 环境类(Context)
 
> 用一个ConcreteStrategy对象来配置,维护一个对Strategy对象的引用。可定义一个接口来让Strategy访问它的数据。

 2. 抽象策略类(Strategy)
 
> 定义所有支持的算法的公共接口,Context使用这个接口来调用某ConcreteStrategy定义的算法。

 3. 具体策略类(ConcreteStrategy)
 
> 以Strategy接口实现某具体算法。

## 简单代码实现 (充值赠送示例)
```php
interface IPayStrategy {
	public function algorithmInterface();
}
class ConcretePayStrategyA implements IPayStrategy {
	public function algorithmInterface() {
		return "A:充值赠送积分";
	}
}
final class ConcretePayStrategyB implements IPayStrategy {
	public function algorithmInterface() {
		return "B:充值赠送论坛币";
	}
}
final class ConcretePayStrategyC implements IPayStrategy {
	public function algorithmInterface() {
		return "C:充值赠送抽奖次数";
	}
}
final class PayContext {
	private $_payStrategy;
	public function __construct(IPayStrategy $strategy) {
		$this->_payStrategy = $strategy;
	}
	public function contextInterface() {
		$this->_payStrategy->algorithmInterface ();
	}
}

// 使用
$c = new PayContext(new ConcretePayStrategyA());
$c->contextInterface();
```


  [1]: http://chuantu.biz/t2/10/1434699928x1822619110.png