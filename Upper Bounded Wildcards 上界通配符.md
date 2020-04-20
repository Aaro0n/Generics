# Upper Bounded Wildcards 上界通配符

You can use an upper bounded wildcard to relax the restrictions on a variable. For example, say you want to write a method that works on `List<Integer>`, `List<Double>,`, *and* `List<Number>`; you can achieve this by using an upper bounded wildcard.  
你可以使用上限通配符来放宽对变量的限制。例如，假设你要编写一种适用于`List<Integer>`，`List<Double>`和`List<Number>`的方法； 你可以使用上界通配符来实现。

To declare an upper-bounded wildcard, use the wildcard character ('`?`'), followed by the `extends` keyword, followed by its *upper bound*. Note that, in this context, `extends` is used in a general sense to mean either "extends" (as in classes) or "implements" (as in interfaces).  
要声明上界通配符，请使用通配符（'`？`'），后接`extends`关键字，后接其*上届*。注意，在这种情况下，“extends”在一般意义上用来表示“extends”（如在类中）或“implements”（如在接口中）。

To write the method that works on lists of `Number` and the subtypes of `Number`, such as `Integer`, `Double`, and `Float`, you would specify `List<? extends Number>`. The term `List<Number>` is more restrictive than `List<? extends Number>` because the former matches a list of type `Number` only, whereas the latter matches a list of type `Number` or any of its subclasses.  
要编写适用于`Number`和`Number`的子类型（例如`Integer`，`Double`和`Float`）列表的方法，你可以指定`List<? extends Number>`。 术语`List<Number>`比`List<? extends Number>`有更多限制。因为前者仅匹配`Number`类型的列表，而后者匹配`Number`类型或其任何子类的列表。

Consider the following `process` method:  
考虑下面`process`方法：

```java
public static void process(List<? extends Foo> list) { /* ... */ }
```

The upper bounded wildcard, `<? extends Foo>`, where `Foo` is any type, matches `Foo` and any subtype of `Foo`. The `process` method can access the list elements as type `Foo`:  
上届通配符`<? extends Foo>`，`Foo`是匹配`Foo`和其子类型的任何类型。`process`方法可以访问元素类型为`Foo`的列表：

```java
public static void process(List<? extends Foo> list) {
    for (Foo elem : list) {
        // ...
    }
}
```

In the `foreach` clause, the `elem` variable iterates over each element in the list. Any method defined in the `Foo` class can now be used on `elem`.  
在`foreach`子句中，`elem`变量遍历列表中的每个元素。 现在可以在`elem`上使用`Foo`类中定义的任何方法。

The `sumOfList` method returns the sum of the numbers in a list:  
`sumOfList`方法返回列表中数字的总和：
```java
public static double sumOfList(List<? extends Number> list) {
    double s = 0.0;
    for (Number n : list)
        s += n.doubleValue();
    return s;
}
```

The following code, using a list of `Integer` objects, prints `sum = 6.0`:  
下面的代码，使用 `Integer`对象列表，打印出 `sum = 6.0`：
```java
List<Integer> li = Arrays.asList(1, 2, 3);
System.out.println("sum = " + sumOfList(li));
```

A list of `Double` values can use the same `sumOfList` method. The following code prints `sum = 7.0`:  
以`Double`为值的列表可以使用相同的方法`sumOfList`.下面代码打印了`sum = 7.0`：

```java
List<Double> ld = Arrays.asList(1.2, 2.3, 3.5);
System.out.println("sum = " + sumOfList(ld));
```