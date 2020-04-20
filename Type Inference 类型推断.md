# Type Inference 类型推断

*Type inference* is a Java compiler's ability to look at each method invocation and corresponding declaration to determine the type argument (or arguments) that make the invocation applicable. The inference algorithm determines the types of the arguments and, if available, the type that the result is being assigned, or returned. Finally, the inference algorithm tries to find the *most specific* type that works with all of the arguments.  
*类型推断*是Java编译器通过查看每个方法调用和相应声明以确定类型参数（或参数）使得该调用可行的能力。 推断算法确定参数的类型，以及确定被分配或返回的结果的类型（如果有）。 最后，推断算法试图找到适用于所有参数的*最具体*类型。

To illustrate this last point, in the following example, inference determines that the second argument being passed to the `pick` method is of type `Serializable`:  
为了说明最后一点，在下面的示例中，推断确定传递给`pick`方法的第二个参数的类型为`Serializable`：

```java
static <T> T pick(T a1, T a2) { return a2; }
Serializable s = pick("d", new ArrayList<String>());
```

## Type Inference and Generic Methods 类型推断和泛型函数

[Generic Methods](https://docs.oracle.com/javase/tutorial/java/generics/methods.html) introduced you to type inference, which enables you to invoke a generic method as you would an ordinary method, without specifying a type between angle brackets. Consider the following example, [`BoxDemo`](https://docs.oracle.com/javase/tutorial/java/generics/examples/BoxDemo.java), which requires the [`Box`](https://docs.oracle.com/javase/tutorial/java/generics/examples/Box.java) class:  
泛型函数引入了类型推断，使你可以像调用普通函数一样调用泛型函数，而无需在尖括号之间指定类型。考虑下面的示例，[`BoxDemo`](https://docs.oracle.com/javase/tutorial/java/generics/examples/BoxDemo.java)，它需要[`Box`](https://docs.oracle.com/javase/tutorial/java/generics/examples/Box.java)类：


```java
public class BoxDemo {

  public static <U> void addBox(U u, 
      java.util.List<Box<U>> boxes) {
    Box<U> box = new Box<>();
    box.set(u);
    boxes.add(box);
  }

  public static <U> void outputBoxes(java.util.List<Box<U>> boxes) {
    int counter = 0;
    for (Box<U> box: boxes) {
      U boxContents = box.get();
      System.out.println("Box #" + counter + " contains [" +
             boxContents.toString() + "]");
      counter++;
    }
  }

  public static void main(String[] args) {
    java.util.ArrayList<Box<Integer>> listOfIntegerBoxes =
      new java.util.ArrayList<>();
    BoxDemo.<Integer>addBox(Integer.valueOf(10), listOfIntegerBoxes);
    BoxDemo.addBox(Integer.valueOf(20), listOfIntegerBoxes);
    BoxDemo.addBox(Integer.valueOf(30), listOfIntegerBoxes);
    BoxDemo.outputBoxes(listOfIntegerBoxes);
  }
}
```

The following is the output from this example:  
下面是这个例子的输出：

```
Box #0 contains [10]
Box #1 contains [20]
Box #2 contains [30]
```

The generic method `addBox` defines one type parameter named `U`. Generally, a Java compiler can infer the type parameters of a generic method call. Consequently, in most cases, you do not have to specify them. For example, to invoke the generic method `addBox`, you can specify the type parameter with a *type witness* as follows:  
泛型函数`addBox`定义了一个参数类型`U`。通常，Java编译器可以推断出泛型方法调用的类型参数。因此，大多数情况下，你可以不必指定他们。例如，调用泛型函数`addBox`，你可以使用*类型见证*指定类型参数，如下所示:

```java
BoxDemo.<Integer>addBox(Integer.valueOf(10), listOfIntegerBoxes);
```

Alternatively, if you omit the type witness,a Java compiler automatically infers (from the method's arguments) that the type parameter is `Integer`:  
或者，如果省略*类型见证*，则Java编译器会自动（根据方法的参数）推断出类型参数为`Integer`：

```java
BoxDemo.addBox(Integer.valueOf(20), listOfIntegerBoxes);
```

## Type Inference and Instantiation of Generic Classes 泛型类的类型推断和实例化

You can replace the type arguments required to invoke the constructor of a generic class with an empty set of type parameters (`<>`) as long as the compiler can infer the type arguments from the context. This pair of angle brackets is informally called [the diamond](https://docs.oracle.com/javase/tutorial/java/generics/types.html#diamond).  
你可以通过空的类型参数（`<>`）替换调用泛型类中的构造函数所需的类型参数，只要编译器可以通过上下文推导出类型参数。这一对尖括号被非正式地称为[菱形符号](Generic%20Types%20泛型类型.md#the-diamond-菱形符号).

For example, consider the following variable declaration:  
例如，考虑以下变量声明：

```java
Map<String, List<String>> myMap = new HashMap<String, List<String>>();
```

You can substitute the parameterized type of the constructor with an empty set of type parameters (`<>`):  
你可以用一组空的类型参数（<>）代替构造函数的参数化类型：

```java
Map<String, List<String>> myMap = new HashMap<>();
```

Note that to take advantage of type inference during generic class instantiation, you must use the diamond. In the following example, the compiler generates an unchecked conversion warning because the `HashMap()` constructor refers to the `HashMap` raw type, not the `Map<String, List<String>>` type:  
请注意，要在泛型类实例化过程中利用类型推断的优势，必须使用菱形符号。下面的例子中，
编译器会生成未经检查的转换警告，因为`HashMap()`构造函数引用的是`HashMap`原始类型，而不是`Map<String, List<String>>`类型：
```java
Map<String, List<String>> myMap = new HashMap(); // unchecked conversion warning
```

## Type Inference and Generic Constructors of Generic and Non-Generic Classes 泛型和非泛型类的类型推断和泛型构造函数

Note that constructors can be generic (in other words, declare their own formal type parameters) in both generic and non-generic classes. Consider the following example:  
请注意，构造函数可以是泛型的（换句话说，声明自己的形式类型参数）无论在泛型和非泛型类中。 考虑以下示例：
```java
class MyClass<X> {
  <T> MyClass(T t) {
    // ...
  }
}
```

Consider the following instantiation of the class `MyClass`:  
考虑下面的类`MyClass`的实例化：
```java
new MyClass<Integer>("")
```

This statement creates an instance of the parameterized type `MyClass`; the statement explicitly specifies the type `Integer` for the formal type parameter, `X`, of the generic class `MyClass`. Note that the constructor for this generic class contains a formal type parameter, `T`. The compiler infers the type `String` for the formal type parameter, `T`, of the constructor of this generic class (because the actual parameter of this constructor is a `String` object).  
该语句创建了参数化类型`MyClass`的实例；该语句为泛型类`MyClass`的形式类型参数`X`明确指定类型`Integer`。注意，这个泛型类的构造函数包含了形式类型参数`T`。编译器为该泛型类的构造函数的形式类型参数`T`推断出类型为`String`（因为该构造函数的实际参数是`String`对象）。


Compilers from releases prior to Java SE 7 are able to infer the actual type parameters of generic constructors, similar to generic methods. However, compilers in Java SE 7 and later can infer the actual type parameters of the generic class being instantiated if you use the diamond (`<>`). Consider the following example:  
Java SE 7之前的发行版中的编译器能够推断泛型构造函数的实际类型参数，类似于泛型方法。然而，如果使用菱形符号（`<>`），则Java SE 7和更高版本中的编译器可以推断要实例化的泛型类的实际类型参数。考虑下面的例子:

```java
MyClass<Integer> myObject = new MyClass<>("");
```

In this example, the compiler infers the type `Integer` for the formal type parameter, `X`, of the generic class `MyClass`. It infers the type `String` for the formal type parameter, `T`, of the constructor of this generic class.  
在这个例子中，编译器推断出泛型类`MyClass`的形式类型参数`X`的类型为`Integer`。它推断出泛型类的构造函数中的形式参数`T`的类型为`String`。

------

**Note:** It is important to note that the inference algorithm uses only invocation arguments, target types, and possibly an obvious expected return type to infer types. The inference algorithm does not use results from later in the program.  
**注意：** 重要的是要注意，推断算法仅使用调用参数，目标类型，并且可能使用明显的预期返回类型去推断类型。推断算法不使用程序后面的结果。

------

## Target Types 目标类型

The Java compiler takes advantage of target typing to infer the type parameters of a generic method invocation. The *target type* of an expression is the data type that the Java compiler expects depending on where the expression appears. Consider the method `Collections.emptyList`, which is declared as follows:  
Java编译器利用目标类型推断泛型方法调用的类型参数。表达式的*目标类型*是Java编译器期望的数据类型，具体取决于表达式出现的位置。考虑方法`Collections.emptyList`，该方法声明如下：
```java
static <T> List<T> emptyList();
```

Consider the following assignment statement:  
考虑以下赋值语句：
```java
List<String> listOne = Collections.emptyList();
```
This statement is expecting an instance of `List`; this data type is the target type. Because the method `emptyList` returns a value of type `List`, the compiler infers that the type argument `T` must be the value `String`. This works in both Java SE 7 and 8. Alternatively, you could use a type witness and specify the value of `T` as follows:  
该语句需要`List`的实例； 此数据类型是目标类型。因为方法`emptyList`返回一个类型为`List`的值，所以编译器推断类型实参`T`必须为值`String`。这适用于Java SE 7和8。
另外，你可以使用“类型见证”并按如下方式指定`T`的值：
```java
List<String> listOne = Collections.<String>emptyList();
```

However, this is not necessary in this context. It was necessary in other contexts, though. Consider the following method:  
但是，在这种情况下，这不是必需的。 不过，在其他情况下这是必要的。 请考虑以下方法：

```java
void processStringList(List<String> stringList) {
    // process stringList
}
```

Suppose you want to invoke the method `processStringList` with an empty list. In Java SE 7, the following statement does not compile:  
假设你要使用一个空列表调用方法`processStringList`。 在Java SE 7中，以下语句不会编译：

```java
processStringList(Collections.emptyList());
```

The Java SE 7 compiler generates an error message similar to the following:  
Java SE 7编译器生成类似于以下内容的错误消息：

```java
List<Object> cannot be converted to List<String>
```

The compiler requires a value for the type argument `T` so it starts with the value `Object`. Consequently, the invocation of `Collections.emptyList` returns a value of type `List`, which is incompatible with the method `processStringList`. Thus, in Java SE 7, you must specify the value of the value of the type argument as follows:  
编译器需要一个值类用作型参数`T`，因此它以值`Object`开始。 所以，调用`Collections.emptyList`返回一个类型为`List`的值，该值与方法`processStringList`不兼容。 因此，在Java SE 7中，必须指定类型参数的值，如下所示：

```java
processStringList(Collections.<String>emptyList());
```

This is no longer necessary in Java SE 8. The notion of what is a target type has been expanded to include method arguments, such as the argument to the method `processStringList`. In this case, `processStringList` requires an argument of type `List`. The method `Collections.emptyList` returns a value of `List`, so using the target type of `List`, the compiler infers that the type argument `T` has a value of `String`. Thus, in Java SE 8, the following statement compiles:  
在Java SE 8中，这不再是必需的。什么是目标类型的概念已扩展为包括方法参数，例如`processStringList`方法的参数。 在这种情况下，`processStringList`需要一个类型为`List`的参数。 方法`Collections.emptyList`返回一个`List`的值，因此使用目标类型`List`，编译器推断类型参数`T`的值为`String`。 因此，在Java SE 8中，以下语句可以编译：
```java
processStringList(Collections.emptyList());
```

See [Target Typing](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html#target-typing) in [Lambda Expressions](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html) for more information.  
有关更多信息，请参见[Lambda表达式](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)中的[目标类型](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html#target-typing) 。