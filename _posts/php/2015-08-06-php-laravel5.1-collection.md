---
layout: post
title: 在Laravel5.x中使用集合
category: php
tags: [redis , laravel , redis , laravel5.x]
description: 在Laravel5.x中使用集合collection。
---

### Laravel集合:Collection
集合类的源码路径在：laravel\framework\src\Illuminate\Support\Collection.php。命名空间:Illuminate\Support。
Collection提供了两种方法封装数组数据，从而对数组进行流畅、方便的链式操作。同时，Laravel的helper.php提供了一个辅助方法方法collect，快速将数组转换为集合。

### 将数组转换为集合

- 利用类的构造函数转换

```
$collect = new Collection([]);
```

- 利用类的静态make方法转换

```
$collect = Collection::make([]);
```

- 利用辅助方法collect转换

```
$collect = collect([]);
```

### Collection类除了封装了一部分原生array_xxxx数组方法,还提供了很多方法方便我们对数组数据进行帅选和重组。

- 所有封装方法列表

> all,chunk,collapse,contains,count,diff,each,filter,first,flatten,flip,
> forget,forPage,get,groupBy,has,implode,intersect,isEmpty,keyBy,keys,
> last,map,merge,pluck,pop,prepend,pull,push,put,random,reduce,reject,
> reverse,search,shift,shuffle,slice,sort,sortBy,sortByDesc,splice,sum,
> take,toArray,toJson,transform,unique,values,where,whereLoose,zip

- 假如我们要对一个数组的所有值value排序后转换为json，以前我们可能这样做

```
$arr = ['s','o','r','t'];
sort($arr);
$json = json_encode($arr,$option);
```

- 转换为集合链式操作

```
$json = collect($arr)->sort()->toJson($option);
```

### 下面是Collection类中封装的几个比较有意思的方法

>  **有个主意的地方，collection中基本所有的封装方法，都是返回一个新的集合对象，需要通过all()或toArray()的方式再转换为数组等**

- forPage(\$page, \$perPage) 分页，返回截取某页数据的新集合

> \$page : 所在页码(从1开始)
> \$perPage : 每页的数据长度

```
$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9])->forPage(2, 3);

$collection->all();
// [4, 5, 6]
```

- groupBy(\$groupBy, \$preserveKeys = false) 分组，按照某个key对数组进行分组

> \$groupBy ： 数组key或者回调函数
> \$preserveKeys : 是否保持key不变(当为true时，key的值为在原数组所在位置索引)

```
$arr = [
    ['account_id' => 'account-x10', 'product' => 'Chair'],
    ['account_id' => 'account-x10', 'product' => 'Bookcase'],
    ['account_id' => 'account-x11', 'product' => 'Desk'],
];

$grouped = collect($arr)->groupBy('account_id')->toArray();
/*
    [
        'account-x10' => [
            ['account_id' => 'account-x10', 'product' => 'Chair'],
            ['account_id' => 'account-x10', 'product' => 'Bookcase'],
        ],
        'account-x11' => [
            ['account_id' => 'account-x11', 'product' => 'Desk'],
        ],
    ]
*/

// 回调
$grouped = collect($arr)->groupBy(function ($item, $key) {
    return substr($item['account_id'], -3);
})->toArray();

/*
    [
        'x10' => [
        
            ['account_id' => 'account-x10', 'product' => 'Chair'],
            ['account_id' => 'account-x10', 'product' => 'Bookcase'],
        ],
        'x11' => [
            ['account_id' => 'account-x11', 'product' => 'Desk'],
        ],
    ]
*/
```

- reject($callback) 帅选集合中判断条件为假的数据组成的集合

> \$callback ： 条件判断回调函数

```
$collection = collect([1, 2, 3, 4]);

$filtered = $collection->reject(function ($item) {
    // 相当于返回<=2的数据
    return $item > 2;
});

$filtered->all();
// [1, 2]
```

- sortBy(\$callback, \$options = SORT_REGULAR, \$descending = false) 排序

> \$callback ： 数组key或者回调函数
> \$options ： sortBy的排序是使用asort或arsort实现的，因此\$options的参考值和asort的\$sort_flags参考值一样，php官网文档地址:[http://php.net/manual/zh/function.asort.php][1]
> \$descending ： 是否为降序排序

```
$collection = collect([
    ['name' => 'Desk', 'colors' => ['Black', 'Mahogany']],
    ['name' => 'Chair', 'colors' => ['Black']],
    ['name' => 'Bookcase', 'colors' => ['Red', 'Beige', 'Brown']],
]);

$sorted = $collection->sortBy(function ($product, $key) {
    return count($product['colors']);
});

$sorted->values()->all();

/*
    [
        ['name' => 'Chair', 'colors' => ['Black']],
        ['name' => 'Desk', 'colors' => ['Black', 'Mahogany']],
        ['name' => 'Bookcase', 'colors' => ['Red', 'Beige', 'Brown']],
    ]
*/
```

- zip($items) 组合，将一个或多个数组组合成一个新的集合

> \$items ： 要组合的数组

```
$collection = collect(['Chair', 'Desk']);

$zipped = $collection->zip([100, 200]);

$zipped->toArray();

// [['Chair', 100], ['Desk', 200]]
```



  [1]: http://php.net/manual/zh/function.asort.php