# Generic Methods and Bounded Type Parameters 泛型函数和有界类型参数

Bounded type parameters are key to the implementation of generic algorithms. Consider the following method that counts the number of elements in an array `T[]` that are greater than a specified element `elem`.  
有界类型参数是实现泛型算法的关键。考虑以下方法，该方法计算数组`T[]`中大于指定元素`elem`的元素数量。

```java
public static <T> int countGreaterThan(T[] anArray, T elem) {
	int count = 0;
	for (T e : anArray)
		if (e > elem) // compiler error
			++count;
	return count;
}
```
The implementation of the method is straightforward, but it does not compile because the greater than operator (>) applies only to primitive types such as `short`,`int`, `double`, `long`, `float`, `byte`, and `char`. You cannot use the > operator to compare objects. To fix the problem, use a type parameter bounded by the `Comparable<T>` interface:  
该方法的实现很简单，但是不能编译，因为大于运算符（>）仅适用于基本类型，例如`short`，`int`，`double`，`long`，`float`，`byte`和`char`。你不能使用>运算符比较对象。 要解决此问题，使用由`Comparable<T>`接口限制的类型参数：
```java
public interface Comparable<T> {
	public int compareTo(T o);
}
```
The resulting code will be:  
结果代码将是：
```java
public static <T extends Comparable<T>> int countGreaterThan(T[] anArray, T elem) {
	int count = 0;
	for (T e : anArray)
		if (e.compareTo(elem) > 0)
			++count;
	return count;
}
```