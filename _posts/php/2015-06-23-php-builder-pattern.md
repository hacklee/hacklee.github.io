---
layout: post
title: php设计模式之建造者模式
category: php
tags: [php , observer , pattern]
description: php建造者模式简单介绍与使用
---

## 什么是建造者模式
观察者模式是一种创建型模式，将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。(建造者模式的实质是解耦组装过程和创建具体部件，使得我们不用去关心每个部件是如何组装的。)

## UML图
![观察者模式UML图][1]

## 组成部分

- 抽象建造者（Builder）
 
> 给出一个抽象接口，以规范产品对象的各个组成成分的建造。这个接口规定要实现复杂对象的哪些部分的创建，并不涉及具体的对象部件的创建。

- 具体建造者（ConcreteBuilder）
 
> 实现Builder接口，针对不同的商业逻辑，具体化复杂对象的各部分的创建。 在建造过程完成后，提供产品的实例。

- 导演类（Director）
 
> 调用具体建造者来创建复杂对象的各个部分，在指导者中不涉及具体产品的信息，只负责保证对象各部分完整创建或按某种顺序创建。

- 产品类 (Product)
 
> 一般是一个较为复杂的对象，也就是说创建对象的过程比较复杂，一般会有比较多的代码量。在本类图中，产品类是一个具体的类，而非抽象类。实际编程中，产品类可以是由一个抽象类与它的不同实现组成，也可以是由多个抽象类与他们的实现组成。

## 优缺点

- 优点

> 隔离了构建的步骤和具体的实现，为产品的具体实现提供了灵活度。

> 封装和抽象了每个步骤的实现，实现了依赖倒转原则。

> 封装了具体的步骤，减少了代码的冗余。

- 缺点

> 建造者接口的修改会导致所有执行类的修改

## 简单代码实现 (html页面创建)
```php
interface IBuilder {
	public function buildPartA();
	public function buildPartB();
	public function buildPartC();
	public function getResult();
}
final class ConcretePageBuilder implements IBuilder {
	private $_pageProduct;
	public function __construct() {
		$this->_pageProduct = new PageProduct ();
	}
	public function buildPartA() {
		$this->_pageProduct->addContent ( "<div>head</div>" );
	}
	public function buildPartB() {
		$this->_pageProduct->addContent ( "<div>body</div>" );
	}
	public function buildPartC() {
		$this->_pageProduct->addContent ( "<div>foot</div>" );
	}
	public function getResult() {
		return $this->_pageProduct;
	}
}
final class PageProduct {
	private $_html = array ();
	public function addContent($html) {
		array_push ( $this->_html, $html );
	}
}
final class Director{
	private $_builder;
	public function __construct(IBuilder $builder) {
		$this->_builder = $builder;
	}
	public function build() {
		$this->_builder->buildPartA();
		$this->_builder->buildPartB();
		$this->_builder->buildPartC();
	}
}
$buidler = new ConcretePageBuilder();
$d = new Director($buidler);
$d->build();
$product = $buidler->getResult();
```

  [1]: http://chuantu.biz/t2/10/1435056435x-1376440080.gif
