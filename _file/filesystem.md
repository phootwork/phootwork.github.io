---
layout: package
title: Filesystem
---

The `phootwork/file` package comes with two classes to access the local filesystem that share a unified and convenient API:

1. `File`
2. `Directory`

For both scenarios, they are instantiated with the pathname or path as argument for the constructor:

```php
$directoryPathname = '/path/to/directory'
$directory = new Directory($directoryPathname);

$filePathname = '/path/to/file.ext';
$file = new File($filePathname);

// -- or --

$directoryPath = new Path('/path/to/directory');
$directory = new Directory($directoryPath);

$filePath = new Path('/path/to/file.ext');
$file = new File($filePath);
```

Learn more about [handling path](/file/paths).

## Operations

Main operations _move_, _copy_ and _delete_ are supported. Also handy, does a file exist ?

```php
$file = new File('hello/world.txt');
$file->exists(); // true
$file->move('been/there/done/that');
$file->copy('exactly');
$file->delete();
```

Important: `delete()` on directories works recursively!

The operations onto each of those instances may vary:

- For files: [create, read and write](/file/files)
- For directories: [create and iterate directories](/file/directories)

## Links

A path can point to a link or not independent whether this is a file or directory. Operations will be executed on the link target location. Creating links is not supported (yet ? - submit a PR ;-). Checking whether you are operating on one and where that will be is supported:

```php
$dir = new Directory('path/to/portal');
$dir->isLink(); // true
$dir->getLinkTarget(); // path/to/original/destination
```

## Attributes

A file has plenty attributes, such as the owner or the last accessed time. 

### Permissions

Permissions are relevant when you are in need changing them in order to make files writable. There are convenient checks to test some of those permissions:

- `isReadable()` - to test whether it can be read
- `isWritable()` - to test whether it can be written
- `isExecutable()` - to test whether it can be executed

For more details there is `getOwner()` and `getGroup()` to know whom this file belongs to and `setOwner()`, `setGroup()` and `setMode()` to change all of that.   

### Timestamps

Timestamps for files are retrievable via `getCreatedAt()`, `getLastAccessedAt()` and `getModifiedAt()`. You can use [`touch()`](/file/files) to change them.
 

