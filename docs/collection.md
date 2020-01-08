# Phoootwork collection

Phootowrk [collection](https://github.com/phootwork/collection) is a library inspired by java `java.util.Collection`,
which provides some collections for PHP.

These collections are available:

- Lists:
    - `ArrayList` - provides a List
    - `Set` - provides a Set (only unique elements)
    - `Stack` - provides a Stack (FILO - first in last out)
    - `Queue` - provides a Queue (FIFO - first in first out)
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

An [ArrayList](api/phootwork/collection/ArrayList.html) is a collection of data that can have same or different types and can be objects, too. Each element of the 
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

Now you're ready to manipulate your collection:

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$fruits = new ArrayList(['apple', 'pear', 'peach']);

$fruits->size();     // (int) 3
$fruits->isEmpty(); // (bool) false
$fruits->contains('pear'); // (bool) true

// Much more into the api documentation
```

### Addition

You can add one or more elements to the *ArrayList* via the [add()](api/phootwork/collection/ArrayList.html#method_add) method:

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$fruits = new ArrayList(['apple', 'pear', 'peach']);

$fruits->add('banana');
$fruits->add('apricot', 'watermelon', 'lemon');

$fruits->toArray(); // ['apple', 'pear', 'peach', 'banana', 'apricot', 'watermelon', 'lemon']
```

As you can see in the example above, `add` method appends an element at the end of the collection.
If you want to define the position of the added element you can use the [insert()](api/phootwork/collection/ArrayList.html#method_insert) method; remember that the position starts from 0
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
    If your *ArrayList* models an **associative array** (the indexes are strings) the insertion ** will overwrite** the element
    at the given position. See the example below:


```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$fruits = new ArrayList(['fruit1' => 'apple', 'fruit2' => 'pear', 'fruit3' => 'peach']);

$fruits->insert('banana', 'fruit1');

$fruits->toArray(); // ['fruit1' => 'banana', 'fruit2' => 'pear', 'fruit3' => 'peach']
```

A collection can contain any type of elements, i.e. arrays:

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$fruits = new ArrayList(['apple', 'pear', 'peach']);

$fruits->add('pineapple', ['kiwi', 'lychee'], 'dorian');

$fruits->toArray(); // ['apple', 'pear', 'peach', 'pineapple', ['kiwi', 'lychee'], 'dorian']
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
[search()](api/phootwork/collection/ArrayList.html#method_search) function, this will be the second parameter to your passed function (useful if you don't want to use an 
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

While `search()` only tells you some element is present, if you want to return this element use the
[find()](api/phootwork/collection/ArrayList.html#method_find) rsp. [findAll()](api/phootwork/collection/ArrayList.html#method_findAll)
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

If you are interested in the index of a search, you can use [findIndex()](api/phootwork/collection/ArrayList.html#method_findIndex).
There is also [findLastIndex()](api/phootwork/collection/ArrayList.html#method_findLastIndex) which searches the list
from back to front.

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

You can swap the order of your elements with [reverse()](api/phootwork/collection/ArrayList.html#method_reverse).

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$list = new ArrayList([5, 2, 8, 3, 9, 4, 6, 1, 7, 10]);
$reversed = $list->reverse();
print_r($reversed); // [10, 7, 1, 6, 4, 9, 3, 8, 2, 5]
```

### Simple Sorting

There is built in support for running [sort()](api/phootwork/collection/ArrayList.html#method_sort):

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$unsorted = new ArrayList([5, 2, 8, 3, 9, 4, 6, 1, 7, 10]);
$sorted = $unsorted->sort();
print_r($sorted); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

### Bring your own Comparison

Of course you can use your own comparison to sort a list. In the example below you can see a function in action. The 
returned value must be an integer: 0 if the first and the second values are equal, -1 if the first is minor of the second,
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

Or you can bring in any [Comparator](api/phootwork/lang/Comparator.html). So good, there is a [ComparableComparator](api/phootwork/lang/ComparableComparator.html)
provided by `phootwork/lang` we can use.

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;
use phootwork\lang\ComparableComparator;

$unsorted = new ArrayList(['x', 'c', 'a', 't', 'm']);
$sorted = $unsorted->sort(new ComparableComparator());
print_r($sorted); // ['a', 'c', 'm', 't', 'x']
```

### Remove

You can remove one or more elements from a collection, via the [remove()](api/phootwork/collection/ArrayList.html#method_remove)
method:

```php
<?php declare(strict_types=1);
use phootwork\collection\ArrayList;

$fruits = new ArrayList(['apple', 'banana', 'pine', 'banana', 'ananas']);

$fruits->remove('banana');
$fruits->remove('banana', 'ananas', 'pine');

$fruits->toArray(); // ['apple']
```

### More Methods

You can find more *ArrayList* methods into the [api documentation](api/phootwork/collection/ArrayList.html).

## Set

The [Set](api/phootwork/collection/Set.html) collection is composed by *unique* elements. It shares much methods with `ArrayList` except those which manipulate indexes.
When you use a `Set` collection you really don't care about indexes.

As usual, you can create a Set collection by passing an array or an [Iterator](https://www.php.net/manual/en/class.iterator.php)
object to the constructor:

```php
<?php declare(strict_types=1);
use phootwork\collection\Set;

$fruits = new Set(['apple', 'pear', 'peach']);
```

Since the collection is composed by unique elements, any duplicates are ignored: 

```php
<?php declare(strict_types=1);
use phootwork\collection\Set;

$fruits = new Set(['apple', 'pear', 'peach', 'pear', 'banana', 'apple']);

$fruits->toArray(); // ['apple', 'pear', 'peach', 'banana']
```

### Addition

You can add one or more elements to the *Set* via the [add()](api/phootwork/collection/Set.html#method_add) method:

```php
<?php declare(strict_types=1);
use phootwork\collection\Set;

$fruits = new Set(['apple', 'pear', 'peach']);

$fruits->add('banana');
$fruits->add('pineapple', ['kiwi', 'lychee'], 'dorian');

$fruits->toArray(); // ['apple', 'pear', 'peach', 'pineapple', ['kiwi', 'lychee'], 'dorian']

// Since the Set elements are unique, the instruction below doesn't modify the collection
$fruits->add('apple', 'peach');

$fruits->toArray(); // ['apple', 'pear', 'peach', 'pineapple', ['kiwi', 'lychee'], 'dorian']
```

!!! note
    Set doesn't have the [insert()](#addition_1) method.
    
### More Methods

Many Set methods are shared with other collections: please see the [api documentation](api/phootwork/collection/Set.html)
and the examples above, in [ArrayList](#arraylist) section of this document.

## Map

A [Map](api/phootwork/collection/Map.html) is a collection of key-value pairs, where the value can be any type you want, objects included. You can think about Map
as an associative array with super-powers.

As usual, you can instantiate a new *Map* object by passing an associative array or an
[Iterator](https://www.php.net/manual/en/class.iterator.php) object to the constructor:

```php
<?php declare(strict_types=1);
use phootwork\collection\Map;

$vehicles = new Map(['1 wheel' => 'unicycle', '2 wheels' => 'scooter', '4 wheels' => 'car']);
```

### Additions

You can add one element to a Map via the [set()](api/phootwork/collection/Map.html#method_set) method, which requires
the key as first argument and the value to add as second argument: 

```php
<?php declare(strict_types=1);
use phootwork\collection\Map;

$vehicles = new Map(['1 wheel' => 'unicycle', '2 wheels' => 'scooter', '4 wheels' => 'car']);

$vehicles->set('3 wheels', 'sidecar');
```

You can add more elements via the [setAll()](api/phootwork/collection/Map.html#method_setAll) method. It expects an associative
array or an [Iterator](https://www.php.net/manual/en/class.iterator.php) of elements to add:

```php
<?php declare(strict_types=1);
use phootwork\collection\Map;

$vehicles = new Map();

$coll = [
    '1 wheel' => 'unicycle',
    '2 wheels' => 'scooter',
    '3 wheels' => 'sidecar',
    '4 wheels' => 'car'
];

$vehicles->setAll($coll);
```

### Getting the Elements

You can get an element of the collection via the [get()](api/phootwork/collection/Map.html#method_get) method:

```php
<?php declare(strict_types=1);
use phootwork\collection\Map;

$vehicles = new Map(['1 wheel' => 'unicycle', '2 wheels' => 'scooter', '4 wheels' => 'car']);

$vehicles->get('4 wheels'); // return 'car'

$vehicles->get('6 wheels'); // not found: return null
```

You can also get the key of an element by passing it to the [getKey()](api/phootwork/collection/Map.html#method_getKey) method:

```php
<?php declare(strict_types=1);
use phootwork\collection\Map;

$vehicles = new Map(['1 wheel' => 'unicycle', '2 wheels' => 'scooter', '4 wheels' => 'car']);

$vehicles->getKey('scooter'); // return '2 wheels'
```

### More Methods

You can find all the other Map methods in the [documentation api](api/phootwork/collection/Map.html).

## Stack

A [Stack](api/phootwork/collection/Stack.html) is a FILO (First In Last Out) collection.

As usual you can create a Stack by passing an array, or an [Iterator](https://www.php.net/manual/en/class.iterator.php),
to the constructor:

```php
<?php declare(strict_types=1);
use phootwork\collection\Stack;

$fruits = new Stack(['apple', 'pear', 'peach']);
```

!!! Note
    When you deal with Stack, the order is important: the constructor pushes the elements to end of the passed array,
    so when you pop an element it results in the last of the given array.
    
### Additions

You can add one or more elements to the Stack via the [push()](api/phootwork/collection/Stack.html#method_push) method:

```php
<?php declare(strict_types=1);
use phootwork\collection\Stack;

$fruits = new Stack(['apple', 'pear', 'peach']);

$fruits->push('lemon');
$fruits->toArray(); //['apple', 'pear', 'peach', 'lemon']

$fruits->push('pine', 'banana', 'ananas');
$fruits->toArray(); //['apple', 'pear', 'peach', 'lemon', 'pine', 'banana', 'ananas']
```

### Peek an Element

The method [peek()](api/phootwork/collection/Stack.html#method_peek) returns the last inserted element of the collection
but it doesn't remove it:

```php
<?php declare(strict_types=1);
use phootwork\collection\Stack;

$fruits = new Stack(['apple', 'pear', 'peach', 'pine']);

$fruits->size(); // 4

$fruits->peek(); // return 'pine'

$fruits->size(); // still 4
```

### Pop an Element

Popping an element from a Stack means to retrieve the last inserted element and remove it from the collection:

```php
<?php declare(strict_types=1);
use phootwork\collection\Stack;

$fruits = new Stack(['apple', 'pear', 'peach', 'pine']);

$fruits->size(); // 4

$fruits->pop(); // return 'pine'

$fruits->size(); // 4
$fruits->toArray(); // ['apple', 'pear', 'peach']
```

### More Methods

You can find all the other Stack methods in the [documentation api](api/phootwork/collection/Stack.html).


## Queue

A [Queue](api/phootwork/collection/Queue.html) is a FIFO (First In First Out) collection.

As usual, you can create a Queue by passing an array, or an [Iterator](https://www.php.net/manual/en/class.iterator.php),
to the constructor:

```php
<?php declare(strict_types=1);
use phootwork\collection\Queue;

$fruits = new Queue(['apple', 'pear', 'peach']);
```

!!! Note
    As for the Stack collection, when you deal with Queue, the order is important: the constructor enqueues the elements,
    so when you poll an element it results in the first of the given array.

### Additions

You can add one or more elements to the Queue via the [enqueue()](api/phootwork/collection/Queue.html#method_enqueue) method:

```php
<?php declare(strict_types=1);
use phootwork\collection\Queue;

$fruits = new Queue(['apple', 'pear', 'peach']);

$fruits->enqueue('lemon');
$fruits->toArray(); //['lemon', 'apple', 'pear', 'peach']
```

### Peek an Element

The [peek()](api/phootwork/collection/Queue.html#method_peek) method returns the first inserted element of the collection but
it doesn't remove it:

```php
<?php declare(strict_types=1);
use phootwork\collection\Queue;

$fruits = new Queue(['apple', 'pear', 'peach', 'pine']);

$fruits->size(); // 4

$fruits->peek(); // return 'apple'

$fruits->size(); // still 4
```

### Poll an Element

Polling an element from a Queue means to retrieve the first inserted element from the collection and remove it:

```php
<?php declare(strict_types=1);
use phootwork\collection\Queue;

$fruits = new Queue(['apple', 'pear', 'peach', 'pine']);

$fruits->size(); // 4

$fruits->poll(); // return 'apple'

$fruits->size(); // 3
$fruits->toArray(); // ['pear', 'peach', 'pine']
```

### More Methods

You can find all the other Queue methods in the [documentation api](api/phootwork/collection/Queue.html).