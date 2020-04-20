# Guidelines for Wildcard Use 通配符使用准则

One of the more confusing aspects when learning to program with generics is determining when to use an upper bounded wildcard and when to use a lower bounded wildcard. This page provides some guidelines to follow when designing your code.  
在学习使用泛型编程时，更令人困惑的方面之一是确定何时使用上界通配符以及何时使用下界通配符。 此页面提供了一些在设计代码时要遵循的准则。

For purposes of this discussion, it is helpful to think of variables as providing one of two functions:  
为了便于讨论，将变量视为提供以下两个功能之一将很有帮助：

- **An "In" Variable**  **"In" 变量**

  An "in" variable serves up data to the code. Imagine a copy method with two arguments: `copy(src, dest)`. The `src` argument provides the data to be copied, so it is the "in" parameter.  
  "in"变量为代码提供数据。想象一个有两个参数的copy函数：`copy(src, dest)`。`src`参数提供要复制的数据，因此它是“in“参数。

- **An "Out" Variable**  **"Out" 变量**

  An "out" variable holds data for use elsewhere. In the copy example, `copy(src, dest)`, the `dest` argument accepts data, so it is the "out" parameter.  
  “out”变量保存要在其他地方使用的数据。 在复制示例`copy(src, dest)`中，`dest`参数接受数据，因此它是“out“参数。

Of course, some variables are used both for "in" and "out" purposes — this scenario is also addressed in the guidelines.  
当然，某些变量既用于“in”又用于“out”目的——准则中也解决了这种情况。

You can use the "in" and "out" principle when deciding whether to use a wildcard and what type of wildcard is appropriate. The following list provides the guidelines to follow:  
在决定是否使用通配符以及哪种类型的通配符时，可以使用“in”和“out”原理。 以下列表提供了要遵循的准则：

------

**Wildcard Guidelines:**  **通配符准则：**

- An "in" variable is defined with an upper bounded wildcard, using the `extends` keyword.  
“in”变量使用一个上界通配符定义，使用`extends`关键字。
- An "out" variable is defined with a lower bounded wildcard, using the `super` keyword.  
“out”变量使用一个下界通配符定义，使用`super`关键字。
- In the case where the "in" variable can be accessed using methods defined in the `Object` class, use an unbounded wildcard.  
如果可以使用`Object`类中定义的方法访问“ in”变量，请使用无界通配符。
- In the case where the code needs to access the variable as both an "in" and an "out" variable, do not use a wildcard.  
如果代码需要访问这个变量如同“in”和“out”两个变量一样，不要使用通配符。

------

These guidelines do not apply to a method's return type. Using a wildcard as a return type should be avoided because it forces programmers using the code to deal with wildcards.  
这些准则不适用于方法的返回类型。 应该避免使用通配符作为返回类型，因为这会迫使程序员使用代码来处理通配符。

A list defined by `List<? extends ...>` can be informally thought of as read-only, but that is not a strict guarantee. Suppose you have the following two classes:  
由`List<? extends ...>`定义的列表可以被非正式地认为是只读的，但这不是严格的保证。 假设你具有以下两个类：
```java
class NaturalNumber {

    private int i;

    public NaturalNumber(int i) { this.i = i; }
    // ...
}

class EvenNumber extends NaturalNumber {

    public EvenNumber(int i) { super(i); }
    // ...
}
```

Consider the following code:  
考虑下面代码：

```java
List<EvenNumber> le = new ArrayList<>();
List<? extends NaturalNumber> ln = le;
ln.add(new NaturalNumber(35));  // compile-time error
```

Because `List<EvenNumber>` is a subtype of `List<? extends NaturalNumber>`, you can assign `le` to `ln`. But you cannot use `ln` to add a natural number to a list of even numbers. The following operations on the list are possible:  
由于`List<EvenNumber>`是`List<? extends NaturalNumber>`子类，你可以把`le`赋值给`ln`。但是你不能使用`ln`将自然数添加到偶数列表中。列表中的以下操作是可能的：

- You can add `null`. 你不能添加 `null`。
- You can invoke `clear`.你可以调用 `clear`。
- You can get the iterator and invoke `remove`. 你可以获取迭代器并调用`remove`。
- You can capture the wildcard and write elements that you've read from the list. 你可以捕获通配符并写入从列表中读取的元素。

You can see that the list defined by `List<? extends NaturalNumber>` is not read-only in the strictest sense of the word, but you might think of it that way because you cannot store a new element or change an existing element in the list.  
你可以看到由`List<? extends NaturalNumber>`定义的列表从严格意义上来说，不是只读的，但你可能会这样想，因为你无法存储新元素或更改列表中的现有元素。