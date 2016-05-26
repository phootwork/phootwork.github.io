---
layout: package
title: Goodies
---

## Map

 Applies the callback to the elements of a collection.

```php
<?php
$list = new ArrayList([2, 3, 4]);
$pow2 = $list->map(function ($item) {
	return $item * $item;
});
print_r($pow2); // [4, 9, 16]
```

## Reduce

Iteratively reduce a collection to a single value using a callback function.

```php
<?php
$list = new ArrayList(range(1, 10));
$sum = $list->reduce(function($a, $b) {
	return $a + $b;
});
echo $sum; // 55
```

## Filter

Filters elements of a collection using a callback function.

```php
<?php
$list = new ArrayList([1, 2, 3, 4, 5, 6]);
$odd = $list->filter(function ($item) {
	return $item & 1;
});
print_r($odd); // [1, 3, 5]
```


## Each

Iteration with a callback function.

```php
<?php
$list = new ArrayList(range(1, 10));
$list->each(function($elem) {
	// do something with $elem
});
```
