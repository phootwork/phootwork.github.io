---
layout: package
title: Directories
---

## Create a Directory

Try creating a directory and catch upon afterwards if that plan failed:

```php
try {
	$dir = new Directory('/hello/there');
	$dir->make();
} catch (FileException $e) {
	// oupsi
}
```

and delete the directory later on:

```php
try {
	$dir = new Directory('/hello/there');
	$dir->delete();
} catch (FileException $e) {
	// nope, not gonna happen
}
```

Important notice: `delete()` on a directory works recursively (unlink phps `rmdir()` function).

## Iterate a Directory

For most of the time, you want to iterate over the files within a directory - recursively or not is up to you. Let's have a look at an example:

```php
$files = new ArrayList();
$folders = new ArrayList();
$dir = new Directory('path/to/folder');
foreach ($dir as $file) {
	if ($file->isDot()) {
		continue;
	}
	
	if ($file->isFile()) {
		$files->add($file->toFile());
	}
	
	if ($file->isFolder()) {
		$folders->add($file->toDirectory());
	}
}
```

The `$file` is our own `FileDescriptor`, that offers the same operations mentioned under [Filesystem](/file/filesystem) plus has checks for `isDot()`, `isFile()` and `isDirectory()`. Additionally brings transforms to cast a `$file` in [`toFile()`](/file/files) and [`toDirectory()`](/file/directories).
