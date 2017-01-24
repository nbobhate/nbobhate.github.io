---
layout: post
title: Stack Memory vs Heap Memory
published: true
tags: [Java]
---

It is oftem easy to ignore what your application is doing when it gets right down to the bits and nibbles. However, having some idea of how the JVM manages memory is highly beneficial, especially when you start getting serious about low level optimisation. Of course there is a lot more to it, but here's a simplified start describing stack memory and heap memory.<!--more-->

Let's assume you have a basic "Hello world" Java application. In fact, here's a sample I'll be referring to.

{% highlight java %}
public class Main {

    public static void main(String[] args) { //1

        LocalDate localDate; //2

        localDate = LocalDate.now(); //3

        int primitive; //4

        primitive = 4; //5

        doAction(localDate); //6

        System.out.println(primitive); //8
    }

    private static void doAction(LocalDate localDate) {
        System.out.println(localDate); //7
    }
}
{% endhighlight %}

When the main method is run, the following steps occur:
1. A frame is created in the stack to represent the ``main()`` method.
2. The next line in the method declares a local reference variable of type ``LocalDate``. A slot is created in the current frame to hold and manage the value of this reference variable. Note that no object has been created yet.
3. A new ``LocalDate`` object is created in the heap. The memory address it occupies in heap memory is then stored in the slot assigned for the ``localDate`` reference variable in the current frame.
4. A new local variable is declared for an integer primitive type. Another slot is created in the current frame. Note that the variable has not been assigned a value yet.
5. We assign the value ``4`` to the local variable. Since this is for a primitive type, the actual value is stored in the slot that was created in the above step. This means that the variable ``primitive`` points to the actual value assigned to the variable and not a reference to it.
6. The main method now calls another method. This triggers the creation of a new frame on the stack to hold all the data required for the ``doAction()`` method. We now have two frames on the stack, with the newest frame at the top.
7. The ``doAction()`` method will execute the lines relevant to it. Once the method is done executing, the frame created for it and all its containing data is removed from the stack.
8. Execution then falls back to the next available frame on the stack, in this case the frame for the ``main()`` method. The remaining lines of the main method are executed.

In a similar fashion, once execution is complete in the ``main()`` method frame, the frame and contents are removed from memory.

Some observations can be made from the above. Memory management of the stack is done in a LAST IN FIRST OUT manner, whereas, memory management of the heap is taken care of by the Garbage Collector. If the heap runs out of memory, a ``java.lang.OutOfMemoryError: Java Heap Space`` error is thrown. If the stack runs out of memory, a ``java.lang.StackOverFlowError `` error is thrown.
