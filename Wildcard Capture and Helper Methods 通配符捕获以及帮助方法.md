# Wildcard Capture and Helper Methods 通配符捕获以及帮助方法

In some cases, the compiler infers the type of a wildcard. For example, a list may be defined as `List<?>` but, when evaluating an expression, the compiler infers a particular type from the code. This scenario is known as *wildcard capture*.  
在某些情况下，编译器会推断通配符的类型。例如，一个列表也许会定义为`List<?>`，但是，当评估这个表达式时，编译器会从代码中推断出一个特定的类型。这种情况称为*通配符捕获*。

For the most part, you don't need to worry about wildcard capture, except when you see an error message that contains the phrase "capture of".  
在大多数情况下，你无需担心通配符捕获，除非你看到包含短语“capture of”的错误消息。

The [`WildcardError`](https://docs.oracle.com/javase/tutorial/java/generics/examples/WildcardError.java) example produces a capture error when compiled:  
[`WildcardError`](https://docs.oracle.com/javase/tutorial/java/generics/examples/WildcardError.java) 这个例子在编译时产生了一个捕获错误：

```java
import java.util.List;

public class WildcardError {

    void foo(List<?> i) {
        i.set(0, i.get(0));
    }
}
```

In this example, the compiler processes the `i` input parameter as being of type `Object`. When the `foo` method invokes [List.set(int, E)](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#set-int-E-), the compiler is not able to confirm the type of object that is being inserted into the list, and an error is produced. When this type of error occurs it typically means that the compiler believes that you are assigning the wrong type to a variable. Generics were added to the Java language for this reason — to enforce type safety at compile time.  
在此示例中，编译器将`i`输入参数处理为`Object`类型。当`foo`方法调用[List.set(int, E)](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#set-int-E-)，编译器无法确认要插入列表中的对象的类型，并产生错误。当发生这种类型的错误时，通常意味着编译器认为你正在将错误的类型分配给变量。由于这个原因，泛型被添加到Java语言中——在编译时加强类型安全。

The `WildcardError` example generates the following error when compiled by Oracle's JDK 7 `javac` implementation:  
当由Oracle的JDK 7 `javac`实现编译时，`WildcardError`示例生成以下错误:

```java
WildcardError.java:6: error: method set in interface List<E> cannot be applied to given types;
    i.set(0, i.get(0));
     ^
  required: int,CAP#1
  found: int,Object
  reason: actual argument Object cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Object from capture of ?
1 error
```

In this example, the code is attempting to perform a safe operation, so how can you work around the compiler error? You can fix it by writing a *private helper method* which captures the wildcard. In this case, you can work around the problem by creating the private helper method, `fooHelper`, as shown in [`WildcardFixed`](https://docs.oracle.com/javase/tutorial/java/generics/examples/WildcardFixed.java):  
在此示例中，代码正在尝试执行安全操作，那么如何解决编译器错误？你可以通过编写捕获通配符的 *私有帮助方法* 来修复它。在这种情况下，你可以通过创建 *私有帮助方法* `fooHelper`来解决此问题，如[`WildcardFixed`](https://docs.oracle.com/javase/tutorial/java/generics/examples/WildcardFixed.java)中所示

```java
public class WildcardFixed {

    void foo(List<?> i) {
        fooHelper(i);
    }


    // Helper method created so that the wildcard can be captured
    // through type inference.
    private <T> void fooHelper(List<T> l) {
        l.set(0, l.get(0));
    }

}
```

Thanks to the helper method, the compiler uses inference to determine that `T` is `CAP#1`, the capture variable, in the invocation. The example now compiles successfully.  
多亏了帮助方法，编译器在调用中使用推断来确定`T`是捕获变量`CAP#1`。这个例子现在可以成功编译。

By convention, helper methods are generally named `*originalMethodName*Helper`.  
按照惯例，帮助方法通常命名为`*originalMethodName*Helper`。

Now consider a more complex example, [`WildcardErrorBad`](https://docs.oracle.com/javase/tutorial/java/generics/examples/WildcardErrorBad.java):  
现在考虑一个更复杂的例子，[`WildcardErrorBad`](https://docs.oracle.com/javase/tutorial/java/generics/examples/WildcardErrorBad.java):
```java
import java.util.List;

public class WildcardErrorBad {

    void swapFirst(List<? extends Number> l1, List<? extends Number> l2) {
      Number temp = l1.get(0);
      l1.set(0, l2.get(0)); // expected a CAP#1 extends Number,
                            // got a CAP#2 extends Number;
                            // same bound, but different types
      l2.set(0, temp);	    // expected a CAP#1 extends Number,
                            // got a Number
    }
}
```

In this example, the code is attempting an unsafe operation. For example, consider the following invocation of the `swapFirst` method:  
在此的示例代码正在尝试不安全的操作。 例如，考虑以下对`swapFirst`方法的调用：

```java
List<Integer> li = Arrays.asList(1, 2, 3);
List<Double>  ld = Arrays.asList(10.10, 20.20, 30.30);
swapFirst(li, ld);
```

While `List<Integer>` and `List<Double>` both fulfill the criteria of `List<? extends Number>`, it is clearly incorrect to take an item from a list of `Integer` values and attempt to place it into a list of `Double` values.  
`List<Integer>`和`List<Double>`都满足`List<? extends Number>`规范，那么从`Integer`列表中取出一项并将其放入`Double`列表中显然是不正确的。

Compiling the code with Oracle's JDK `javac` compiler produces the following error:  
使用Oracle的JDK `javac`编译器编译代码会产生以下错误：

```java
WildcardErrorBad.java:7: error: method set in interface List<E> cannot be applied to given types;
      l1.set(0, l2.get(0)); // expected a CAP#1 extends Number,
        ^
  required: int,CAP#1
  found: int,Number
  reason: actual argument Number cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Number from capture of ? extends Number
WildcardErrorBad.java:10: error: method set in interface List<E> cannot be applied to given types;
      l2.set(0, temp);      // expected a CAP#1 extends Number,
        ^
  required: int,CAP#1
  found: int,Number
  reason: actual argument Number cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Number from capture of ? extends Number
WildcardErrorBad.java:15: error: method set in interface List<E> cannot be applied to given types;
        i.set(0, i.get(0));
         ^
  required: int,CAP#1
  found: int,Object
  reason: actual argument Object cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Object from capture of ?
3 errors
```

There is no helper method to work around the problem, because the code is fundamentally wrong.  
没有解决此问题的帮助方法，因为代码根本上是错误的。