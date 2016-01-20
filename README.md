# ExternalEnumeration

![Maintenance Status](https://img.shields.io/badge/maintenance-active-green.svg)

Adds an external Enumerator that provides the full richness of the
Smalltalk enumeration protocol to an internal collection or complex
data structure without exposing the entire collection.

ExternalEnumeration is licensed under the MIT license.  See the
copyright tab in the RB, the `notice` property of this package, or the
`LICENSE` file on GitHub.

ExternalEnumeration's primary home is the [Cincom Public Store Repository](http://www.cincomsmalltalk.com/CincomSmalltalkWiki/Public+Store+Repository).
Check there for the latest version.  It is also on
[GitHub](https://github.com/randycoulman/ExternalEnumeration).

ExternalEnumeration was developed in VW 7.9.1, but is compatible with
any version of VisualworksSmalltalk. If you find any incompatibilities,
let me know (see below for contact information) or file an issue on
GitHub.

# Introduction

Smalltalk collections have a rich protocol for enumerating their
elements: simple iteration with `do:`, mapping with `collect:`,
filtering with `select:` and `reject:`, reducing with `fold:` and
`inject:into:`, and more.

Many systems contain objects that are logically collections, but
rightfully do not inherit from classes in the collection hierarchy.
Perhaps they contain a collection internally, but have a more
restricted API for interacting with the collection.

Some systems contain complex data structures that need to be
enumerated.  It is often possible to expose some kind of a `do:`
method for enumerating the elements of the structure.
`Behaviour>>subclassesDo:` and `VisualComponent>>childrenDo:` are
examples.

If we want to provide the full enumeration API in these cases, our
choices are limited:

* Expose an internal collection via a method.  This allows client code
  access to the full enumeration API, but also allows access to the
  full collection API.  This can be dangerous, as the client code can
  then modify the collection as well.  For complex data structures,
  there isn't even an internal collection to expose.

* Use [traits](http://scg.unibe.ch/research/traits), assuming there is
  a functional implementation for your Smalltalk.

* Implement the full enumeration API on the class, delegating the
  methods to the internal collection.  This is a lot of tedious work
  that must be repeated for each collection-like object in the system.

ExternalEnumeration provides a new option to solve these problems.  It
provides `Enumerator`, an object that takes a target object and a
`do:` selector and provides the full, rich enumeration API that
Smalltalk programmers expect.

`Enumerator` is an implementation of the
[external iterator](http://c2.com/cgi/wiki?ExternalIterator) pattern.

# Usage

To create an `Enumerator`, send the `#on:selector:` message to the
`Enumerator` class.  The resulting enumerator responds to all of the
normal collection enumeration messages.  The selector must be the name
of a method that provides `do:` semantics for the target object.

```
subclasses := Enumerator on: Collection selector: #allSubclassesDo:.
(subclasses groupedBy: #superclass) inspect.
```

Some enumeration messages return a collection (`collect:`, `select:`,
`reject:`, and `groupedBy:`).  By default, the resulting collection
will be an `OrderedCollection` (or a `Dictionary` whose values are
`OrderedCollections` in the case of `groupedBy:`).  If you'd like to
use a different collection type (like `Set` or `Array`), you can use
the `#on:selector:species:` method instead:

```
MyCollectionObject>>elements
    ^Enumerator on: internalElements selector: #do: species: Set
```

`Enumerator` currently implements the following methods:

* `allSatisfy:`
* `anySatisfy:`
* `collect:`
* `detect:`
* `detect:ifFound:`
* `detect:ifFound:ifNone:`
* `detect:ifNone:`
* `do:`
* `do:separatedBy:`
* `fold:`
* `groupedBy:`
* `inject:into:`
* `reject:`
* `select:`

# Contributing

I'm happy to receive bug fixes and improvements to this package.  If
you'd like to contribute, please publish your changes as a "branch"
(non-integer) version in the Public Store Repository and contact me as
outlined below to let me know.  I will consider merging your changes
back into the "trunk" as soon as I can review them.

# Contact Information

If you have any questions about ExternalEnumeration and how to use it, feel
free to contact me.

* Web site: http://randycoulman.com
* Blog: Courageous Software (http://randycoulman.com/blog)
* E-mail: randy _at_ randycoulman _dot_ com
* Twitter: @randycoulman
* GitHub: randycoulman
