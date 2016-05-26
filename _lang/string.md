---
layout: package
title: String
---

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
