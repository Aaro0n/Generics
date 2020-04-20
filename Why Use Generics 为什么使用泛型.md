# Why Use Generics? 为什么使用泛型？

In a nutshell, generics enable types (classes and interfaces) to be parameters when defining classes, interfaces and methods. Much like the more familiar formal parameters used in method declarations, type parameters provide a way for you to re-use the same code with different inputs. The difference is that the inputs to formal parameters are values, while the inputs to type parameters are types.  
简而言之，泛型支持在定义类、接口和方法时将类型(类和接口)作为参数。就像在方法声明中常见的形式参数，类型参数为您提供了一种方法，可以在不同的输入中重用相同的代码。区别在于形式参数的输入是值，而类型参数的输入是类型。

Code that uses generics has many benefits over non-generic code:  
与非泛型代码相比，使用泛型的代码具有许多优点：

- Stronger type checks at compile time. 在编译时进行更强的类型检查。

    A Java compiler applies strong type checking to generic code and issues errors if the code violates type safety. Fixing compile-time errors is easier than fixing runtime errors, which can be difficult to find.  
    Java编译器将强类型检查应用于通用代码，如果代码违反类型安全，则会发出错误。修复编译时错误比修复运行时错误容易，后者可能很难找到。

- Elimination of casts. 消除转换

    The following code snippet without generics requires casting:  
    下面的代码片段没有泛型的需要强制转换
    ```java
    List list = new ArrayList();
    list.add("hello");
    String s = (String) list.get(0);
    ```
    When re-written to use generics, the code does not require casting:  
    当使用泛型进行重写，代码就不需要强制转换了
    ```java
    List<String> list = new ArrayList<String>();
    list.add("hello");
    String s = list.get(0); // no cast
    ```
- Enabling programmers to implement generic algorithms. 使程序员能够实现泛型算法。

    By using generics, programmers can implement generic algorithms that work on collections of different types, can be customized, and are type safe and easier to read.  
    通过使用泛型，程序员可以实现工作在不同类型的集合的，可以自定义并且类型安全且易于阅读的泛型算法。

