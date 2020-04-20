# Unbounded Wildcards 无界通配符

The unbounded wildcard type is specified using the wildcard character (`?`), for example, `List<?>`. This is called a *list of unknown type*. There are two scenarios where an unbounded wildcard is a useful approach:  
无界通配符类型使用通配符（`?`）指定，例如，`List<?>`。 这称为*未知类型列表*。 在两种情况下，无界通配符是一种有用的方法：

- If you are writing a method that can be implemented using functionality provided in the `Object` class.  
如果你正在编写可以使用`Object`类中提供的功能实现的方法。
- When the code is using methods in the generic class that don't depend on the type parameter. For example, `List.size` or `List.clear`. In fact, `Class<?>` is so often used because most of the methods in `Class<T>` do not depend on `T`.  
当代码使用泛型类中不依赖于类型参数的方法时。 例如，`List.size`或`List.clear`。 实际上，`Class<?>`经常被使用，因为`Class<T>`中的大多数方法都不依赖于`T`。

Consider the following method, `printList`:  
考虑下面的方法，`printList`：

```java
public static void printList(List<Object> list) {
    for (Object elem : list)
        System.out.println(elem + " ");
    System.out.println();
}
```

The goal of `printList` is to print a list of any type, but it fails to achieve that goal — it prints only a list of `Object` instances; it cannot print `List<Integer>`, `List<String>`, `List<Double>`, and so on, because they are not subtypes of `List<Object>`. To write a generic `printList` method, use `List<?>`:  
`printList`的目的是打印任何类型的列表，但是它并没有达到这个目的——它只能打印`Object`实例的列表；它不能打印`List<Integer>`，`List<String>`，`List<Double>`等，因为它们不是`List<Object>`的子类型。要编写通用的`printList`方法，请使用`List<?>`：
```java
public static void printList(List<?> list) {
    for (Object elem: list)
        System.out.print(elem + " ");
    System.out.println();
}
```

Because for any concrete type `A`, `List<A>` is a subtype of `List<?>`, you can use `printList` to print a list of any type:  
因为对于任何具体类型`A`，`List<A>`是`List<?>`的子类型，所以可以使用`printList`打印任何类型的列表：

```java
List<Integer> li = Arrays.asList(1, 2, 3);
List<String>  ls = Arrays.asList("one", "two", "three");
printList(li);
printList(ls);
```

------

**Note:** The [`Arrays.asList`](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#asList-T...-) method is used in examples throughout this lesson. This static factory method converts the specified array and returns a fixed-size list.  
**注意：** 在本课程的示例中，将使用[`Arrays.asList`](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#asList-T...-)方法。 此静态工厂方法将转换指定的数组并返回固定大小的列表。

------

It's important to note that `List<Object>` and `List<?>` are not the same. You can insert an `Object`, or any subtype of `Object`, into a `List<Object>`. But you can only insert `null` into a `List<?>`. The [Guidelines for Wildcard Use](https://docs.oracle.com/javase/tutorial/java/generics/wildcardGuidelines.html) section has more information on how to determine what kind of wildcard, if any, should be used in a given situation.  
重要的是要注意`List<Object>`和`List<?>`不同。可以将对象或对象的任何子类型插入`List<Object>`。 但是只能将`null`插入`List<?>`。 [通配符使用准则](Guidelines%20for%20Wildcard%20Use%20通配符使用准则.md)部分提供了有关如何确定在给定情况下应使用哪种通配符，如果有，应该在给定的情况下使用。