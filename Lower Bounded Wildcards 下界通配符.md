# Lower Bounded Wildcards 下界通配符

The [Upper Bounded Wildcards](https://docs.oracle.com/javase/tutorial/java/generics/upperBounded.html) section shows that an upper bounded wildcard restricts the unknown type to be a specific type or a subtype of that type and is represented using the `extends` keyword. In a similar way, a *lower bounded* wildcard restricts the unknown type to be a specific type or a *super type* of that type.  
[上界通配符](Upper%20Bounded%20Wildcards%20上界通配符.md)部分显示，上界通配符将未知类型限制为特定类型或该类型的子类型，并使用`extends`关键字表示。 以类似的方式，*下界通配符*将未知类型限制为特定类型或该类型的*超类型*。

A lower bounded wildcard is expressed using the wildcard character ('`?`'), following by the `super` keyword, followed by its *lower bound*: `<? super A>`.  
下界通配符使用通配符（'`?`'）表示，后跟`super`关键字，在跟其*下界*：`<? super A>`。

------

**Note:** You can specify an upper bound for a wildcard, or you can specify a lower bound, but you cannot specify both.  
**注意：** 你可以指定通配符的上界，也可以指定下界，但不能同时指定两者。

------

Say you want to write a method that puts `Integer` objects into a list. To maximize flexibility, you would like the method to work on `List<Integer>`, `List<Number>`, and `List<Object>` — anything that can hold `Integer` values.  
假设你要编写一个将`Integer`对象放入列表的方法。 为了最大程度地提高灵活性，你希望该方法可用于`List <Integer>`，`List<Number>`和`List<Object>` —可以容纳`Integer`值的任何内容。

To write the method that works on lists of `Integer` and the supertypes of `Integer`, such as `Integer`, `Number`, and `Object`, you would specify `List<? super Integer>`. The term `List<Integer>` is more restrictive than `List<? super Integer>` because the former matches a list of type `Integer` only, whereas the latter matches a list of any type that is a supertype of `Integer`.  
要编写适用于`Integer`和`Integer`超类型（例如`Integer`，`Number`和`Object`）列表的方法，你可以指定`List<? super Integer>`。 术语`List<Integer>`的限制比`List<? super Integer>`严格，因为前者仅匹配`Integer`类型的列表，而后者则匹配`Integer`超类型的任何类型的列表。


The following code adds the numbers 1 through 10 to the end of a list:  
下面的代码将数字1到10添加到列表的末尾:
```java
public static void addNumbers(List<? super Integer> list) {
    for (int i = 1; i <= 10; i++) {
        list.add(i);
    }
}
```

The [Guidelines for Wildcard Use](https://docs.oracle.com/javase/tutorial/java/generics/wildcardGuidelines.html) section provides guidance on when to use upper bounded wildcards and when to use lower bounded wildcards.  
 [通配符使用准则](Guidelines%20for%20Wildcard%20Use%20通配符使用准则.md)部分提供有关何时使用上界通配符以及何时使用下界通配符的指南。