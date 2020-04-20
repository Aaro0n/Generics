# Lesson: Generics (Updated) 课程：泛型

In any nontrivial software project, bugs are simply a fact of life. Careful planning, programming, and testing can help reduce their pervasiveness, but somehow, somewhere, they'll always find a way to creep into your code. This becomes especially apparent as new features are introduced and your code base grows in size and complexity.  
在任何一个非平凡的软件项目中，bug都是生活中的一部分。小心的规划，编码，和测试可以帮组减少他们的普遍性，但是无论如何，它们总会找到一种方法潜入您的代码。随着新特性的引入，以及代码库的规模和复杂性的增加，这一点变得尤为明显。

Fortunately, some bugs are easier to detect than others. Compile-time bugs, for example, can be detected early on; you can use the compiler's error messages to figure out what the problem is and fix it, right then and there. Runtime bugs, however, can be much more problematic; they don't always surface immediately, and when they do, it may be at a point in the program that is far removed from the actual cause of the problem.  
幸运地，有些bug比其它的更容易被检测。例如，编译时期的bug可以在早期被检测到；可以使用编译的错误信息去弄清楚是什么问题，然后就可以在那里立即进行修复。然而，运行时期的bug会更加的棘手；它们并不总是立即出现，当它们出现时，可能是程序中的某个点与问题的实际原因相距甚远。


Generics add stability to your code by making more of your bugs detectable at compile time. After completing this lesson, you may want to follow up with the [Generics](https://docs.oracle.com/javase/tutorial/extra/generics/index.html) tutorial by Gilad Bracha.  
泛型通过在编译时检测出更多的bug来增加代码的稳定性。在本课程之后，你可能想要继续学习Gilad Bracha的[泛型](https://docs.oracle.com/javase/tutorial/extra/generics/index.html)教程。

