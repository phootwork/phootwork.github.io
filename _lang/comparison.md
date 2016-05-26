---
layout: package
title: Comparison
---

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
