# Restrictions on Generics 泛型的限制

To use Java generics effectively, you must consider the following restrictions:
为了有效地使用Java泛型，必须考虑以下限制：

- [Cannot Instantiate Generic Types with Primitive Types 无法实例化具有基本类型的泛型类型](#cannot-instantiate-generic-types-with-primitive-types-无法实例化具有基本类型的泛型类型e)
- [Cannot Create Instances of Type Parameters 不能创建类型参数实例](#cannot-create-instances-of-type-parameters-不能创建类型参数实例)
- [Cannot Declare Static Fields Whose Types are Type Parameters 静态字段的类型不能声明为类型参数](#cannot-declare-static-fields-whose-types-are-type-parameters-静态字段的类型不能声明为类型参数)
- [Cannot Use Casts or `instanceof` With Parameterized Types 不能对参数化类型使用强制转换或`instanceof`](#cannot-use-casts-or-instanceof-with-parameterized-types-不能对参数化类型使用casts或instanceof)
- [Cannot Create Arrays of Parameterized Types 无法创建参数化类型的数组](#cannot-create-arrays-of-parameterized-types-无法创建参数化类型的数组)
- [Cannot Create, Catch, or Throw Objects of Parameterized Types 无法创建，捕获或引发参数化类型的对象](#cannot-create-catch-or-throw-objects-of-parameterized-types-无法创建捕获或引发参数化类型的对象)
- [Cannot Overload a Method Where the Formal Parameter Types of Each Overload Erase to the Same Raw Type 在每个重载的形式参数类型都擦除为相同原始类型的方法的情况下，无法重载这个方法](#cannot-overload-a-method-where-the-formal-parameter-types-of-each-overload-erase-to-the-same-raw-type-在每个重载的形式参数类型都擦除为相同原始类型的方法的情况下无法重载这个方法)

## Cannot Instantiate Generic Types with Primitive Types 无法实例化具有基本类型的泛型类型

Consider the following parameterized type:  
考虑下面参数类型：
```java
class Pair<K, V> {

    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    // ...
}
```

When creating a `Pair` object, you cannot substitute a primitive type for the type parameter `K` or `V`:  
当创建一个`Pair`对象，你不能用基本类型替代类型参数`K`或者`V`：

```
Pair<int, char> p = new Pair<>(8, 'a');  // compile-time error
```

You can substitute only non-primitive types for the type parameters `K` and `V`:  
你只能用非基本类型替换参数类型`K` 和 `V`：
```
Pair<Integer, Character> p = new Pair<>(8, 'a');
```

Note that the Java compiler autoboxes `8` to `Integer.valueOf(8)` and '`a`' to `Character('a')`:  
请注意，Java编译器自动将`8`装箱为`Integer.valueOf(8)`，将'`a`'装箱为`Character('a')`：

```java
Pair<Integer, Character> p = new Pair<>(Integer.valueOf(8), new Character('a'));
```

For more information on autoboxing, see [Autoboxing and Unboxing](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html) in the [Numbers and Strings](https://docs.oracle.com/javase/tutorial/java/data/index.html) lesson.  
更多关于自动装箱的信息查看[Numbers and Strings](https://docs.oracle.com/javase/tutorial/java/data/index.html) 课程中的[Autoboxing and Unboxing](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html)

## Cannot Create Instances of Type Parameters 不能创建类型参数实例

You cannot create an instance of a type parameter. For example, the following code causes a compile-time error:  
你不能创建一个类型参数实例。例如，下面代码会导致编译时错误：

```java
public static <E> void append(List<E> list) {
    E elem = new E();  // compile-time error
    list.add(elem);
}
```

As a workaround, you can create an object of a type parameter through reflection:  
解决方法是，可以通过反射创建类型参数的对象：

```java
public static <E> void append(List<E> list, Class<E> cls) throws Exception {
    E elem = cls.newInstance();   // OK
    list.add(elem);
}
```

You can invoke the `append` method as follows:  
你可以按以下方式调用`append`方法：

```java
List<String> ls = new ArrayList<>();
append(ls, String.class);
```

## Cannot Declare Static Fields Whose Types are Type Parameters 静态字段的类型不能声明为类型参数

A class's static field is a class-level variable shared by all non-static objects of the class. Hence, static fields of type parameters are not allowed. Consider the following class:  
类的静态字段是该类的所有非静态对象共享的类级别变量。因此，不允许使用类型参数的静态字段。 考虑下面的类：

```java
public class MobileDevice<T> {
    private static T os;

    // ...
}
```

If static fields of type parameters were allowed, then the following code would be confused:  
如果允许使用类型参数的静态字段，那么以下代码将被混淆：

```java
MobileDevice<Smartphone> phone = new MobileDevice<>();
MobileDevice<Pager> pager = new MobileDevice<>();
MobileDevice<TabletPC> pc = new MobileDevice<>();
```

Because the static field `os` is shared by `phone`, `pager`, and `pc`, what is the actual type of `os`? It cannot be `Smartphone`, `Pager`, and `TabletPC` at the same time. You cannot, therefore, create static fields of type parameters.  
因为静态字段`os`由`phone`，`pager`和`pc`共享，所以`os`的实际类型是什么？ 它不能同时是`Smartphone`，`Pager`和`TabletPC`。 因此，你无法创建类型参数的静态字段。

## Cannot Use Casts or `instanceof` with Parameterized Types 不能对参数化类型使用Casts或`instanceof`

Because the Java compiler erases all type parameters in generic code, you cannot verify which parameterized type for a generic type is being used at runtime:  
因为Java编译器会擦除通用代码中的所有类型参数，所以你无法验证在运行时使用的是泛型类型的参数化类型：

```java
public static <E> void rtti(List<E> list) {
    if (list instanceof ArrayList<Integer>) {  // compile-time error
        // ...
    }
}
```

The set of parameterized types passed to the `rtti` method is:  
传递给`rtti`方法的参数化类型集合为:

```
S = { ArrayList<Integer>, ArrayList<String> LinkedList<Character>, ... }
```

The runtime does not keep track of type parameters, so it cannot tell the difference between an `ArrayList<Integer>` and an `ArrayList<String>`. The most you can do is to use an unbounded wildcard to verify that the list is an `ArrayList`:  
运行时不跟踪类型参数，因此无法区分`ArrayList<Integer>`和`ArrayList<String>`之间的区别。 你最多可以做的是使用无限制的通配符来验证列表是否为`ArrayList`：

```java
public static void rtti(List<?> list) {
    if (list instanceof ArrayList<?>) {  // OK; instanceof requires a reifiable type
        // ...
    }
}
```

Typically, you cannot cast to a parameterized type unless it is parameterized by unbounded wildcards. For example:  
通常，除非使用不受限制的通配符对其进行参数化，否则无法将其转换为参数化类型。 例如：

```java
List<Integer> li = new ArrayList<>();
List<Number>  ln = (List<Number>) li;  // compile-time error
```

However, in some cases the compiler knows that a type parameter is always valid and allows the cast. For example:  
但是，在某些情况下，编译器知道类型参数始终有效并允许强制转换。 例如：

```java
List<String> l1 = ...;
ArrayList<String> l2 = (ArrayList<String>)l1;  // OK
```

## Cannot Create Arrays of Parameterized Types 无法创建参数化类型的数组

You cannot create arrays of parameterized types. For example, the following code does not compile:  
你不能创建参数化类型的数组。 例如，以下代码无法编译：

```java
List<Integer>[] arrayOfLists = new List<Integer>[2];  // compile-time error
```

The following code illustrates what happens when different types are inserted into an array:  
以下代码说明了将不同类型插入到数组中时发生的情况：

```java
Object[] strings = new String[2];
strings[0] = "hi";   // OK
strings[1] = 100;    // An ArrayStoreException is thrown.
```

If you try the same thing with a generic list, there would be a problem:  
如果你对泛型列表尝试相同的操作，则会出现问题：

```java
Object[] stringLists = new List<String>[];  // compiler error, but pretend it's allowed
stringLists[0] = new ArrayList<String>();   // OK
stringLists[1] = new ArrayList<Integer>();  // An ArrayStoreException should be thrown,
                                            // but the runtime can't detect it.
```

If arrays of parameterized lists were allowed, the previous code would fail to throw the desired `ArrayStoreException`.  
如果允许使用参数化列表的数组，则先前的代码将无法引发所需的`ArrayStoreException`。

## Cannot Create, Catch, or Throw Objects of Parameterized Types 无法创建，捕获或引发参数化类型的对象

A generic class cannot extend the `Throwable` class directly or indirectly. For example, the following classes will not compile:  
泛型类不能直接或间接扩展`Throwable`类。 例如，以下类将无法编译：

```java
// Extends Throwable indirectly
class MathException<T> extends Exception { /* ... */ }    // compile-time error

// Extends Throwable directly
class QueueFullException<T> extends Throwable { /* ... */ // compile-time error
```

A method cannot catch an instance of a type parameter:  
方法无法捕获类型参数的实例：

```java
public static <T extends Exception, J> void execute(List<J> jobs) {
    try {
        for (J job : jobs)
            // ...
    } catch (T e) {   // compile-time error
        // ...
    }
}
```

You can, however, use a type parameter in a `throws` clause:  
但是，你可以在`throws`子句中使用类型参数：

```java
class Parser<T extends Exception> {
    public void parse(File file) throws T {     // OK
        // ...
    }
}
```

## Cannot Overload a Method Where the Formal Parameter Types of Each Overload Erase to the Same Raw Type 在每个重载的形式参数类型都擦除为相同原始类型的方法的情况下，无法重载这个方法

A class cannot have two overloaded methods that will have the same signature after type erasure.  
一个类不能有两个重载的方法，这些方法在类型擦除后将具有相同的签名。

```java
public class Example {
    public void print(Set<String> strSet) { }
    public void print(Set<Integer> intSet) { }
}
```

The overloads would all share the same classfile representation and will generate a compile-time error.  
重载将共享相同的类文件表示，并且将生成编译时错误。