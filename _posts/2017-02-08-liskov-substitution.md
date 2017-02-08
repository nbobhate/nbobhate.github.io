---
layout: post
title: The Liskov Substitution Principle
published: true
tags: [Best Practises]
---

The Liskov Substitution Principle is one of the [SOLID Principles]({% post_url 2017-02-02-solid-principles %}) and states that subclasses must be substitutable for their superclass. All subclasses of a superclass must share an *IS-A* relationship with the superclass. If a subclass doesn't share all the inherited state from the superclass, then the subclass is violating the Liskov Substitution principle.

As an example, we have a superclass representing a `Teacher`.
{% highlight java %}
public abstract class Teacher {
  public abstract void teach();

  public void otherDuties() {
    System.out.println("Performing other duties.");
  }
}
{% endhighlight %}

The following classes, `ScienceTeacher` and `MathTeacher`, both inherit all the behaviour present in the superclass `Teacher`.
{% highlight java %}
public class ScienceTeacher extends Teacher {
  @Overrides
  public void teach() {
    System.out.println("Teaching science!");
  }
}
{% endhighlight %}

{% highlight java %}
public class MathTeacher extends Teacher {
  @Overrides
  public void teach() {
    System.out.println("Teaching math!");
  }
}
{% endhighlight %}

So far so good. Lets say that there exists a post called `TeachingAssistant`. How would we model this? An instance of `TeachingAssistant` certainly performs other duties, but they don't teach. Allowing `TeachingAssistant` to extend `Teacher` just to inherit some behaviour violates the Liskov Substitution principle in the following ways.

Firstly, allowing `TeachingAssistant` to be a derivative of `Teacher` has created a situation where the client will have to have knowledge of `TeachingAssistant` in order to avoid calling the `teach()` method. Secondly, we can accommodate for this by having the `teach()` method in `TeachingAssistant` return nothing or throw an exception, but this forces the superclass, or contract, to change. This is indicative of bad design and should be avoided.

In this instance, we can solve this by removing the behaviour of *teaching* into an interface and introduce a new class `SchoolStaff` as our superclass.
{% highlight java %}
public interface Teacher {
  public void teach();
}
{% endhighlight %}

{% highlight java %}
public class SchoolStaff {
  public void otherDuties() {
    System.out.println("Performing other duties.");
  }
}
{% endhighlight %}

Our teaching staff now change to implement the interface `Teacher`, and extend from `SchoolStaff`.
{% highlight java %}
public class ScienceTeacher extends SchoolStaff implements Teacher {
  @Overrides
  public void teach() {
    System.out.println("Teaching science!");
  }
}
{% endhighlight %}

{% highlight java %}
public class MathTeacher extends SchoolStaff implements Teacher {
  @Overrides
  public void teach() {
    System.out.println("Teaching math!");
  }
}
{% endhighlight %}

The `TeachingAssistant` now extends `SchoolStaff`.
{% highlight java %}
public class TeachingAssistant extends SchoolStaff {

}
{% endhighlight %}

This may seem like a lengthy process but this design adheres to the Liskov Substitution principle. Both the `ScienceTeacher` and `MathTeacher` share an *IS-A* relationship with `SchoolStaff` and `Teacher`. The `TeachingAssistant` *IS-A* `SchoolStaff` without clients expecting it to also be a `Teacher`.

When deciding how to structure subclasses, simply ask if all the behaviour of the superclass should be applied to the subclass. If an aspect of the superclass behaviour doesn't make sense for the subclass, you are more than likely violating the Liskov Substitution principle by forcing the association.
