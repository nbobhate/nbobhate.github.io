---
layout: post
title: Caching Hash Codes
published: true
tags: [Java]
---

In some cases you need to employ as many optimization strategies as possible to make your application perform just that much faster. I came across this small bit of optimization information and it rather blew my mind. As with most things, there is unfortunately an important caveat.<!--more-->

Who would think second about calculating hash codes? Not your beginner to average programmer. You need that all important hash code method if you want to use any data structures that employ hashing. And these days just one click in an IDE gives you both the `equals()` and `hashcode()` for free. However, computing an object's hash code can be somewhat expensive, especially considering that the computation occurs very time the method is called.

Now, if you happen to have an immutable object, such as the `String` class, you can cache the hash code and avoid doing a recalculation. And here we see the caveat. There are not many times that you would create an immutable object. You'd have to be pretty certain that your class will never need to be inherited. But is always a nice thing to bear in mind if you ever do create that one immutable class: cache the hash code and save some processing.

{% highlight java %}
public final class String {
  private int hash = 0;

  public int hashCode() {
    if (hash != 0) return hash;

    for (int i = 0; i < length(); i++) {
      hash = hash * 31 + (int) charAt(i);
      return hash;
    }
  }
}
{% endhighlight %}
