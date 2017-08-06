---
layout: post
title: Introduction to Java Collections
published: true
tags: [Java]
---

Collections are data structures used to store objects. The basic functions you can perform with any collection structure are *adding* an object, *removing* an object, *finding* and *returning* a specific object, and *iterating* through all the objects in a collection.

Most collection data structures extend from the `java.util.Collection` interface, namely implementations of `Set`, `List`, and `Queue`.

![Java Collection Interface]({{ site.url }}/public/images/posts/collection.png)

The `Map` data structures are still referred to as collections even though they do not extend from `java.util.Collection`.

![Java Map Interface]({{ site.url }}/public/images/posts/map.png)

Collections have four basic properties:
- The *listing* of objects (implementations of `List`)
- A collection of *distinct* objects (implementations of `Set`)
- A collection of *uniquely identifiable* objects (implementations of `Map`)
- Objects that are *arranged by order* (implementations of `Queue`)

These implementations can be further specialised with the following properties:
- **Unordered**: Objects in the collection are not added or maintained in any specific order
- **Unsorted**: Objects in the collection are not sorted by any specific property
- **Ordered**: Iteration through the objects follows a specific order that is not random
- **Sorted**: The order of the objects is determined by rules based on the properties of the objects themselves (eg natural order)

A collection can be *unordered and unsorted*, *ordered and unsorted*, or *ordered and sorted*. A collection can never be *unordered and sorted* because sorting is inherently a type of ordering.

Java also provides a number of static utility methods for use when working with collections in the `java.util.Collections` and `java.util.Arrays` concrete classes.

![Collection Utility Classes]({{ site.url }}/public/images/posts/collection_utils.png)
