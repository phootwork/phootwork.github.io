---
layout: package
title: Decode
---

The `Json::decode()` works almost similarly to php's `json_decode()` except it always returns an array instead of an object.

## Synopsis

```php
array Json::decode (string $json [, int $options = 0 [, int $depth = 512]])
```

## Simple Decoding

Decoding is straight forward:

```php
<?php
use phootwork\json\Json;

$json = '...';
$array = Json::decode($json);
```

**Note**: Unlike php, this library returns decoded JSON as array!

## Decode to Collection

The json package works in harmony with [Collections](/collection). You can decode json into any collection easily.

```php
<?php
use phootwork\json\Json;

$json = '...';
$array = Json::toCollection($json);
```

The collection parser will detect what kind of collection type (`Map` or `ArrayList`) the provided json is and will return the matching type of collection. If you know what kind of collection to expect you can call `Json::toMap()` or `Json::toList()` directly.
