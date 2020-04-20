# Raw Types 原始类型

A *raw type* is the name of a generic class or interface without any type arguments. For example, given the generic `Box` class:  
*原始类型*是没有任何类型参数的泛型类或接口的名称。 例如，给定泛型`Box`类：

```java
public class Box<T> {
    public void set(T t) { /* ... */ }
    // ...
}
```

To create a parameterized type of `Box<T>`, you supply an actual type argument for the formal type parameter `T`:  
要创建`Box<T>`的参数化类型，你为形式类型参数`T`提供一个实际的类型参数：

```java
Box<Integer> intBox = new Box<>();
```

If the actual type argument is omitted, you create a raw type of `Box<T>`:  
如果实际类型参数省略，那么就为`Box<T>`创建了一个原始类型：

```java
Box rawBox = new Box();
```

Therefore, `Box` is the raw type of the generic type `Box<T>`. However, a non-generic class or interface type is *not* a raw type.  
因此，`Box`是泛型类型`Box<T>`的原始类型。但是，非泛型类或接口类型*不是*原始类型。

Raw types show up in legacy code because lots of API classes (such as the `Collections` classes) were not generic prior to JDK 5.0. When using raw types, you essentially get pre-generics behavior — a `Box` gives you `Object`s. For backward compatibility, assigning a parameterized type to its raw type is allowed:  
原始类型出现在旧版代码中，因为在JDK 5.0之前，许多API类（例如`Collections`类）不是泛型的。当你使用原始类型，你本质上获得了前泛型行为 ——`Box`中提供`Object`。为了向后兼容，允许将参数化类型分配给其原始类型：

```java
Box<String> stringBox = new Box<>();
Box rawBox = stringBox;               // OK
```

But if you assign a raw type to a parameterized type, you get a warning:  
但是如果你把原始类型分配给参数化类型，你会得到一个警告：

```java
Box rawBox = new Box();           // rawBox is a raw type of Box<T>
Box<Integer> intBox = rawBox;     // warning: unchecked conversion
```

You also get a warning if you use a raw type to invoke generic methods defined in the corresponding generic type:  
如果你使用原始类型来调用在相应的泛型类型中定义的泛型方法，也会收到警告：

```java
Box<String> stringBox = new Box<>();
Box rawBox = stringBox;
rawBox.set(8);  // warning: unchecked invocation to set(T)
```

The warning shows that raw types bypass generic type checks, deferring the catch of unsafe code to runtime. Therefore, you should avoid using raw types.  
该警告表明原始类型会绕过泛型类型检查，从而将不安全代码的捕获推迟到运行时。 因此，应避免使用原始类型。

The [Type Erasure](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html) section has more information on how the Java compiler uses raw types.  
[类型擦除](Type%20Erasure%20类型擦除.md) 部分提供了有关Java编译器如何使用原始类型的更多信息。

## Unchecked Error Messages 未检查的错误消息

As mentioned previously, when mixing legacy code with generic code, you may encounter warning messages similar to the following:  
如前所述，将旧代码与泛型代码混合时，你可能会遇到类似于以下内容的警告消息：

```java
Note: Example.java uses unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.
```

This can happen when using an older API that operates on raw types, as shown in the following example:  
当对原始类型使用较旧的API时，可能会发生这种情况，如下例所示：

```java
public class WarningDemo {
    public static void main(String[] args){
        Box<Integer> bi;
        bi = createBox();
    }

    static Box createBox(){
        return new Box();
    }
}
```

The term "unchecked" means that the compiler does not have enough type information to perform all type checks necessary to ensure type safety. The "unchecked" warning is disabled, by default, though the compiler gives a hint. To see all "unchecked" warnings, recompile with `-Xlint:unchecked`.  
术语“unchecked”表示编译器没有足够的类型信息去执行所有必要的类型检测以确保类型安全。默认情况下，“unchecked”警告是关闭的，尽管编译器会给出提示。要查看所有“unchecked”的警告，请使用`-Xlint：unchecked`重新编译。

Recompiling the previous example with `-Xlint:unchecked` reveals the following additional information:  
使用`-Xlint:unchecked`重新编译前面的示例，会显示以下附加信息:

```java
WarningDemo.java:4: warning: [unchecked] unchecked conversion
found   : Box
required: Box<java.lang.Integer>
        bi = createBox();
                      ^
1 warning
```

To completely disable unchecked warnings, use the `-Xlint:-unchecked` flag. The `@SuppressWarnings("unchecked")` annotation suppresses unchecked warnings. If you are unfamiliar with the `@SuppressWarnings` syntax, see [Annotations](https://docs.oracle.com/javase/tutorial/java/annotations/index.html).  
要完全关闭未选中的警告，请使用`-Xlint:-unchecked`标志。`@SuppressWarnings("unchecked")`注释会取消未经检查的警告。如果你不熟悉`@SuppressWarnings`语法，请参阅[注解](https://docs.oracle.com/javase/tutorial/java/annotations/index.html)。