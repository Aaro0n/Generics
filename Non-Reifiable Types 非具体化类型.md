# Non-Reifiable Types 非具体化类型

The section [Type Erasure](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html) discusses the process where the compiler removes information related to type parameters and type arguments. Type erasure has consequences related to variable arguments (also known as *varargs* ) methods whose varargs formal parameter has a non-reifiable type. See the section [Arbitrary Number of Arguments](https://docs.oracle.com/javase/tutorial/java/javaOO/arguments.html#varargs) in [Passing Information to a Method or a Constructor](https://docs.oracle.com/javase/tutorial/java/javaOO/arguments.html) for more information about varargs methods.  
[类型擦除](Type%20Erasure%20类型擦除.md) 部分讨论了编译器删除与类型参数和类型参数有关的信息的过程。类型擦除的结果与可变参数（也称为*varargs*）方法有关，这些方法的可变形式参数具有不可修改的类型。有关可变参数方法的更多信息请看[Passing Information to a Method or a Constructor](https://docs.oracle.com/javase/tutorial/java/javaOO/arguments.html) 中的 [Arbitrary Number of Arguments](https://docs.oracle.com/javase/tutorial/java/javaOO/arguments.html#varargs) 章节。

This page covers the following topics:  
此页面涵盖以下主题：

- [Non-Reifiable Types 非具体化类型](#non-reifiable-types-非具体化类型)
- [Heap Pollution 堆污染](#heap-pollution-堆污染)
- [Potential Vulnerabilities of Varargs Methods with Non-Reifiable Formal Parameters 具有非具体化形参的可变参数方法的潜在漏洞](#potential-vulnerabilities-of-varargs-methods-with-non-reifiable-formal-parameters-具有非具体化形参的可变参数方法的潜在漏洞)
- [Preventing Warnings from Varargs Methods with Non-Reifiable Formal Parameters 从具有非具体化形参的可变参数函数中阻止警告](#prevent-warnings-from-varargs-methods-with-non-reifiable-formal-parameters-从具有非具体化形参的可变参数函数中阻止警告)

## Non-Reifiable Types 非具体化类型

A *reifiable* type is a type whose type information is fully available at runtime. This includes primitives, non-generic types, raw types, and invocations of unbound wildcards.  
可具体化类型是其类型信息在运行时完全可用的类型。这包括基本类型，非泛型类型，原始类型以及无界通配符的调用。

*Non-reifiable types* are types where information has been removed at compile-time by type erasure — invocations of generic types that are not defined as unbounded wildcards. A non-reifiable type does not have all of its information available at runtime. Examples of non-reifiable types are `List<String>` and `List<Number>`; the JVM cannot tell the difference between these types at runtime. As shown in [Restrictions on Generics](https://docs.oracle.com/javase/tutorial/java/generics/restrictions.html), there are certain situations where non-reifiable types cannot be used: in an `instanceof` expression, for example, or as an element in an array.  
*非具体化类型*是在编译时期被类型擦除移除信息的类型——未定义为无界通配符的泛型类型调用。*非具体化类型*在运行时并不具有所有可用信息。非具体化类型的例子是`List<String>` 和 `List<Number>`，JVM在运行时无法分辨出这些类型之间的区别。就像[泛型的限制](Restrictions%20on%20Generics%20泛型的限制.md)展示的一样，在某些情况下，不能使用*非具体化类型*：例如在表达式的`instanceof`中，或作为数组中的元素。


## Heap Pollution 堆污染

*Heap pollution* occurs when a variable of a parameterized type refers to an object that is not of that parameterized type. This situation occurs if the program performed some operation that gives rise to an unchecked warning at compile-time. An *unchecked warning* is generated if, either at compile-time (within the limits of the compile-time type checking rules) or at runtime, the correctness of an operation involving a parameterized type (for example, a cast or method call) cannot be verified. For example, heap pollution occurs when mixing raw types and parameterized types, or when performing unchecked casts.  
当参数化类型的变量引用的对象不是该参数化类型的对象时，就会发生*堆污染*。如果程序执行了一些在编译时期产生的未检测警告的操作就会发生这种情况。无论在编译时期（在编译时类型检查规则的范围内）还是运行时期，涉及参数化类型的操作（例如，强制转换或方法调用）的正确性无法被验证。例如，当混合原始类型和参数化类型时，或者执行未经检查的强制转换时，就会发生堆污染。

In normal situations, when all code is compiled at the same time, the compiler issues an unchecked warning to draw your attention to potential heap pollution. If you compile sections of your code separately, it is difficult to detect the potential risk of heap pollution. If you ensure that your code compiles without warnings, then no heap pollution can occur.  
在正常情况下，当同时编译所有代码时，编译器会发出未经检查的警告，以引起你对潜在堆污染的注意。如果分别编译部分代码，则很难检测到堆污染的潜在风险。如果确保代码在没有警告的情况下进行编译，则不会发生堆污染。

## Potential Vulnerabilities of Varargs Methods with Non-Reifiable Formal Parameters 具有非具体化形参的可变参数方法的潜在漏洞

Generic methods that include vararg input parameters can cause heap pollution.  
包含可变输入参数的泛型函数会导致堆污染。

Consider the following `ArrayBuilder` class:  
考虑下面`ArrayBuilder`类：
```java
public class ArrayBuilder {

  public static <T> void addToList (List<T> listArg, T... elements) {
    for (T x : elements) {
      listArg.add(x);
    }
  }

  public static void faultyMethod(List<String>... l) {
    Object[] objectArray = l;     // Valid
    objectArray[0] = Arrays.asList(42);
    String s = l[0].get(0);       // ClassCastException thrown here
  }

}
```

The following example, `HeapPollutionExample` uses the `ArrayBuiler` class:  
下面的例子，`HeapPollutionExample`使用 `ArrayBuiler`类：
```java
public class HeapPollutionExample {

  public static void main(String[] args) {

    List<String> stringListA = new ArrayList<String>();
    List<String> stringListB = new ArrayList<String>();

    ArrayBuilder.addToList(stringListA, "Seven", "Eight", "Nine");
    ArrayBuilder.addToList(stringListB, "Ten", "Eleven", "Twelve");
    List<List<String>> listOfStringLists =
      new ArrayList<List<String>>();
    ArrayBuilder.addToList(listOfStringLists,
      stringListA, stringListB);

    ArrayBuilder.faultyMethod(Arrays.asList("Hello!"), Arrays.asList("World!"));
  }
}
```

When compiled, the following warning is produced by the definition of the `ArrayBuilder.addToList` method:  
编译后，`ArrayBuilder.addToList`定义的方法产生了如下警告：
```
warning: [varargs] Possible heap pollution from parameterized vararg type T
```

When the compiler encounters a varargs method, it translates the varargs formal parameter into an array. However, the Java programming language does not permit the creation of arrays of parameterized types. In the method `ArrayBuilder.addToList`, the compiler translates the varargs formal parameter `T... elements` to the formal parameter `T[] elements`, an array. However, because of type erasure, the compiler converts the varargs formal parameter to `Object[] elements`. Consequently, there is a possibility of heap pollution.  
当编译器遇到可变参数方法时，它将可变形式参数转换为数组。但是，Java编程语言不允许创建参数化类型的数组。在方法`ArrayBuilder.addToList`中，编译器将可变形式参数`T... elements`转换为形式参数`T[] elements`数组。但是，由于类型擦除，编译器将可变形式参数转换为 `Object[] elements`。因此，存在堆污染的可能性。


The following statement assigns the varargs formal parameter `l` to the `Object` array `objectArray`:  
以下语句将可变形式参数`l`分配给对象数组`objectArray`：

```java
Object[] objectArray = l;
```

This statement can potentially introduce heap pollution. A value that does match the parameterized type of the varargs formal parameter `l` can be assigned to the variable `objectArray`, and thus can be assigned to `l`. However, the compiler does not generate an unchecked warning at this statement. The compiler has already generated a warning when it translated the varargs formal parameter `List... l` to the formal parameter `List[] l`. This statement is valid; the variable `l` has the type `List[]`, which is a subtype of `Object[]`.  
该语句可能会导致堆污染。可以将与可变形式参数`l`的参数化类型匹配的值分配给变量`objectArray`，从而可以将其分配给`l`。但是，编译器不会在此语句上生成未经检查的警告。编译器在将可变形式参数`List... l`转换为形式参数`List[] l`时已产生警告。 此声明是有效的； 变量`l`的类型为`List[]`，这是`Object[]`的子类型。

Consequently, the compiler does not issue a warning or error if you assign a `List` object of any type to any array component of the `objectArray` array as shown by this statement:  
因此，如果将任何类型的`List`对象分配给`objectArray`数组的任何数组组件，则编译器不会发出警告或错误，如以下语句所示：

```java
objectArray[0] = Arrays.asList(42);
```

This statement assigns to the first array component of the `objectArray` array with a `List` object that contains one object of type `Integer`.  
该语句将`List`对象分配给`objectArray`数组的第一个数组组件，该`List`对象包含一个`Integer`类型的对象。

Suppose you invoke `ArrayBuilder.faultyMethod` with the following statement:  
假设你使用以下语句调用`ArrayBuilder.faultyMethod`：

```java
ArrayBuilder.faultyMethod(Arrays.asList("Hello!"), Arrays.asList("World!"));
```

At runtime, the JVM throws a `ClassCastException` at the following statement:  
在运行时，JVM在以下语句中抛出`ClassCastException`：
```java
// ClassCastException thrown here
String s = l[0].get(0);
```

The object stored in the first array component of the variable `l` has the type `List<Integer>`, but this statement is expecting an object of type `List<String>`.  
存储在变量`l`的第一个数组组件中的对象的类型为`List<Integer>`，但是此语句要求的对象类型为`List<String>`。

## Prevent Warnings from Varargs Methods with Non-Reifiable Formal Parameters 从具有非具体化形参的可变参数函数中阻止警告

If you declare a varargs method that has parameters of a parameterized type, and you ensure that the body of the method does not throw a `ClassCastException` or other similar exception due to improper handling of the varargs formal parameter, you can prevent the warning that the compiler generates for these kinds of varargs methods by adding the following annotation to static and non-constructor method declarations:  
如果你声明一个可变参数函数，它具有参数化类型的参数，并且你确定函数体中不会抛出`ClassCastException`，或者其它因为对可变形参处理不当产生的类型异常，你可以阻止这个编译器为这些可变参数函数产生的警告， 通过给静态和非构造函数声明添加下面的注解：
```java
@SafeVarargs
```

The `@SafeVarargs` annotation is a documented part of the method's contract; this annotation asserts that the implementation of the method will not improperly handle the varargs formal parameter.  
`@SafeVarargs`注解是该方法合同的书面部分； 该注释断言该方法的实现不会不适当地处理可变形式参数。

It is also possible, though less desirable, to suppress such warnings by adding the following to the method declaration:  
尽管不太理想，但也可以通过在方法声明中添加以下内容来抑制此类警告：

```java
@SuppressWarnings({"unchecked", "varargs"})
```

However, this approach does not suppress warnings generated from the method's call site. If you are unfamiliar with the `@SuppressWarnings` syntax, see [Annotations](https://docs.oracle.com/javase/tutorial/java/annotations/index.html).  
但是，这种方法不能抑制从该方法的调用站点生成的警告。 如果你不熟悉`@SuppressWarnings`语法，请参阅[注解](https://docs.oracle.com/javase/tutorial/java/annotations/index.html)。