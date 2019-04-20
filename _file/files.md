---
layout: package
title: Files
---

## Creating Empty Files

To create empty files, just `touch()` them:

```php
$file->touch();
```

Note: `touch()` can also be used to changes `created` and `lastAcceessed` attributes.

## Reading and Writing

Retrieving and putting contents of and to files is straightforward using `read()` and `write($content)` methods:

```php
$content = $file->read();
$content .= 'addition';
$file->write($content);
```