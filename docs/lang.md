Missing PHP language constructs

Installation via composer:

```
composer require phootwork/lang
```

## Goals

- Provide common but missing php classes
- Objects for native php constructs
- Consistent API
- Inspired by `java.lang`, `java.util` and  [`Stringy`](https://github.com/danielstjules/Stringy)

Arrays are key data structures for both map and list like types in php.

## Arrayable

Getting an array from an object is not easy to know. That's why there is an
`Arrayable` interface which has only one method: `toArray()` and helps to be put on
objects that will return an array. Use it for type-hinting or check the instance of
an object whether is arrayable.

## ArrayObject

An `ArrayObject` is a wrapper around a native php array but provides consistent
methods instead of weird functions and parameter order that are hard to read in
your code.

Alternatively you can use the <a href="/collection">Collection</a> package.

Comparison in php is mainly supported on a function level, e.g. `sort()` (and other array sorting functions). Also php provides `usort()` to pass in your own comparison code, there is no such thing on a class level.

## Comparable

Implement the `Comparable` interface on those objects you want to compare with others. You can now compare your object to another one:

```php
<?php
use phootwork\lang\Comparable;

class MeasurementPoint implements Comparable {

	// ...

	public function compareTo($comparison) {
		// comparison logic here
	}
}

$p1 = new MeasurementPoint();
$p2 = new MeasurementPoint();
$p2->compareTo($p1);
```

## Comparator

A `Comparator` is somebody who compares two values with each other. Use it to compare your objects.

```php
<?php
use phootwork\lang\Comparator;

class MeasurementComparator implements Comparator {

	public function compare($a, $b) {
		return $a->compareTo($b);
	}
}

$p1 = new MeasurementPoint();
$p2 = new MeasurementPoint();
$comparator = new MeasurementComparator();
$comparator->compare($a, $b);
```

## ComparableComparator

The `ComparableComparator` contains the same functionality as the `MeasurementComparator` above. If you don't have a comparator at hand and need one, the `ComparableComparator` is there for you.


For string manipulation php offers a lot of functions with inconsistent naming scheme and parameter ordering. Here is the `Text` (since `string` is a reserved word in php7) class to clear this mess.

## Instantiation

Instantiation can happen in two ways.

1) Regular way with the `new` keyword:

```php
<?php
use phootwork\lang\Text;

$str = new Text('my string');
```

2) Using the static create method:

```php
<?php
use phootwork\lang\Text;

$str = Text::create('my string');
```

which is mostly helpful in one-line code (such as if statements).

## Method Chaining

Method chaining is possible with the `Text` class:

```php
<?php
$str = new Text('MyFunNyStrIng');
$str->lower()->upperFirst(); // Myfunnystring (see also Text::capitalize())
```

## Examples

Some examples where the `Text` class really shines:

1) In `if` statements:

```php
<?php

if (Text::create($str)->startsWith('hello')) {
	// ...
}
```
