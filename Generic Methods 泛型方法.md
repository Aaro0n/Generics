# Generic Methods 泛型方法

*Generic methods* are methods that introduce their own type parameters. This is similar to declaring a generic type, but the type parameter's scope is limited to the method where it is declared. Static and non-static generic methods are allowed, as well as generic class constructors.  
*泛型方法*是引入了自己的类型参数的方法。这类似于声明泛型类型，但是类型参数的范围仅限于声明它的方法。允许使用静态和非静态通用方法，以及泛型类构造函数。

The syntax for a generic method includes a list of type parameters, inside angle brackets, which appears before the method's return type. For static generic methods, the type parameter section must appear before the method's return type.  
泛型方法的语法包括类型参数列表，在尖括号内，在方法的返回类型之前。对于静态泛型方法，类型参数部分必须出现在方法的返回类型之前。

The `Util` class includes a generic method, `compare`, which compares two `Pair` objects:  
`Util`类包含一个泛型函数，`compare`，它比较两个`Pair`对象：

```java
public class Util {
    public static <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2) {
        return p1.getKey().equals(p2.getKey()) &&
               p1.getValue().equals(p2.getValue());
    }
}

public class Pair<K, V> {

    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public void setKey(K key) { this.key = key; }
    public void setValue(V value) { this.value = value; }
    public K getKey()   { return key; }
    public V getValue() { return value; }
}
```

The complete syntax for invoking this method would be:  
调用此方法的完整语法为：

```java
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.<Integer, String>compare(p1, p2);
```

The type has been explicitly provided, as shown in bold. Generally, this can be left out and the compiler will infer the type that is needed:  
该类型已明确提供，如粗体所示。通常，可以将其忽略，编译器将推断出所需的类型：

```java
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.compare(p1, p2);
```

This feature, known as *type inference*, allows you to invoke a generic method as an ordinary method, without specifying a type between angle brackets. This topic is further discussed in the following section, [Type Inference](https://docs.oracle.com/javase/tutorial/java/generics/genTypeInference.html).  
此功能称为*类型推断*，使你可以在不指定尖括号之间的类型的情况下，将泛型方法作为普通方法调用。 后面章节[类型推断](./Type%20Inference%20类型推断.md)将进一步讨论该主题。

