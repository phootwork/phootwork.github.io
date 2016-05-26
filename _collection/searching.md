---
layout: package
title: Searching
---

## Search for Existance

To search your collection pass in a function that takes an element as first item. If you pass a parameter to the `search()` function, this will be the second parameter to your passed function (useful if you don't want to use an anonymous function every time).

```php
<?php
$list = new ArrayList(range(1, 10));

$found = $list->search(function ($elem) {
	return $elem == 4;
});

// use a function that takes the query as second parameter
// $query will have the value 4 as you've just passed in
$found = $list->search(4, function ($elem, $query) {
	return $elem == $query;
});
```

## Find an Element

While `search()` only tells you some element is present, if you want to return this element use the `find()` rsp. `findAll()` method.

```php
<?php
$fruits = new ArrayList(['apple', 'banana', 'pine', 'banana', 'ananas']);
$pine = $fruits->find(function ($elem) {
	return $elem == 'pine';
});

$bananas = $fruits->findAll(function($elem) {
	return $elem == 'banana';
});
```

## Find an Index

If you are interessted in the index of a search, you can use `findIndex()` also there is `findLastIndex()` which searches the list from back to front.

```php
<?php
$fruits = new ArrayList(['apple', 'banana', 'pine', 'banana', 'ananas']);
$index = $fruits->findIndex(function ($elem) {
	return $elem == 'banana';
});
echo $index; // 1

$lastIndex = $fruits->findLastIndex(function ($elem) {
	return $elem == 'banana';
});
echo $lastIndex; // 3
```
