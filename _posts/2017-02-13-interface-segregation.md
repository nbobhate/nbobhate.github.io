---
layout: post
title: Interface Segregation Principle
published: true
tags: [Best Practises]
---

The Interface Segregation Principle is from the set of [SOLID Principles]({% post_url 2017-02-02-solid-principles %}). This principle simply states that clients should not be forced to depend on methods they don't use. This is often the case when inheriting from *fat* classes (classes that have too many responsibilities).

Lets look at the simple example below. We have an interface called `Travel` that allows the implementing client to travel via different modes of transport.
{% highlight java %}
public interface Travel {
  public void travelbyBus();
  public void travelByTrain();
}
{% endhighlight %}

Now lets introduce a class that will implement `Travel`. For argument's sake we have a class `Freight` that must be transported somehow. It makes sense for `Freight` to travel by train, but not by bus. However, since `Freight` is implementing `Travel`, it is forced to have an implementation of the `travelbyBus()` method. This clearly violates the Interface Segregation principle.
{% highlight java %}
public class Freight implements Travel {
  @Override
  public void travelbyBus() {
    //...
  }

  @Override
  public void travelByTrain() {
    //...
  }
}
{% endhighlight %}

The `Travel` interface should rather be split into two separate interfaces as follows.
{% highlight java %}
public interface BusTravel {
  public void travelbyBus();
}

public interface TrainTravel {
  public void travelByTrain();
}
{% endhighlight %}

This allows `Freight` to implement the interface that it requires without having to worry about handling responsibilities it doesn't need.
{% highlight java %}
public class Freight implements TrainTravel {
  @Override
  public void travelByTrain() {
    //...
  }
}
{% endhighlight %}
