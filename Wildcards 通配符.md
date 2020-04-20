# Wildcards 通配符

In generic code, the question mark (`?`), called the *wildcard*, represents an unknown type. The wildcard can be used in a variety of situations: as the type of a parameter, field, or local variable; sometimes as a return type (though it is better programming practice to be more specific). The wildcard is never used as a type argument for a generic method invocation, a generic class instance creation, or a supertype.  
在泛型代码中，问号标志 (`?`)，称为*通配符*，代表未知类型。通配符可以在多种情况下使用：作为参数类型，字段或局部变量的类型； 有时作为返回类型（尽管更具体一些是更好的编程实践）。通配符从不用作泛型方法调用、泛型类实例创建或超类型的类型参数。


The following sections discuss wildcards in more detail, including upper bounded wildcards, lower bounded wildcards, and wildcard capture.  
下面几节将更详细地讨论通配符，包括上界通配符、下界通配符和通配符捕获。