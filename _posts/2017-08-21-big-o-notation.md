---
layout: post
title: "Oh no! It's Big O!"
published: true
tags: [Algorithms]
---

Amongst the many considerations to keep in mind when creating an algorithm, time complexity is probably the most important. When determining time complexity of an algorithm, we want to know how an algorithm scales when the input data increases in size. This is known as the order of growth of the algorithm and can be expressed using a mathematical notation called the **Big O** notation.

We can follow these rules when trying to determine the Big O time complexity of an algorithm:
- 1 unit of time for arithmetical/logical operations
- 1 unit of time for assignment/return operations
- 1 unit of time for memory access operations
- Drop all constants
- Drop lower order values
  - \\( O(1) < O(logn) < O(n) < O(nlogn) < O(n^2) < O(2^n) < O(n!) \\)

# 1. Constant Time Algorithms
Algorithms or functions that execute in the same amount of time regardless of the input data size are said to have a ***constant time complexity***, or \\( O(1) \\). In this example, adding an item to the global list object will take the same amount of time no matter what the size of the data.

{% highlight java %}
public List<String> allStrings = new ArrayList<>();

public void addItemToList(String item) {
  allStrings.add(items);
}
{% endhighlight %}

# 2. Linear Time Algorithms
Algorithms where the time to complete is directly proportional to the size of the input data are said to have ***linear time complexity***. A good example of this is to search for an item in an array by checking each item in turn. The worst case scenario is that the item we are looking for is the last item in the array. If the array size is \\( n \\), we can say that the function below has a complexity of \\( O(n) \\).

{% highlight java %}
public List<String> allStrings = new ArrayList<>();

public String searchForItem(String item) {
  int n = allStrings.size();
  for (int i = 0; i < n; i++) {
    if (allStrings.get(i).equals(item)) {
      return allStrings.get(i);
    }
  }
}
{% endhighlight %}

# 3. Quadratic Time Algorithms
When the time to complete for an algorithm is proportional to the square of the amount of input data, or cubed, etc, then we say that the algorithm has ***quadratic time complexity***. So as the input data increases, the time take for the algorithm to run increases drastically. This is extremely inefficient for large data sets so algorithms that have a quadratic time complexity should be avoided. A well used example of this is the **Bubble Sort** algorithm. Each iteration of the outer loop requires the complete traversal of the inner loop. This gives us the complexity \\( O(n^2) \\).

{% highlight java %}
public int[] someArray = new int[]{10,2,5,6,18,9};
public int n = someArray.length;

public void bubblesort() {
  for (int i = n-1; i > 1; i--) {
    for (int j = 0; j < i; j++) {
      if (someArray[j] > someArray[j+1]) {
        swap(j, j+1);
      }
    }
  }
}
{% endhighlight %}

# 4. Logarithmic Time Algorithms
Algorithms that have a time complexity of \\( O(logn) \\) or \\( O(nlogn) \\) are said to have a ***logarithmic time complexity***.

A complexity of \\( O(logn) \\) is given to an algorithm when the input data is decreased by about 50% each time the algorithm is run. This characteristic makes algorithms with a logarithmic time complexity ideal because the time required to complete isn't negatively impacted by larger data sets. A perfect example of this is the **Binary Search** algorithm.

{% highlight java %}
public int[] someArray = new int[]{2,5,6,9,10,18};

public void binarySearch(int value) {
  int lowIndex = 0;
  int highIndex = someArray.length - 1;

  while (lowIndex <= highIndex) {
    int middleIndex = (highIndex + lowIndex) / 2;

    if (someArray[middleIndex] < value) {
      lowIndex = middleIndex + 1;
    } else if (someArray[middleIndex] > value) {
      highIndex = middleIndex - 1;
    } else {
      // found match
      lowIndex = highIndex + 1; // to terminate the while loop
    }
  }
}
{% endhighlight %}

# Resources
These were a big help to me:
- [Big-O Notations](https://www.youtube.com/watch?v=V6mKVRU1evU)
- [Big-O Notation in 5 Minutes - The Basics](https://www.youtube.com/watch?v=__vX2sjlpXU)
- [Log N](https://www.youtube.com/watch?v=kjDR1NBB9MU)
- [Big-O Cheatsheet](http://bigocheatsheet.com/)
