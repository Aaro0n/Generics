# Erasure of Generic Methods 泛型方法擦除

The Java compiler also erases type parameters in generic method arguments. Consider the following generic method:  
Java编译器还会擦除泛型方法参数中的类型参数。 考虑以下通用方法：

```java
// Counts the number of occurrences of elem in anArray.
//
public static <T> int count(T[] anArray, T elem) {
    int cnt = 0;
    for (T e : anArray)
        if (e.equals(elem))
            ++cnt;
        return cnt;
}
```

Because `T` is unbounded, the Java compiler replaces it with `Object`:  
由于`T`是无界的，因此Java编译器将其替换为`Object`：

```java
public static int count(Object[] anArray, Object elem) {
    int cnt = 0;
    for (Object e : anArray)
        if (e.equals(elem))
            ++cnt;
        return cnt;
}
```

Suppose the following classes are defined:  
假设定义了以下类：

```java
class Shape { /* ... */ }
class Circle extends Shape { /* ... */ }
class Rectangle extends Shape { /* ... */ }
```

You can write a generic method to draw different shapes:  
你可以编写一个泛型方法来绘制不同的形状：

```java
public static <T extends Shape> void draw(T shape) { /* ... */ }
```

The Java compiler replaces `T` with `Shape`:  
Java编译器将`T`替换为`Shape`：

```java
public static void draw(Shape shape) { /* ... */ }
```