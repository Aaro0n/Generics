# Bounded Type Parameters 有界类型参数

There may be times when you want to restrict the types that can be used as type arguments in a parameterized type. For example, a method that operates on numbers might only want to accept instances of `Number` or its subclasses. This is what *bounded type parameters* are for.  
有时你可能想限制可以在参数化类型中用作类型参数的类型。例如，对数字进行操作的方法可能只希望接受`Number`或其子类的实例。这就是*有界类型参数*的用途。

To declare a bounded type parameter, list the type parameter's name, followed by the `extends` keyword, followed by its *upper bound*, which in this example is `Number`. Note that, in this context, `extends` is used in a general sense to mean either "extends" (as in classes) or "implements" (as in interfaces).  
要声明一个有界的类型参数，先列出该类型参数的名称，然后是`extends`关键字，然后是其*上限*（在本示例中为`Number`）。请注意，在这种情况下，`extends`通常用于表示“extends”（如在类中）或“implements”（如在接口中）。

```java
public class Box<T> {

    private T t;          

    public void set(T t) {
        this.t = t;
    }

    public T get() {
        return t;
    }

    public <U extends Number> void inspect(U u){
        System.out.println("T: " + t.getClass().getName());
        System.out.println("U: " + u.getClass().getName());
    }

    public static void main(String[] args) {
        Box<Integer> integerBox = new Box<Integer>();
        integerBox.set(new Integer(10));
        integerBox.inspect("some text"); // error: this is still String!
    }
}
```

By modifying our generic method to include this bounded type parameter, compilation will now fail, since our invocation of `inspect` still includes a `String`:  
通过修改泛型方法以包含此有界类型参数，由于我们的`inspect`调用仍包含`String`，因此编译现在将失败：

```java
Box.java:21: <U>inspect(U) in Box<java.lang.Integer> cannot
  be applied to (java.lang.String)
                        integerBox.inspect("10");
                                  ^
1 error
```

In addition to limiting the types you can use to instantiate a generic type, bounded type parameters allow you to invoke methods defined in the bounds:  
除了限制可用于实例化泛型类型的类型之外，有界类型参数还允许你调用在边界中定义的方法：

```java
public class NaturalNumber<T extends Integer> {

    private T n;

    public NaturalNumber(T n)  { this.n = n; }

    public boolean isEven() {
        return n.intValue() % 2 == 0;
    }

    // ...
}
```

The `isEven` method invokes the `intValue` method defined in the `Integer` class through `n`.  
`isEven`方法通过`n`调用`Integer`类中定义的`intValue`方法。

## Multiple Bounds 多个界限

The preceding example illustrates the use of a type parameter with a single bound, but a type parameter can have *multiple bounds*:  
前面的示例说明了使用带单个界限的类型参数，但是一个类型参数可以具有*多个界限*：

```java
<T extends B1 & B2 & B3>
```

A type variable with multiple bounds is a subtype of all the types listed in the bound. If one of the bounds is a class, it must be specified first. For example:  
具有多个界限的类型变量是该界限中列出的所有类型的子类型。如果界限之一是类，则必须首先指定它。 例如：

```java
Class A { /* ... */ }
interface B { /* ... */ }
interface C { /* ... */ }

class D <T extends A & B & C> { /* ... */ }
```

If bound `A` is not specified first, you get a compile-time error:  
如果界限`A`不是第一个指定，则会出现编译时错误：

```java
class D <T extends B & A & C> { /* ... */ }  // compile-time error
```