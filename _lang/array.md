---
layout: package
title: Array
---

Arrays are key data structures for both map and list like types in php.

## Arrayable

Getting an array from an object is not easy to know. That's why there is an
`Arrayable` interface which has only one method: `toArray()` and helps to be put on
objects that will return an array. Use it for type-hinting or check the instance of
an object whether is arrayable.

## ArrayObject

An `ArrayObject` is a wrapper around a native php array but provides consistent
methods instead of weird functions and parameter order that are hard to read in
your code.

Alternatively you can use the <a href="/collection">Collection</a> package.
