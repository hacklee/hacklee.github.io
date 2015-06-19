---
layout: post
title: Laravel4.0以上版本使用Asset管理资源
category: php
tags: [php , laravel , framework , Asset]
description: 在Laravel4.0以上版本使用Asset类管理静态资源
---

## Asset
在框架Laravel3.x中，有一个[asset.php][1]，方便我们管理styles和scripts。但是从版本4开始，官方已经去掉这个类。不过有时候这个类还是蛮好的，下面来看看怎么在4.0以上版本使用。

## Asset类中主要用到的两个依赖
从github源码中[asset.php][2]，我们可以看到Asset主要用到Bundle（用于返回加载资源路径）和HTML（生成页面html代码）两个依赖，但是bundle在4.0以上版本已经没有了，我们可以去掉，然后把方法逻辑根据当前项目重新实现。HTML可以用composer资源包‘laravelcollective/html’。

## 实现步骤

+ 加载使用Html
> 参考官网文档[laravelcollective/html][3]
 
+ 修改asset.php里面的path方法
 
```php
    public function path($source)
	{
	    //根据自己的项目实现
	    return "http://xxxxx.com/{$source}";
		//return Bundle::assets($this->bundle).$source;
	}
```

```php
	protected function asset($group, $name)
	{
		if ( ! isset($this->assets[$group][$name])) return '';
		$asset = $this->assets[$group][$name];
		// If the bundle source is not a complete URL, we will go ahead and prepend
		// the bundle's asset path to the source provided with the asset. This will
		// ensure that we attach the correct path to the asset.
		if (filter_var($asset['source'], FILTER_VALIDATE_URL) === false)
		{
			$asset['source'] = $this->path($asset['source']);
		}
		/此处的HTML要改成1中加载的html的alias配置的名字Html一样。且前面加上\或者使用命名空间use Html
		//return HTML::$group($asset['source'], $asset['attributes']);
		return \Html::$group($asset['source'], $asset['attributes']);
	}
```
+ 把asset.php放到项目中，注意修改namespace，用composer自动加载
 
+ 在项目的配置文件app.php下的aliases节点加上DI反射配置
> 如:'Asset' 	=> Import\Lib\Assets\Asset::class (此处为实际项目中的Asset类文件路径)

+ 有了上面的步骤，就可以跟Laravel3一样使用Asset操作了。有点区别就是前面加\或者使用命名空间 use Asset（views页面不用加）;
 
```php
//加载
\Asset::style('default','css/default.css');
\Asset::style('di','css/di.css','default');

\Asset::script('default','js/jquery.js');

//输出
\Asset::styles();
\Asset::scripts();
``` 

  [1]: https://github.com/laravel/laravel/blob/3.0/laravel/asset.php
  [2]: https://github.com/laravel/laravel/blob/3.0/laravel/asset.php
  [3]: http://laravelcollective.com/docs/5.0/html