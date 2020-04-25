---
layout: post
title: Dependency Inversion
published: false
tags: [Best Practises]
---

Dependency Inversion is the 'D' in the [SOLID Principles]({% post_url 2017-02-02-solid-principles %}).

This principle states that in software design, high level modules should not depend on low level modules. Low level modules are generally prone to change as the software evolves, and these changes will ripple out and affect all the higher level modules depending on them.

Rather, both should depend on *abstractions*. Abstractions are not tied down by the details of the implementation, but rather should drive the implementation process by providing *contracts* that subclasses must adhere to. These contracts are less prone to change once they are defined. Abstractions in Java can be realised by the use of `abstract` or `interface` classes.

Lets look at some code. For this example, we have a manufacturing factory that produces various devices. All these devices go through the same steps:

- Assembly
- Testing
- Packaging
- Storage

However, for each device the actual details of each step varies. The manufacturer doesn't need to know what these details are; they just need to be able to ensure that each device goes through each step. Lets start with creating our abstract contract.
{% highlight java %}
package processes;
public abstract class ManufacturingProcess {
  protected void assemble();
  protected void test();
  protected void packaging();
  protected void store();

  ...
}
{% endhighlight %}

Now lets create some devices that we can manufacture. Each subclass of `ManufacturingProcess` is forced to implement the abstract methods defined in the superclass.
{% highlight java %}
package processes;
public class LaptopManufacturer extends ManufacturingProcess {

  @Override
  protected void assemble() {
    System.out.println("Assembling a Laptop");
  }

  @Override
  protected void test() {
    System.out.println("Testing the Laptop");
  }

  @Override
  protected void packaging() {
    System.out.println("Packaging the Laptop");
  }

  @Override
  protected void store() {
    System.out.println("Storing the Laptop");
  }
}
{% endhighlight %}

{% highlight java %}
package processes;
public class TabletManufacturer extends ManufacturingProcess {

  @Override
  protected void assemble() {
    System.out.println("Assembling a Tablet");
  }

  @Override
  protected void test() {
    System.out.println("Testing the Tablet");
  }

  @Override
  protected void packaging() {
    System.out.println("Packaging the Tablet");
  }

  @Override
  protected void store() {
    System.out.println("Storing the Tablet");
  }
}
{% endhighlight %}

In order to ensure that the steps are invoked in the correct order, we can define a method in the superclass that makes the method calls correctly. The `launch()` method doesn't need to worry about the details of each step for each device. It simply stipulates the method order that will be called on the instance of the subclass.
{% highlight java %}
package processes;
public abstract class ManufacturingProcess {
  protected void assemble();
  protected void test();
  protected void packaging();
  protected void store();

  public void launch() {
    assemble();
    test();
    packaging();
    store();
  }
}
{% endhighlight %}

Incidentally, the `launch()` method is an example of applying the Template pattern.

This allows the manufacturer to disregard the concrete types during the manufacturing process, and rather depends on the contract provided by the abstraction. Note that in this case, we have defined all of our process-related classes in a different package, and the methods defined in `ManufacturingProcess` are defined as `protected`. This measn that the instance of `ManufacturingProcess` defined in `Factory` only has access to the `launch()` method. Now the manufacturer doesn't need to know the details about each device's manufacturing process. They can simply invoke the process!
{% highlight java %}
package clients;
public class Factory {
  public static void main(String[] args) {
    ManufacturingProcess laptopManufacturer = new LaptopManufacturer();
    laptopManufacturer.launch();

    ManufacturingProcess tabletManufacturer = new TabletManufacturer();
    tabletManufacturer.launch();
  }
}
{% endhighlight %}
