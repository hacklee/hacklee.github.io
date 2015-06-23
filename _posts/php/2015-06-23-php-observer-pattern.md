---
layout: post
title: php设计模式之观察者模式
category: php
tags: [php , observer , pattern]
description: php观察者模式简单介绍与使用
---

## 什么是观察者模式
观察者模式是一种行为型模式，它定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。又称为发布-订阅（Publish-Subscribe）模式、模型-视图（Model-View）模式、源-监听（Source-Listener）模式、或从属者(Dependents)模式。

## UML图
![观察者模式UML图][1]

## 组成部分

 1. 抽象主题（Subject）
 
> 它把所有观察者对象的引用保存到一个聚集里，每个主题都可以有任何数量的观察者。抽象主题提供一个接口，可以增加和删除观察者对象。

 2. 具体主题（ConcreteSubject）
 
> 将有关状态存入具体观察者对象；在具体主题内部状态改变时，给所有登记过的观察者发出通知。

 3. 抽象观察者（Observer）
 
> 为所有的具体观察者定义一个接口，在得到主题通知时更新自己。

 4. 具体观察者（ConcreteObserver
 
> 实现抽象观察者角色所要求的更新接口，以便使本身的状态与主题状态协调。

## 简单代码实现
```php
interface IObserver {
	public function update();
}
interface ISubject {
	public function attach(IObserver $obs);
	public function detach(IObserver $obs);
	public function notifyObservers();
}
final class ConcreteObserverA implements IObserver {
	public function update() {
		return 'ConcreteObserverA';
	}
}
final class ConcreteSubjectA implements ISubject {
	private static $_observers = array ();
	public function attach(IObserver $obs) {
		static::$_observers [] = $obs;
	}
	public function detach(IObserver $obs) {
		$key = array_search ( $obs, static::$_observers );
		if ($key !== false) {
			unset ( static::$_observers [$key] );
		}
	}
	public function notifyObservers() {
		foreach (static::$_observers as $obs){
			$obs->update();
		}
	}
}
$subject = new ConcreteSubjectA;
$oba = new ConcreteObserverA;
$subject->attach($oba);
$subject->notifyObservers();
$subject->detach($oba);
```

  [1]: http://chuantu.biz/t2/10/1435048144x-1376440080.png
