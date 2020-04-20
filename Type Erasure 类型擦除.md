# Type Erasure 类型擦除

Generics were introduced to the Java language to provide tighter type checks at compile time and to support generic programming. To implement generics, the Java compiler applies type erasure to:  
Java语言引入了泛型，以在编译时提供更严格的类型检查并支持泛型编程。 为了实现泛型，Java编译器将类型擦除应用于：

- Replace all type parameters in generic types with their bounds or `Object` if the type parameters are unbounded. The produced bytecode, therefore, contains only ordinary classes, interfaces, and methods.  
如果类型参数不受限制，则将泛型类型中的所有类型参数替换为其边界或`Object`。 因此，产生的字节码仅包含普通的类，接口和方法。

- Insert type casts if necessary to preserve type safety.  
必要时插入类型转换，以保持类型安全。

- Generate bridge methods to preserve polymorphism in extended generic types.  
生成桥接方法以在扩展的泛型类型中保留多态。

Type erasure ensures that no new classes are created for parameterized types; consequently, generics incur no runtime overhead.  
类型擦除可确保不会为参数化类型创建新的类； 因此，泛型不会带来运行时开销。