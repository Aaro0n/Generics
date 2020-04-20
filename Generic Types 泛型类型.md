# Generic Types 泛型类型

A *generic type* is a generic class or interface that is parameterized over types. The following `Box` class will be modified to demonstrate the concept.  
*泛型类型*是被参数化为类型的泛型类或接口。下面的`Box`类将被修改以演示这个概念。

## A Simple Box Class 一个简单的Box类

Begin by examining a non-generic `Box` class that operates on objects of any type. It needs only to provide two methods: `set`, which adds an object to the box, and `get`, which retrieves it:  
首先检查一个可以操作任何类型的非泛型`Box`类。它只提供了两种方法：`set`，可以向box添加一个对象，和`get`，可以获取这个添加的对象
```java
public class Box {
    private Object object;

    public void set(Object object) { this.object = object; }
    public Object get() { return object; }
}
```

Since its methods accept or return an `Object`, you are free to pass in whatever you want, provided that it is not one of the primitive types. There is no way to verify, at compile time, how the class is used. One part of the code may place an `Integer` in the box and expect to get `Integer`s out of it, while another part of the code may mistakenly pass in a `String`, resulting in a runtime error.  
由于其方法接受或返回`Object`，因此只要它不是基本类型之一，你就可以随意传递所需的任何内容。在编译时期无法去验证这个类的使用方式。代码的一部分可能会将`Integer`放在框中，并期望从中取出`Integer`，而代码的另一部分可能会错误地传入`String`，从而导致运行时错误。


## A Generic Version of the Box Class Box类的泛型版本

A *generic class* is defined with the following format:  
*泛型类*的定义方式如下：
```java
class name<T1, T2, ..., Tn> { /* ... */ }
```

The type parameter section, delimited by angle brackets (`<>`), follows the class name. It specifies the *type parameters* (also called *type variables*) `T1`, `T2`, ..., and `Tn`.  
类型参数是在类名后面被尖括号（`<>`）包围的部分。它指定类型参数(也称为*类型变量*)`T1`、`T2`、…和`Tn`。

To update the `Box` class to use generics, you create a *generic type declaration* by changing the code "`public class Box`" to "`public class Box<T>`". This introduces the type variable, `T`, that can be used anywhere inside the class.  
为了把`Box`类升级成为泛型，通过把代码`public class Box`改为`public class Box<T>`就可以创建一个*泛型类型*声明。这引入了类型变量`T`，它可以在类的任何地方使用。

With this change, the `Box` class becomes:  
有了这个改变，`Box`类变成这样：

```java
/**
 * Generic version of the Box class.
 * @param <T> the type of the value being boxed
 */
public class Box<T> {
    // T stands for "Type"
    private T t;

    public void set(T t) { this.t = t; }
    public T get() { return t; }
}
```

As you can see, all occurrences of `Object` are replaced by `T`. A type variable can be any **non-primitive** type you specify: any class type, any interface type, any array type, or even another type variable.  
可以看到，所有出现的`Object`都被`T`替换了。类型变量可以是你指定的任何**非基本**类型:任何类类型、任何接口类型、任何数组类型，甚至是另一个类型变量。

This same technique can be applied to create generic interfaces.  
同样的技术也可以用于创建泛型接口。

## Type Parameter Naming Conventions 类型参数命名约定

