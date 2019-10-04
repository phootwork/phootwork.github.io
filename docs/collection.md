# Phoootwork collection

Phootowrk [collection](https://github.com/phootwork/collection) is a library inspired by java `java.util.Collection`,
 which provides some collections for PHP.

These collections are available:

- Lists:
    - `ArrayList` - provides a List
    - `Set` - provides a Set (only unique elements)
    - `Queue` - provides a Queue (FIFO - first in first out)
    - `Stack` - provides a Stack (FILO - first in last out)
- Maps:
    - `Map` - provides a Map

All classes contain phpdoc, so your lovely IDE will provide content assist on all the methods.

## Installation

Installation via composer:

```
composer require phootwork/collection
```

## Functional Flavour

Some method of the collections accepts a callback (usually a closure) as parameter; the passed function receives an
element of the collection as parameter, i.e.:

```php
<?php declare(strict_types=1);

$collection->someMethod(function(ElementType $element) {
    // some element manipulation
});
```
You can also pass a value to the function, as an additional parameter and you'll find it as the second parameter of your
function:

```php
<?php declare(strict_types=1);

$collection->someMethod('a value', function(ElementType $element, $query) {
    // $query contains 'a value' string
    return $element->getSomething() === $query;
});
```
!!! note
    Be careful: the parameters order is mandatory.
    
See other examples along this document.

## ArrayList

An *ArrayList* is a collection of data that can have same or different types and can be objects, too. Each element of the 
*ArrayList* collection has an index, similar to the normal array.
When it's important for you to access the index (sometimes called position) of the elements into the collection, 
then `ArrayList` is the right choice for you.

You can create an ArrayList collection by passing an array or an [Iterator](https://www.php.net/manual/en/class.iterator.php)
 object to the constructor:

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$fruits = new ArrayList(['apple', 'pear', 'peach']);
```

Now you're ready to manipulate your collection. You can see all the available methods in the [documentation api](api/phootwork/collection/ArrayList.html)

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$fruits = new ArrayList(['apple', 'pear', 'peach']);

$fruits->size();     // (int) 3
$fruits->isEmpty(); // (bool) false
$fruits->contains('pear'); // (bool) true

// Much more into the api
```

### Addiction

You can add one or more elements to the *ArrayList* via the `add` method:

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$fruits = new ArrayList(['apple', 'pear', 'peach']);

$fruits->add('banana');
$fruits->add('apricot', 'watermelon', 'lemon');

$fruits->toArray(); // ['apple', 'pear', 'peach', 'banana', 'apricot', 'watermelon', 'lemon']
```

As you can see in the example above, the `add` method appends an element at the end of the collection.
If you want to define the position of the element you can use the `insert` method; remember that the position starts from 0
(exactly as the index of an array):

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$fruits = new ArrayList(['apple', 'pear', 'peach']);

$fruits->insert('banana', 1);

$fruits->toArray(); // ['apple', 'banana', 'pear', 'peach']
```
The insertion of an element, at a given position, moves the next elements of one position.

!!! warning
    If your *ArrayList* models an associative array (the indexes are strings) the insertion **overwrite** the element
    at the given position. See the example below:

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$fruits = new ArrayList(['fruit1' => 'apple', 'fruit2' => 'pear', 'fruit3' => 'peach']);

$fruits->insert('banana', 'fruit1');

$fruits->toArray(); // ['fruit1' => 'banana', 'fruit2' => 'pear', 'fruit3' => 'peach']
```

### Map

Applies the callback to the elements of a collection and returns an `ArrayList` object containing the result of the manipulation.

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$list = new ArrayList([2, 3, 4]);
$pow2 = $list->map(function (int $element): int {
	return $element * $element;
});
print_r($pow2); // [4, 9, 16]
```

### Each

Iteration with a callback function without returning anything.

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$list = new ArrayList(range(1, 10));
$list->each(function(int $elem): void {
	// do something with $elem
});
```

### Reduce

Iteratively reduce a collection to a single value using a callback function.

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$list = new ArrayList(range(1, 10));
$sum = $list->reduce(function(int $a, int $b) {
	return $a + $b;
});
echo $sum; // 55
```

### Filter

Filters elements of a collection using a callback function.

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$list = new ArrayList([1, 2, 3, 4, 5, 6]);
$odd = $list->filter(function (int $item): int {
	return $item & 1;
});
print_r($odd); // [1, 3, 5]
```

### Search for Existence

To search your collection pass in a function that takes an element as first item. If you pass a parameter to the
`search()` function, this will be the second parameter to your passed function (useful if you don't want to use an 
anonymous function every time).

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$list = new ArrayList(range(1, 10));

$found = $list->search(function (int $elem): bool {
	return $elem === 4;
});

// use a function that takes the query as second parameter
// $query will have the value 4 as you've just passed in
$found = $list->search(4, function (int $elem, int $query): bool {
	return $elem === $query;
});
```

### Find an Element

While `search()` only tells you some element is present, if you want to return this element use the `find()` rsp. `findAll()`
method.

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$fruits = new ArrayList(['apple', 'banana', 'pine', 'banana', 'ananas']);
$pine = $fruits->find(function (string $elem): bool {
	return $elem === 'pine';
});

$bananas = $fruits->findAll(function(string $elem): bool {
	return $elem === 'banana';
});
```

### Find an Index

If you are interested in the index of a search, you can use `findIndex()` also there is `findLastIndex()` which searches
the list from back to front.

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$fruits = new ArrayList(['apple', 'banana', 'pine', 'banana', 'ananas']);
$index = $fruits->findIndex(function (string $elem): bool {
	return $elem == 'banana';
});

echo $index; // 1

$lastIndex = $fruits->findLastIndex(function (string $elem): bool {
	return $elem == 'banana';
});
echo $lastIndex; // 3
```

### Reversing

You can swap the order of your elements with `reverse()`.

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$list = new ArrayList([5, 2, 8, 3, 9, 4, 6, 1, 7, 10]);
$reversed = $list->reverse();
print_r($reversed); // [10, 7, 1, 6, 4, 9, 3, 8, 2, 5]
```

### Simple Sorting

There is built in support for running `sort()`:

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$unsorted = new ArrayList([5, 2, 8, 3, 9, 4, 6, 1, 7, 10]);
$sorted = $unsorted->sort();
print_r($sorted); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

### Bring your own Comparison

Of course you can use your own comparison to sort a list. In the example below you ca see a function in action. The 
returned value must be an integer: 0 if the first and the second value are equals, -1 if the first is minor of the second,
1 if the first is major of the second.

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$unsorted = new ArrayList([5, 2, 8, 3, 9, 4, 6, 1, 7, 10]);
$cmp = function (int $a, int $b): int {
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
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;
use phootwork\lang\ComparableComparator;

$unsorted = new ArrayList(['x', 'c', 'a', 't', 'm']);
$sorted = $unsorted->sort(new ComparableComparator());
print_r($sorted); // ['a', 'c', 'm', 't', 'x']
```
