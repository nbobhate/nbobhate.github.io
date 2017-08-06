---
layout: post
title: Being an Object
published: true
tags: [Java]
---

Every object whether an exception, array or something you've created yourself extends from `java.lang.Object`. The following methods are therefore common to all objects you use or create.

`String toString()`

This method allows you to view the string representation of a class. For example, if you pass an object reference into the `System.out.println()` method, it invokes that object's `toString()` method and prints a string that will look something like "ObjectName@a0f48bc". This isn't terribly helpful so you would usually override the `toString()` method in your class to return a value that is meaningful to know about that object. The most common use of overriding this method is to display the current state of, or values held by, the instance variables.


`boolean equals(Object o)`

Objects can be compared in two ways: by using the `==` operation or by invoking the `equals()` method. The `==` operation compares the value of each object reference, and returns true if they are equal (so they're pointing to the same object).

Some common classes in Java already override the `equals()` method. The `String` class will compare each character in two given strings to determine if they have the same value, and the `Integer` class will return true if the two `Integer` objects both have an `int` value that is the same. Note that these objects can be completely different with differnt object refernces, but if their values are the same, the `equals()` method will return true.

The consequence of not overriding the `equals()` method is that you won't be able to use the object as a key in a hashtable, so you won't get accurate sets or maps. If you ever need to consider two differnt objects as the same, then make sure to override the `equals()` method.

You can override the `equals()` method with the folllowing signature. Note that the method doesn't accept the object type of the implementing class. The correct method defined in `Object` takes an `Object` reference. If you create an `equals(CustomObject o)`, it would be considered overloading and would not be called.

{% highlight java %}
public boolean equals(Object o) {
  if (o instanceof CustomObject) {
    // ...
  }
  return false;
}
{% endhighlight %}

The next line makes sure that the object passed in is the same type as the object it is being compared to. If you don't check this, you risk the chance of the method failing with a `ClassCastException`. If the objects are the same type, you can test whether their internal values match. For best performance you should check the fewest number of attributes.

Certain rules need to be adhered to when overriding the `equals()` method. This is often referred to as the equals() contract.
- `x.equals(null)` should return false
- `x.equals(x)` should return true (reflexive property)
- If `x.equals(y)` is true, then `y.equals(x)` should return true (symmetric property)
- If `x.equals(y)` is true and `y.equals(z)` is true, then `x.equals(z)` should return true (transitive property)
- Multiple invocations of `x.equals(y)` should return the same value
- If two objects are considered equal with the `equals()` method, then they must have identical hashcode values


`int hashCode()`

HashCodes are used to increase the performance of large collections to determine how an object should be stored and later located. They are used as object identifiers, but it is important to note that hashcodes are not necessarily unique.

Here is a small analogy to help understand hashing. Let's say you have a number of buckets. Along comes an object that you need to place in a given bucket. In order to determine which bucket this object must be placed in, we use an algorithm that will always run the same way given a specific input to generate a hashcode as a result. This hashcode is then the bucket reference where the object is placed. More than one object can be placed in a single bucket, and these objects can have different values from each other.

Now you are given another object to use as a reference to retrieve an earlier object you placed in one of the buckets. How do we find this object? We can apply the algorithm to this new object to find the bucket we're looking for. Then we need to go through all the objects in this bucket, compare them to the new object using the `equals()` method, and return the object that matches.

This is why the `equals()` and `hashCode()` methods go hand-in-hand.

THe same attributes you chose to compare in your `equals()` method should be used when you override the `hashCode()` method. You don't have to - in fact, you can return the same value regardless of the object. This means there will only be one bucket containing all the objects.

{% highlight java %}
public int hashCode() {
  //
}
{% endhighlight %}

There is also a hashCode() contract:
- Must consistently return the same integer provided that no value used in the `equals()` method has changed
- If two objects are equal, their hashCode values must also be equal
- Two unequal objects can have the same hasCode value

When serializing objects, some values can be lost. These `transient` variables will not deserialize correctly and will cause unexpected results when included in the `equals()` and `hashCode()` methods. Avoid using `transient` variables in determining `equals()` and `hashCode()`.


`void finalize()`


`final void notify()`


`final void notifyAll()`


`final void wait()`
