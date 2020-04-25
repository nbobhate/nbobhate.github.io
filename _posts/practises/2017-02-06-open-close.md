---
layout: post
title: The Open-Closed Principle
published: false
tags: [Best Practises]
---

The Open-Closed Principle is the second principle making up the [SOLID Principles]({% post_url 2017-02-02-solid-principles %}).

This principle states that software modules, whether methods or classes, should be open for extension, but closed for modification. Basically, changes to existing software should cause minimal (if any) changes to already working and tested code. If developers are constantly updating existing code and thereby tests, to cater for changing requirements then the overall design is considered fragile.

For example, let's say that we have an `Instrument` class. From this we can create subclasses `Violin` and `Flute`.
{% highlight java %}
public class Instrument {
  private String name;
  public Instrument(String name) {
    this.name = name;
  }
}

public class Violin extends Instrument {
  private final static String name = "Violin";
  public Violin() {
    super(name);
  }
}

public class Flute extends Instrument {
  private final static String name = "Flute";
  public Flute() {
    super(name);
  }
}
{% endhighlight %}

Now, we need a conductor to manage our instruments. First we set out the various actions that each instrument must complete. Then the conductor calls on the actions for each instrument in turn.
{% highlight java %}
public class Conductor {

  public void conduct(Instrument instrument) {
    if (instrument instanceof Violin) {
      playSadSolo();
      playQuickStrings();
    } else if (instrument instanceof Flute) {
      playSingleToot();
    }
  }

  // Violin
  private void playSadSolo() {
    System.out.println("Sad solo");
  }

  private void playQuickStrings() {
    System.out.println("Quick Strings!!");
  }

  // Flute
  private void playSingleToot() {
    System.out.println("Single toot");
  }

  // etc
}
{% endhighlight %}

It should be immediately obvious that this design is bad and not maintainable. Firstly, the `Conductor` class violates the [Single Responsibility Principle]({% post_url 2017-02-02-single-responsibility %}) and knows too much about what each instrument should be responsible for. Secondly, the `conduct()` method already violates the Open-Closed Principle: if there are any new instruments the condition will have to be modified to accomodate for them. This is the negative side effect of using `if-else` or `switch` statements in this manner.

Lets untangle the mess by first fixing the Single Responsibility violation by moving the actions each instrument is responsible for into their respective classes. We change the `Instrument` class to an abstract class with an abstract method `play()` that all the subclasses will have to implement.
{% highlight java %}
public abstract class Instrument {
  private String name;
  public Instrument(String name) {
    this.name = name;
  }

  public void play();
}

public class Violin extends Instrument {
  private final static String name = "Violin";

  public Violin() {
    super(name);
  }

  @Override
  public void play() {
    playSadSolo();
    playQuickStrings();
  }

  private void playSadSolo() {
    System.out.println("Sad solo");
  }

  private void playQuickStrings() {
    System.out.println("Quick Strings!!");
  }
}

public class Flute extends Instrument {
  private final static String name = "Flute";

  public Flute() {
    super(name);
  }

  @Override
  public void play() {
    playSingleToot();
  }

  private void playSingleToot() {
    System.out.println("Single toot");
  }
}
{% endhighlight %}

We can clean up `Conductor` as well by removing the `instanceof` check and calling the `play()` method that is overriden in the `Instrument` subclasses. Now when a new instrument is added, the `Conductor` class doesn't have to be modified to accommodate it.
{% highlight java %}
public class Conductor {

  public void conduct(Instrument instrument) {
    instrument.play();
  }
}
{% endhighlight %}

Incidentally, the above solution is also known as a Strategy Pattern.
