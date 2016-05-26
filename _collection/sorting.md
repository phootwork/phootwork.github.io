---
layout: package
title: Sorting
---

## Reversing

You can swap the order of your elements with `reverse()`.

```php
<?php
$list = new ArrayList([5, 2, 8, 3, 9, 4, 6, 1, 7, 10]);
$reversed = $list->reverse();
print_r($reversed); // [10, 7, 1, 6, 4, 9, 3, 8, 2, 5]
```

## Simple Sorting

There is built in support for running `sort()`:

```php
<?php
$unsorted = new ArrayList([5, 2, 8, 3, 9, 4, 6, 1, 7, 10]);
$sorted = $unsorted->sort();
print_r($sorted); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

## Bring your own Comparison

Of course you can use your own comparison to sort a list. For example bring a function.

```php
<?php
$unsorted = new ArrayList([5, 2, 8, 3, 9, 4, 6, 1, 7, 10]);
$cmp = function ($a, $b) {
	if ($a == $b) {
		return 0;
	}
	return ($a < $b) ? -1 : 1;
};
$sorted = $unsorted->sort($cmp);
print_r($sorted); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

Or you can bring in any [`Comparator`](/lang/comparison). So good, there is a `ComparableComparator` provided by `phootwork/lang` we can use.

```php
<?php
$unsorted = new ArrayList(['x', 'c', 'a', 't', 'm']);
$sorted = $unsorted->sort(new ComparableComparator());
print_r($sorted); // ['a', 'c', 'm', 't', 'x']
```
