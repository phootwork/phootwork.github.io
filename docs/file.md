# Phootwork file

Photwwork [file](https://github.com/phootwork/file) library for the local filesystem.

- Provide abstractions to the local file system
- Inspired by java `java.io.File`
- Inspired by eclipse `org.eclipse.core.runtime.IPath`

## Installation

Install via Composer:

```bash
composer require phootwork/file
```

## Paths

Paths contain the location to files or directories. Understand various _paths_ with at this given example:

```bash
File: /path/to/my/file.ext
Directory: /path/to/directory
```

Contains the following _paths_ for file and directory:

| Name      | File Path              | Directory Path        |
| --------- | ---------------------- | --------------------- |
| Dirname   | `/path/to/my`          | `/path/to`            |
| Filename  | `file.ext`             | `directory`           |
| Extension | `ext`                  | -                     |
| Pathname  | `/path/to/my/file.ext` | `/path/to/directory`  |

### Building Paths

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

### Absolute or Relative?

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

### Handling segments

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

### Trailing Slash?

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

### Working with other Paths

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

### Stream Support

All the above mentioned paths methods work with streams, here is an example:

```php
$stream = new Path('file:///Home/Documents');
$stream->isAbsolute(); // true
$stream->isStream(); // true
```

## Filesystem

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

Learn more about [handling path](#paths).

### Operations

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

- For files: [create, read and write](#files)
- For directories: [create and iterate directories](#directories)

### Links

A path can point to a link or not independent whether this is a file or directory. Operations will be executed on the link target location. Creating links is not supported (yet ? - submit a PR ;-). Checking whether you are operating on one and where that will be is supported:

```php
$dir = new Directory('path/to/portal');
$dir->isLink(); // true
$dir->getLinkTarget(); // path/to/original/destination
```

### Attributes

A file has plenty attributes, such as the owner or the last accessed time.

#### Permissions

Permissions are relevant when you are in need changing them in order to make files writable. There are convenient checks to test some of those permissions:

- `isReadable()` - to test whether it can be read
- `isWritable()` - to test whether it can be written
- `isExecutable()` - to test whether it can be executed

For more details there is `getOwner()` and `getGroup()` to know whom this file belongs to and `setOwner()`, `setGroup()` and `setMode()` to change all of that.

#### Timestamps

Timestamps for files are retrievable via `getCreatedAt()`, `getLastAccessedAt()`
and `getModifiedAt()`. You can use [`touch()`](#files) to change them.

## Directories

### Create a Directory

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

### Iterate a Directory

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

The `$file` is our own `FileDescriptor`, that offers the same operations mentioned under [Filesystem](#filesystem) plus has checks for `isDot()`, `isFile()` and `isDirectory()`. Additionally brings transforms to cast a `$file` in [`toFile()`](#files) and [`toDirectory()`](#directories).

## Files

### Creating Empty Files

To create empty files, just `touch()` them:

```php
$file->touch();
```

!!! Note
    `touch()` can also be used to changes `created` and `lastAcceessed` attributes.

### Reading and Writing

Retrieving and putting contents of and to files is straightforward using `read()` and `write($content)` methods:

```php
$content = $file->read();
$content .= 'addition';
$file->write($content);
```

## Testing

For testing your implementations, `phootwork/file` works very nice with [mikey179/vfsStream](https://github.com/bovigo/vfsStream).
Use `vfsStream` to test your own package (so does `phootwork/file`).

Head over to [vfs.bovigo.org](http://vfs.bovigo.org/) for the documentation on
how to use `vfsStream`.
