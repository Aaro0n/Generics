# Wildcards and Subtyping 通配符和子类型

As described in [Generics, Inheritance, and Subtypes](https://docs.oracle.com/javase/tutorial/java/generics/inheritance.html), generic classes or interfaces are not related merely because there is a relationship between their types. However, you can use wildcards to create a relationship between generic classes or interfaces.  
正如在[泛型，继承和子类型](Generics%2C%20Inheritance%2C%20and%20Subtypes%20泛型，继承和子类型.md)这一章节描述的，泛型类或接口并没有仅仅因为它们的类型之间存在关系而相关。但是你可以通过通配符为泛型类或方法创建关系。

Given the following two regular (non-generic) classes:  
给定以下两个常规（非泛型）类：

```java
class A { /* ... */ }
class B extends A { /* ... */ }
```

It would be reasonable to write the following code:  
有理由写出下面的代码：
```java
B b = new B();
A a = b;
```

This example shows that inheritance of regular classes follows this rule of subtyping: class `B` is a subtype of class `A` if `B` extends `A`. This rule does not apply to generic types:  
这个例子表明，常规类的继承遵循子类型的规则:如果`B`继承了`A`，则类`B`是类`A`的子类型。这个规则不适用于泛型类型：

```java
List<B> lb = new ArrayList<>();
List<A> la = lb;   // compile-time error
```

Given that `Integer` is a subtype of `Number`, what is the relationship between `List<Integer>` and `List<Number>`?  
假设`Integer`是`Number`的子类型，`List<Integer>`和`List<Number>`之间的关系是什么？

![diagram showing that the common parent of List<Number> and List<Integer> is the list of unknown type](./static/generics-listParent.gif)  
The common parent is `List<?>`.  
共同的父类是`List<?>`

Although `Integer` is a subtype of `Number`, `List<Integer>` is not a subtype of `List<Number>` and, in fact, these two types are not related. The common parent of `List<Number>` and `List<Integer>` is `List<?>`.  
尽管`Integer`是`Number`的子类， `List<Integer>`却不是`List<Number>`的子类，事实上，这两个类型没有关系。`List<Number>`和`List<Integer>`共同的父类是`List<?>`。

In order to create a relationship between these classes so that the code can access `Number`'s methods through `List<Integer>`'s elements, use an upper bounded wildcard:  
为了在这些类之间创建关系，以便代码可以通过`List<Integer>`的元素访问`Number`的方法，使用上限通配符：

```java
List<? extends Integer> intList = new ArrayList<>();
List<? extends Number>  numList = intList;  // OK. List<? extends Integer> is a subtype of List<? extends Number>
```

Because `Integer` is a subtype of `Number`, and `numList` is a list of `Number` objects, a relationship now exists between `intList` (a list of `Integer` objects) and `numList`. The following diagram shows the relationships between several `List` classes declared with both upper and lower bounded wildcards.  
因为`Integer`是`Number`的子类，并且`numList`是`Number` 对象的列表。因此`intList`（`Integer`对象的列表）和`numList`之间存在关系。下图显示了使用上下界通配符声明的多个`List`类之间的关系。

![diagram showing that List<Integer> is a subtype of both List<? extends Integer> and List<?super Integer>. List<? extends Integer> is a subtype of List<? extends Number> which is a subtype of List<?>. List<Number> is a subtype of List<? super Number> and List>? extends Number>. List<? super Number> is a subtype of List<? super Integer> which is a subtype of List<?>.](./static/generics-wildcardSubtyping.gif)  
A hierarchy of several generic `List` class declarations.  
几个通用`List`类声明的层次结构。

The [Guidelines for Wildcard Use](https://docs.oracle.com/javase/tutorial/java/generics/wildcardGuidelines.html) section has more information about the ramifications of using upper and lower bounded wildcards.  
[通配符使用准则](Guidelines%20for%20Wildcard%20Use%20通配符使用准则.md) 部分提供了有关使用上下界通配符的后果的更多信息。