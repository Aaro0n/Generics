# Questions and Exercises: Generics 问题与练习：泛型

1. Write a generic method to count the number of elements in a collection that have a specific property (for example, odd integers, prime numbers, palindromes). 编写一个泛型方法来计算集合中具有特定属性（例如，奇数整数，素数，回文数）的元素数。
  
2. Will the following class compile? If not, why? 下面的类会编译吗？ 如果没有，为什么？
```java
public final class Algorithm {
    public static <T> T max(T x, T y) {
      return x > y ? x : y;
    }
}
```

3. Write a generic method to exchange the positions of two different elements in an array. 编写泛型方法来交换数组中两个不同元素的位置。



   

4. If the compiler erases all type parameters at compile time, why should you use generics? 如果编译器在编译时擦除所有类型参数，那么为什么要使用泛型？

   

5. What is the following class converted to after type erasure? 类型擦除后，以下类转换为什么？

```java
public class Pair<K, V> {

   public Pair(K key, V value) {
       this.key = key;
       this.value = value;
   }

   public K getKey(); { return key; }
   public V getValue(); { return value; }

   public void setKey(K key)     { this.key = key; }
   public void setValue(V value) { this.value = value; }

   private K key;
   private V value;
}
```

6. What is the following method converted to after type erasure? 类型擦除后，以下方法转换为什么？

```java
public static <T extends Comparable<T>>
   int findFirstGreaterThan(T[] at, T elem) {
   // ...
}
```

7. Will the following method compile? If not, why? 下面的方法会编译吗？如果没有。为什么？

```java
public static void print(List<? extends Number> list) {
   for (Number n : list)
       System.out.print(n + " ");
   System.out.println();
}
```

8. Write a generic method to find the maximal element in the range `[begin, end)` of a list. 编写一个泛型方法能在列表的 `[begin, end)` 范围内找到最大元素。

   

9. Will the following class compile? If not, why? 下面的类会被编译吗？如果没有，为什么？

```java
public class Singleton<T> {

   public static T getInstance() {
       if (instance == null)
           instance = new Singleton<T>();

       return instance;
   }

   private static T instance = null;
}
```

10. Given the following classes: 给定以下类：

```java
class Shape { /* ... */ }
class Circle extends Shape { /* ... */ }
class Rectangle extends Shape { /* ... */ }

class Node<T> { /* ... */ }
```

Will the following code compile? If not, why? 下面的代码会被编译吗？如果没有，为什么？

```java
Node<Circle> nc = new Node<>();
Node<Shape>  ns = nc;
```

11. Consider this class: 考虑这个类：

```java
class Node<T> implements Comparable<T> {
    public int compareTo(T obj) { /* ... */ }
    // ...
}
```

Will the following code compile? If not, why? 
下面的代码会被编译吗？如果没有，为什么？

```java
Node<String> node = new Node<>();
Comparable<String> comp = node;
```

12. How do you invoke the following method to find the first integer in a list that is relatively prime to a list of specified integers? 你如何调用以下方法在列表中查找相对于指定整数列表而言是素数的第一个整数？

```java
public static <T> int findFirst(List<T> list, int begin, int end, UnaryPredicate<T> p)
```

Note that two integers `a` and `b` are relatively prime if `gcd(a, b) = 1`, where `gcd` is short for greatest common divisor.
请注意，如果`gcd(a,b) = 1`，则两个整数`a`和`b`是相对质数，其中`gcd`是最大公约数的缩写。

[Check your answers. ](https://docs.oracle.com/javase/tutorial/java/generics/QandE/generics-answers.html)
[检查你的答案。](https://docs.oracle.com/javase/tutorial/java/generics/QandE/generics-answers.html)