By convention, type parameter names are single, uppercase letters. This stands in sharp contrast to the variable [naming](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/variables.html#naming) conventions that you already know about, and with good reason: Without this convention, it would be difficult to tell the difference between a type variable and an ordinary class or interface name.  
按照约定，类型参数名称是单个大写字母。这与你已经知道的变量[命名](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/variables.html#naming)约定形成鲜明对比，并且有充分的理由：没有该约定，将很难分辨类型变量与普通类或接口名称之间的区别。

The most commonly used type parameter names are:  
常用的类型参数名称是：

- E - Element (used extensively by the Java Collections Framework)
- K - Key
- N - Number
- T - Type
- V - Value
- S,U,V etc. - 2nd, 3rd, 4th types

You'll see these names used throughout the Java SE API and the rest of this lesson.  
你将看到在Java SE API以及本课程其余部分中使用的这些名称。

## Invoking and Instantiating a Generic Type 调用和实例化泛型类型

To reference the generic `Box` class from within your code, you must perform a *generic type invocation*, which replaces `T` with some concrete value, such as `Integer`:  
要从代码中引用泛型`Box`类，必须执行*泛型类型调用*，该调用将`T`替换为某些具体值，例如`Integer`：

```java
Box<Integer> integerBox;
```

You can think of a generic type invocation as being similar to an ordinary method invocation, but instead of passing an argument to a method, you are passing a *type argument* — `Integer` in this case — to the `Box` class itself.  
你可以认为泛型类型调用类似于普通方法调用，但是不是把参数传递给方法，而是将*类型参数*（这个例子是`Integer`）传递给`Box`类本身。

------

**Type Parameter and Type Argument Terminology:** Many developers use the terms "type parameter" and "type argument" interchangeably, but these terms are not the same. When coding, one provides type arguments in order to create a parameterized type. Therefore, the `T` in `Foo<T>` is a type parameter and the `String` in `Foo<String> f` is a type argument. This lesson observes this definition when using these terms.  
**类型形参与类型实参术语**：很多开发者互换的使用术语“类型形参”和“类型实参”，但是这些术语是不相同的。编码时，提供类型实参以创建参数化类型。因此，`Foo<T>`中的`T`是类型形参，而`Foo<String> f`中的`String`是类型实参。在使用这些术语时，本课将遵循此定义。

------

Like any other variable declaration, this code does not actually create a new `Box` object. It simply declares that `integerBox` will hold a reference to a "`Box` of `Integer`", which is how `Box<Integer>` is read.  
像任何其它变量声明一样，此代码实际上不会创建新的`Box`对象。它只是声明`integerBox`将保存对“`Box`of`Integer`”的引用，这就是读`Box<Integer>`的方式。

An invocation of a generic type is generally known as a *parameterized type*.  
泛型类型的调用通常称为*参数化类型*。

To instantiate this class, use the `new` keyword, as usual, but place `<Integer>` between the class name and the parenthesis:  
要实例化此类，请像往常一样使用`new`关键字，但将`<Integer>`放在类名和括号之间：

```java
Box<Integer> integerBox = new Box<Integer>();
```

## The Diamond 菱形符号

In Java SE 7 and later, you can replace the type arguments required to invoke the constructor of a generic class with an empty set of type arguments (<>) as long as the compiler can determine, or infer, the type arguments from the context. This pair of angle brackets, <>, is informally called *the diamond*. For example, you can create an instance of `Box<Integer>` with the following statement:  
在Java SE 7和更高版本中，只要编译器可以从上下文确定或推断出类型实参，就可以用一组空的类型实参（<>）替换调用泛型类的构造函数所需的类型实参。这对尖括号，<>，被非正式地称为菱形符号。例如，你可以使用以下语句创建`Box<Integer>`的实例：

```java
Box<Integer> integerBox = new Box<>();
```

For more information on diamond notation and type inference, see [Type Inference](https://docs.oracle.com/javase/tutorial/java/generics/genTypeInference.html).  
有关菱形符号和类型推断的更多信息，请参见[类型推断](./Type%20Inference%20类型推断.md)。

## Multiple Type Parameters 多种类型形参

As mentioned previously, a generic class can have multiple type parameters. For example, the generic `OrderedPair` class, which implements the generic `Pair` interface:  
如前所述，泛型类可以具有多个类型形参。例如，泛型类`OrderedPair`实现了泛型接口`Pair`：

```java
public interface Pair<K, V> {
    public K getKey();
    public V getValue();
}

public class OrderedPair<K, V> implements Pair<K, V> {

    private K key;
    private V value;

    public OrderedPair(K key, V value) {
	this.key = key;
	this.value = value;
    }

    public K getKey()	{ return key; }
    public V getValue() { return value; }
}
```

The following statements create two instantiations of the `OrderedPair` class:  
以下语句创建`OrderedPair`类的两个实例：

```java
Pair<String, Integer> p1 = new OrderedPair<String, Integer>("Even", 8);
Pair<String, String>  p2 = new OrderedPair<String, String>("hello", "world");
```

The code, `new OrderedPair<String,Integer>`, instantiates `K` as a `String` and `V` as an `Integer`. Therefore, the parameter types of `OrderedPair`'s constructor are `String` and `Integer`, respectively. Due to [autoboxing](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html), it is valid to pass a `String` and an `int` to the class.  
代码`new OrderedPair<String,Integer>`将`K`实例化为`String`，将`V`实例化为`Integer`。 因此，`OrderedPair`的构造函数的参数类型分别为`String`和`Integer`。 由于[自动装箱](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html)，将`String`和`int`传递给类是有效的。

As mentioned in [The Diamond](https://docs.oracle.com/javase/tutorial/java/generics/types.html#diamond), because a Java compiler can infer the `K` and `V` types from the declaration `OrderedPair<String,Integer>`, these statements can be shortened using diamond notation:  
如[The Diamond](#the-diamond-菱形符号)所述，由于Java编译器可以从声明`OrderedPair<String,Integer>`推断出`K`和`V`类型，因此可以使用菱形表示法来缩短这些语句：

```java
OrderedPair<String, Integer> p1 = new OrderedPair<>("Even", 8);
OrderedPair<String, String>  p2 = new OrderedPair<>("hello", "world");
```

To create a generic interface, follow the same conventions as for creating a generic class.  
要创建泛型接口，请遵循与创建泛型类相同的约定。

## Parameterized Types 参数化类型

You can also substitute a type parameter (i.e., `K` or `V`) with a parameterized type (i.e., `List<String>`). For example, using the `OrderedPair<K，V>` example:  
你还可以用参数化类型（即`List<String>`）代替类型参数（即`K`或`V`）。例如，使用`OrderedPair<K，V>`示例：

```java
OrderedPair<String, Box<Integer>> p = new OrderedPair<>("primes", new Box<Integer>(...));
```