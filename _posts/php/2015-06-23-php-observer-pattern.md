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

- 抽象主题（Subject）
 
> 它把所有观察者对象的引用保存到一个聚集里，每个主题都可以有任何数量的观察者。抽象主题提供一个接口，可以增加和删除观察者对象。

- 具体主题（ConcreteSubject）
 
> 将有关状态存入具体观察者对象；在具体主题内部状态改变时，给所有登记过的观察者发出通知。

- 抽象观察者（Observer）
 
> 为所有的具体观察者定义一个接口，在得到主题通知时更新自己。

- 具体观察者（ConcreteObserver
 
> 实现抽象观察者角色所要求的更新接口，以便使本身的状态与主题状态协调。

## 优缺点

- 优点

> 当两个对象之间送耦合，他们依然可以交互，但是不太清楚彼此的细节。观察者模式提供了一种对象设计，让主题和观察者之间送耦合。主题所知道只是一个具体的观察者列表，每一个具体观察者都符合一个抽象观察者的接口。主题并不认识任何一个具体的观察者，它只知道他们都有一个共同的接口。

> 观察者模式支持广播通讯。被观察者会向所有的登记过的观察者发出通知。

- 缺点

> 如果一个被观察者对象有很多的直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间。

> 如果在被观察者之间有循环依赖的话，被观察者会触发它们之间进行循环调用，导致系统崩溃。在使用观察者模式是要特别注意这一点。

> 如果对观察者的通知是通过另外的线程进行异步投递的话，系统必须保证投递是以自恰的方式进行的。

> 虽然观察者模式可以随时使观察者知道所观察的对象发生了变化，但是观察者模式没有相应的机制使观察者知道所观察的对象是怎么发生变化的。 

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
