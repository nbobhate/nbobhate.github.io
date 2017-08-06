---
layout: post
title: Single Responsibility Principle
published: true
tags: [Best Practises]
---

The Single Responsibility Principle is pretty self explanatory, but can be the one best practice that is easily ignored. The results are God Classes which are a nightmare to maintain for new or even existing developers.

In essence this principle states that a class should only be able to perform actions that it is directly responsible for. Sounds pretty obvoious but this distinction does get a bit blurry. The key is to always bear in mind the purpose of your class.

For example, let's say we have a `Book` class. We have some operations that we want to perform with `Book`.
{% highlight java %}
public class Book {

  public String getTitle() {
    return "Some title";
  }

  public String getAuthor() {
    return "Some author";
  }

  public Page getCurrentPage() {
    // Return the current page
  }

  public void printPage() {
    // Print the current page
  }

  public void save() {
    // Save book to file
  }
}
{% endhighlight %}

The `Book` class is currently responsible for returning metadata about itself (title, author), printing a page, and saving itself to a file. The last two operations violate the Single Responsibility Principle because `Book` shouldn't need to worry about how it should be printed and to what format, nor how it is saved. These operations can be removed and the responsibility can be granted to other classes.

We can remove the printing operations and create a class specifically responsible for printing pages. This makes it easier to mamange things like printing to different types of output, printing multiple pages, etc.
{% highlight java %}
public interface Printer {
  public void printPage(Page page);
}

public class PlainTextPrinter implements Printer {
  public void printPage(Page page) {
    // print the page as plain text
  }
}

public class PDFPrinter implements Printer {
  public void printPage(Page page) {
    // print the page as a PDF
  }
}
{% endhighlight %}

So what does `Book` look like now? We can remove the `printPage()` method as we have another class that is responsible for this operation.
{% highlight java %}
public class Book {

  public String getTitle() {
    return "Some title";
  }

  public String getAuthor() {
    return "Some author";
  }

  public Page getCurrentPage() {
    // Return the current page
  }

  public void save() {
    // Save book to DB
  }
}
{% endhighlight %}

When we want to then print a page from a `Book` instance, we can do so as follows,
{% highlight java %}
public static void main(String[] args) {
  Book book = new Book();
  Printer printer = new PlainTextPrinter();
  printer.printPage(book.getCurrentPage());
}
{% endhighlight %}

Similarly, we can remove the saving responsibility from the `Book` class and create a specific class that handles this operation.
{% highlight java %}
public class SimpleFilePersistance {
  public void save(Book book) {
    // Persist the book to a file
  }
}
{% endhighlight %}

Our `Book` class is now much simpler and more cohesive:
{% highlight java %}
public class Book {

  public String getTitle() {
    return "Some title";
  }

  public String getAuthor() {
    return "Some author";
  }

  public Page getCurrentPage() {
    // Return the current page
  }
}
{% endhighlight %}

We can use the `SimpleFilePersistance` class as follows.
{% highlight java %}
public static void main(String[] args) {
  Book book = new Book();

  Printer printer = new PlainTextPrinter();
  printer.printPage(book.getCurrentPage());

  SimpleFilePersistance simpleFilePersistance = new SimpleFilePersistance();
  simpleFilePersistance.save(book);
}
{% endhighlight %}
