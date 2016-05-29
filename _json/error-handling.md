---
layout: package
title: Error Handling
---

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
