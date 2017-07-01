---
layout: package
title: Encode
---

The `Json::encode()` function works almost similarly to php's `json_encode()`, except there is proper [error handling](../error-handling/) built in.

## Synopsis

```php
string Json::encode (mixed $data [, int $options = 0 [, int $depth = 512]])
```

## Simple Encoding

Encoding data into json is done straight forward:

```php
<?php
use phootwork\json\Json;

$data = [...];
$json = Json::encode($data);
```

## Encode with Options

You can also pass options as second parameter:

```php
<?php
use phootwork\json\Json;

$data = [...];
$json = Json::encode($data, Json::PRETTY_PRINT | Json::UNESCAPED_SLASHES);
```
