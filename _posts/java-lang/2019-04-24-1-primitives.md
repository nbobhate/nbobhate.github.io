---
layout: post
title: Primitives
published: true
tags: [Java Language]
---

A summary all about Java primitives.

# Basics
### Integers

- Signed (positive and negative) number values

| Name | Width | Information |
|--|--|--|
| `byte` | 8 | Smallest integer, useful for handling streams of data
| `short` | 16 | Least used
| `int` | 32 | Most common integer type, bytes and shorts are promoted to `int` when they are evaluated in an expression
| `long` | 64 | Useful when big whole numbers are needed |

### Floating Point Numbers

- Useful when dealing with real numbers and calculations that require fractional precision

| Name | Width | Information |
|--|--|--|
| `float` | 32 | Single precision value, useful when a large degree of precision isn't necessary
| `double` | 64 | Double precision value, most times faster than single precision, optimised for high-speed mathematical calculations

### Characters

| Name | Width | Information |
|--|--|--|
| `char` | 16 | Stores Unicode characters, no negative values, technically an integral type so ++/-- can be applied to it

### Boolean

| Name | Width | Information |
|--|--|--|
| `boolean` | VM dependant | Logical values, either `true` or `false`


# Type Conversion and Casting

Automatic type conversion, also known as a *widening conversion* will happen if
- the two types are compatible
- the destination type is larger than the source type

For example, an `int` can be assigned to a `long`.

Attempting to assign an `int` to a `byte` is known as a narrowing conversion and won't be done automatically. Since the destination is smaller than the source type, you need to use explicit casting, like this `byte byteVal = (byte) intVal;`.

For example:
- `int` -> `byte`: If the integer value is larger than the range of byte, the resulting value will be the original value modulo byte's range.
- `float` -> `int`: The fraction is simply truncated.


# Automatic Type Promotion

This is done because the precision required of an intermediate value in an expression may sometimes exceed the range of either operand.

For example in the following code, the subexpression `a * b` exceeds the range of a `byte` so Java automatically promotes these both to type `int`.
```java
byte a = 40;
byte b = 50;
byte c = 100;
int d = a * b / c;
```
This is what can sometimes cause the following compiler error to happen.
```java
byte b = 50;  
b = b * 2; // Error! Cannot assign an int to a byte!
```

The rules for type promotion are:
- All `byte`, `short` and `char` types are promoted to `int`
- If an operand is type `long`, all operands are promoted to `long`
- If an operand is type `float`, all operands are promoted to `float`
- If an operand is type `double`, all operands are promoted to `double`
