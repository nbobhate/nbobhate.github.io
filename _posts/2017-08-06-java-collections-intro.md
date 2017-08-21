---
layout: post
title: "Introduction to Java Collections"
published: false
tags: [Java]
---

Collections are data structures used to store objects. The basic functions you can perform with any collection structure are ***adding*** an object, ***removing*** an object, ***finding*** and ***returning*** a specific object, and ***iterating*** through all the objects in a collection.

>   Prerequisite reading; [Noteworthy Methods in Object]({{ site.url }}{% post_url 2017-08-05-java-object-methods %}), [Magic of Autoboxing]({{ '/baking' | prepend: site.url }})

Collections have four basic properties:
- The ***listing*** of objects (implementations of `List`)
- A collection of ***distinct*** objects (implementations of `Set`)
- Objects that are ***arranged by order*** (implementations of `Queue`)
- A collection of ***uniquely identifiable*** objects (implementations of `Map`)

These implementations can be further specialised with the following properties:
- **Unordered**: Objects in the collection are not added or maintained in any specific order
- **Unsorted**: Objects in the collection are not sorted by any specific property
- **Ordered**: Iteration through the objects follows a specific order that is not random
- **Sorted**: The order of the objects is determined by rules based on the properties of the objects themselves (natural order, using a `Comparator`)

A collection can be ***unordered and unsorted***, ***ordered and unsorted***, or ***ordered and sorted***. A collection can never be ***unordered and sorted*** because sorting is inherently a type of ordering.

# Implementations of `Collection`

In Java most collection data structures extend from the `java.util.Collection` interface. The most common interfaces and structures are discussed below.
![Java Collection Interface]({{ site.url }}/public/images/posts/collection.png){: .post-image }

## List
Lists are typically **ordered and unsorted**, and centre around the idea of an *index*. A list structure maintains the order in which elements are inserted, and this is the expected order when iterating through the structure. Elements can then be retrieved from a list by specifying the index position that they were inserted at.

### ArrayLists

### Vectors

### LinkedList

## Set
Sets maintain uniqueness and rely on the `equals()` method to ensure that duplicate elements cannot be inserted.

### HashSet

### LinkedHashSet

### TreeSet

## Queue


# Wait, where's `Map`?
The `Map` data structures are still referred to as collections even though they do not extend from `java.util.Collection`.
![Java Map Interface]({{ site.url }}/public/images/posts/map.png){: .post-image }



# Utility Classes
Java also provides a number of static utility methods for use when working with collections in the `java.util.Collections` and `java.util.Arrays` concrete classes. Some useful methods from these are described below.
![Collection Utility Classes]({{ site.url }}/public/images/posts/collection_utils.png){: .post-image }

## Collections

## Arrays
