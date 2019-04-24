---
layout: post
title: Builder with Single Class
published: false
tags: [Design Patterns]
---

Ever created a simple enough class that ended up with a large number of overloaded constructors? Something that resembles the class below?

{% highlight java %}
public class Person {

  private String firstName;
  private String[] middleNames;
  private String lastName;
  private float height;
  private float weight;

  public Person(String firstName) {
    this.firstName = firstName;
  }

  public Person(String firstName, String lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  public Person(String firstName, String[] middleNames, String lastName) {
    this.firstName = firstName;
    this.middleNames = middleNames;
    this.lastName = lastName;
  }

  public Person(String firstName, float height) {
    this.firstName = firstName;
    this.height = height;
  }

  // ... more constructors?
  // getters and setters
}
{% endhighlight %}

This becomes really messy and hard to maintain so let's rather use a Builder pattern to help us create, or 'build' this class' instances. First let's create a static inner class to be our builder. The last method in the `Builder` will actually return a new instance of `Person`.
{% highlight java %}
public static class Builder {
  // Our builder class must have the same variables as the class we want to create
  private String firstName;
  private String[] middleNames;
  private String lastName;
  private float height;
  private float weight;

  // This constructor acts as 'minimum requirements' expected by this class
  public Builder(String firstName, String lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  // Each of the remaining objects are set with methods that essentially sets the field on 'this' and then returns the 'this' instance

  public Builder middleNames(String[] middleNames) {
    this.middleNames = middleNames;
    return this;
  }

  public Builder height(float height) {
    // We can do input checking to make sure we don't get nonsense values
    if (height < 0) {
      throw new IllegalArgumentException();
    }
    this.height = height;
    return this;
  }

  public Builder weight(float weight) {
    if (weight < 0) {
      throw new IllegalArgumentException();
    }
    this.weight = weight;
    return this;
  }

  public PersonBuilder build() { // Let's discuss this!
    return new Person(this);
  }
}
{% endhighlight %}

Now let's see what our `Person` constructor will look like.
{% highlight java %}
public class Person {

  private String firstName;
  private String[] middleNames;
  private String lastName;
  private float height;
  private float weight;

  // The constructor is private so that it can't be invoked externally to the class
  // Use the values set in the Builder instance to set the `Person` instance fields
  private Person(Builder builder) {
    this.firstName = builder.firstName;
    this.middleNames = builder.middleNames;
    this.lastName = builder.lastName;
    this.height = builder.height;
    this.weight = builder.weight;
  }

  // getters and setters
  // static inner Builder class
}
{% endhighlight %}

Now when we want to create a new `Person` instance we can simply build an instance by setting the fields that are known.
{% highlight java %}
Person personWithHeight = new Person.Builder("Some", "One").height(5.8F).build();
Person personWithHeightWeight = new Person.Builder("Some", "One").height(5.8F).weight(68.2F).build();
{% endhighlight %}
