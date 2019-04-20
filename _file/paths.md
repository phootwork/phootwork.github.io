---
layout: package
title: Paths
---

Paths contain the location to files or directories. Understand various _paths_ with at this given example:

```
File: /path/to/my/file.ext
Directory: /path/to/directory
```

Contains the following _paths_  for file and directory:

| Name | File Path | Directory Path |
| ==== | ========= | ============== |
| Dirname | `/path/to/my` | `/path/to` |
| Filename | `file.ext` | `directory` |
| Extension | `ext` | - |
| Pathname | `/path/to/my/file.ext` | `/path/to/directory` |

## Building Paths

The `Path` class helps out with all utilities to handle paths. Let's start with a base path from which we append to a locales directory and config file where we change the extension later on:

```php
$base = new Path('/path/to');
$locales = $base->append('locales'); // /path/to/locales
$config = $base->append('config.yml'); // /path/to/config.yml

$config->getPathname(); // /path/to/config.yml
$config->getFilename();  // config.yml
$config->getDirname(); // /path/to
$config->getExtension(); // yml

$config->setExtension('json'); // /path/to/config.json
```

## What Path?

Easy checks whether a path is empty or absolute:

```php
$dunnoWhatPathIHaveHere = '...'; // we get this as a return from somewhere else

if ($dunnoWhatPathIHaveHere->isEmpty()) {
	// nothing to see here, go away!
}

if ($dunnoWhatPathIHaveHere->iAbsolute()) {
	// also works on windows!
}
```

## Handling segments

Segments are all the parts between the path delimiter `/` (even windows path are stored as unix path internally). Some convenient methods for them:

```php
$p = new Path('this/is/the/path/to/my/file.ext');
		
$p->segments()->toArray(); // ['this', 'is', 'the', 'path', 'to', 'my', 'file.ext']
$p->segmentCount(); // 7
$p->segment(1); // is
$p->lastSegment(); // file.ext
$p->upToSegment(3); // this/is/the
$p->removeFirstSegments(2); // the/path/to/my/file.ext
$p->removeLastSegments(2); // this/is/the/path/to
$p->upToSegment(0); // (empty string)
```


## Trailing Slash?

Very often you need to have a trailing slash (or not). This is ugly to do with given string manipulation functions, this is covered for sure:

```php
$p = new Path('/path/to/folder');

$p->hasTrailingSeparator(); // false
$p->addTrailingSeparator(); // /path/to/folder/
$p->addTrailingSeparator(); // hah! doesn't add twice here!
$p->hasTrailingSeparator(); // true
$p->removeTrailingSeparator(); // /path/to/folder
$p->hasTrailingSeparator(); // false
```

Whenever you need the path with a trailing slash, call `$p->addTrailingSeparator()` which works as your "guarding operator" and you can be sure, there will be one - and only one - trailing separator add the end of your path.

## Working with other Paths

A quite frequent operation for paths is to work with other paths. For example is one path the parent of another how is one path relative to another one? Instead of having replace and string logic, focus on writing clean and semantic code.

```php
$file = new Path('this/is/the/path/to/my/file.ext');
$parent = new Path('this/is/the');

if ($parent->isPrefixOf($file)) {
	// yup, we are
}

$file->makeRelativeTo($parent); // path/to/my/file.ext
```

It's sometimes interessting to know how much equal two paths are, here comes `matchingFirstSegments()` and `equals()`:

```php
$base = new Path('this/is/the/path/to/my/file.ext');
$prefix = new Path('this/is/the');
$anotherPath = new Path('this/is/another/path');

$base->matchingFirstSegments($prefix); // 3
$base->matchingFirstSegments($anotherPath); // 2
$prefix->equals($base); // false
$prefix->equals('this/is/the'); // true
```

## Stream Support

All the above mentioned paths methods work with streams, here is an example:

```php
$stream = new Path('file:///Home/Documents');
$stream->isAbsolute(); // true
$stream->isStream(); // true
```




 