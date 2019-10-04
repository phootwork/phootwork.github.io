PHP Json library

## Goals

- Wrap native PHP functions with classes
- Provide solid error handling with exceptions

## Installation

Installation via composer:

```
composer require phootwork/json
```

## Decode

The `Json::decode()` works almost similarly to php's `json_decode()` except it always returns an array instead of an object.

### Synopsis

```php
array Json::decode (string $json [, int $options = 0 [, int $depth = 512]])
```

### Simple Decoding

Decoding is straight forward:

```php
<?php
use phootwork\json\Json;

$json = '...';
$array = Json::decode($json);
```

**Note**: Unlike php, this library returns decoded JSON as array!

### Decode to Collection

The json package works in harmony with [Collections](/collection). You can decode json into any collection easily.

```php
<?php
use phootwork\json\Json;

$json = '...';
$array = Json::toCollection($json);
```

The collection parser will detect what kind of collection type (`Map` or `ArrayList`) the provided json is and will return the matching type of collection. If you know what kind of collection to expect you can call `Json::toMap()` or `Json::toList()` directly.

## Encode

The `Json::encode()` function works almost similarly to php's `json_encode()`, except there is proper [error handling](../error-handling/) built in.

### Synopsis

```php
string Json::encode (mixed $data [, int $options = 0 [, int $depth = 512]])
```

### Simple Encoding

Encoding data into json is done straight forward:

```php
<?php
use phootwork\json\Json;

$data = [...];
$json = Json::encode($data);
```

### Encode with Options

You can also pass options as second parameter:

```php
<?php
use phootwork\json\Json;

$data = [...];
$json = Json::encode($data, Json::PRETTY_PRINT | Json::UNESCAPED_SLASHES);
```

## Error Handling

Error handling with native php functions is a bit ... well... "old". For that reason every call to `Json::*` will throw an exception. Let's see an example for that:

```php
<?php
use phootwork\json\Json;
use phootwork\json\JsonException;

try {
	$json = '...';
	$array = Json::decode($json);
} catch (JsonException $e) {
	echo $e->getMessage();
}
```
