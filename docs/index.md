# Welcome to Phootwork!

> Many PHP functions are simply wrappers around the native C counterparts, and they do their great work since the earliest php versions.
The time passed, the versions increased and PHP 5 and PHP 7 arrived, with their brand new object oriented heart.
Anyway, those functions are still there and new PHP versions don't offer alternatives.

[Phootwork :fab fa-github:](https://github.com/phootwork/phootwork) is a collection of php libraries which fill these gaps in the php language and provides consistent object oriented solutions
where the language natively offers only functions.

The phootwork package includes:

- [collection](https://github.com/phootwork/collection) a library to model several flavours of collections
- [file](https://github.com/phootwork/file) an object oriented library to manipulate filesystems elements (*stream compatible*)
- [json](https://github.com/phootwork/json) a json library, with clean syntax and proper error handling
- [lang](https://github.com/phootwork/lang) a library to manipulate arrays and strings in an object oriented way
- [tokenizer](https://github.com/phootwork/tokenizer) an easy to use tokenizer library for PHP code
- [xml](https://github.com/phootwork/xml) an object oriented xml utility library

## Installation

We use [composer](https://getcomposer.org) as dependency manager and distribution system. To install the library run:

```bash
composer require phootwork/phootwork
```

Each single package can be installed separately. I.e. if you want to include in your project the `collection` library only:

```bash
composer require phootwork/collection
```

!!! note
    The single library packages does not ship with tests and --dev dependencies. If you want to run the test suite or 
    contribute to the library, you have to install the whole `phootwork/phootwork` package.

## A Little Taste

The following examples show what you can find in this library. You can discover much, much more by reading the documentation
and the api.

### A Little Taste of *lang* Library (`phootwork\lang\Text` class);

```php
<?php declare(strict_types=1);
/**
 * Example describing how to manipulate a string via the Text class
 * and its nice fluent api.
 */
use phootwork\lang\Text;

$text = new Text('a beautiful string');

// Remove the substring 'a ' and capitalize. Note: Text objects are *immutable*, 
// so you should assign the result to a variable
$text = $text->slice(2)->toCapitalCase(); // 'Beautiful string'

// Capitalize each word and add an 's' character at the end of the string
$text = $text->toCapitalCaseWords()->append('s'); // 'Beautiful Strings'

// Calculate the length of the string
$length = $text->length(); // 17

// Check if the string ends with the 'ngs' substring
$text->endsWith('ngs'); // true
```
### A Little Taste of *collection* Library (`phootwork\collection\Stack` class)

```php
<?php declare(strict_types=1);
/**
 * Example describing how to manipulate a Stack collection (Last In First Out)
 * via the Stack class
 */
use phootwork\collection\Stack;

$stack = new Stack(['Obiwan', 'Luke', 'Yoda', 'Leila']);

// Sort the stack
$stack = $stack->sort(); // ['Leila', 'Luke', 'Obiwan', 'Yoda']

// Check if the collection contains any elements
$stack->isEmpty();  // false

// How many elements?
$stack->size(); // 4

// Push an elememt
$stack->push('Chewbecca');

// How many elements now?
$stack->size(); // 5

// Peek the head element (return the head element, without removing it)
$stack->peek(); // 'Chewbecca'
$stack->size(); // 5

// Pop the head element
$stack->pop(); // 'Chewbecca'
$stack->size(); // 4: pop() removes the popped element
```
<hr>
<a class="btn btn-info btn-lg" href="https://github.com/phootwork/phootwork/issues"><i class="fab fa-github"></i><b> Any issue?</b></a>
<a class="btn btn-info btn-lg" href="https://github.com/phootwork/phootwork.github.io/issues"><i class="fas fa-keyboard"></i><b> Found a typo?</b></a>
