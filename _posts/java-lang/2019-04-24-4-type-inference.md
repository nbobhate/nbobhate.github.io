---
layout: post
title: Local Variable Type Inference
published: true
tags: [Java Language]
---

Type inference for local variables is a new feature as of Java 10. Please read [Variables]({% post_url 2019-04-24-2-variables %}) first.

It is possible to let the compiler infer the type of a local variable based on the type of its initializer. This streamlines the code by removing the need to include the type in the variable declaration. Instead, the context sensitive word, `var` is used.

```java
// Existing way to declare a variable
double cost = 12.50;

// Type infernce as of Java 10
var cost = 12.50;

// Also applicable to arrays
var intArr = new int[5];
```

Since `var` iscontext sensitive, it can still be used as a variable name, although this may lead to a bit of confusion:
```java
int var = 1;
```

## Some Restrictions

It is not possible to infer the type of a variable without an initializer, therefore, the following statement is wrong:
```java
var counter;
```

Only one variable can be declared at a time.

`Null` can't be used as an initializer.

An array initializer can't be used with `var`, for example:
```java
// This is correct
var intArr = new int[5];

// This is wrong
var intArr = {2, 4, 6, 8, 10}};
```

Can also be used in `for` loops:
```java
for (var i = 0; i < 10; i++) {
  ...
}

for (var str : strArr) {
  ...
}

// And for objects
var dog = new Dog();
```
