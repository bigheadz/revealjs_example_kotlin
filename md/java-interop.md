
# Calling Java code from Kotlin
Kotlin生来就是和Java混的

[ppt]:title

随便调：

``` kotlin
import java.util.*

fun demo(source: List<Int>) {
    val list = ArrayList<Int>()
    // 'for'-loops work for Java collections:
    for (item in source) {
        list.add(item)
    }
    // Operator conventions work as well:
    for (i in 0..source.size - 1) {
        list[i] = source[i] // get and set are called
    }
}
```
[ppt]:()

## Getters and Setters

java中的getter和setter会被转换成对应的property一样调用：

``` kotlin
import java.util.Calendar

fun calendarDemo() {
    val calendar = Calendar.getInstance()
    if (calendar.firstDayOfWeek == Calendar.SUNDAY) {  //call getFirstDayOfWeek()
        calendar.firstDayOfWeek = Calendar.MONDAY      //call setFirstDayOfWeek()
    }
    if (!calendar.isLenient) {                         //call isLenient() 
        calendar.isLenient = true                      //call setLenient()
    }
}
```

[ppt]:()

#### exception
Java只有setter， 就不会转换成property.

[ppt]:()

## Methods returning void

```java
//java
void test() { ... }
```
```kotlin
//kotlin
const t = test() // t is Unit
```

[ppt]:()

## java用到了Koltin的关键字

``` kotlin
//kotlin
foo.`is`(bar)
```

[ppt]:()

## Null-Safety and Platform Types
Java随时返回null, kotlin的non-null检查在java面前不堪一击。   
So, Platform Types应运而生。

[ppt]:()

``` kotlin
val list = ArrayList<String>() // non-null (constructor result)
list.add("Item")
val size = list.size // non-null (primitive int)
val item = list[0] // platform type inferred (ordinary Java object)
```
``` kotlin
item.substring(1) // allowed, may throw an exception if item == null
```

[ppt]:()

Platform types 不可直接标识， 它是被编译器inference出来的。但是也可以把它赋值给你想要的对应类里面去

``` kotlin
val nullable: String? = item // allowed, always works
val notNull: String = item // allowed, may fail at runtime
```
note:
编译器会尽可能的防止空指针被赋值到non-null类型中去， 及时抛出异常。

[ppt]:()

<!--
### Notation for Platform Types

编译器和IDE有时候如下标识Platform Types

* `T!` means "`T` or `T?`",
* `(Mutable)Collection<T>!` means "Java collection of `T` may be mutable or not, may be nullable or not",
* `Array<(out) T>!` means "Java array of `T` (or a subtype of `T`), nullable or not"

-->

### Nullability annotations

用annotations标识的java不会被转换成Platform type.
  * [JetBrains](https://www.jetbrains.com/idea/help/nullable-and-notnull-annotations.html)
(`@Nullable` and `@NotNull` from the `org.jetbrains.annotations` package)
  * Android (`com.android.annotations` and `android.support.annotations`)
  * JSR-305 (`javax.annotation`)
  * FindBugs (`edu.umd.cs.findbugs.annotations`)
  * Eclipse (`org.eclipse.jdt.annotation`)
  * Lombok (`lombok.NonNull`).

[ppt]:()

## Mapped types

[ppt]:()

| **Java type** | **Kotlin type**  |
|---------------|------------------|
| `byte`        | `kotlin.Byte`    |
| `short`       | `kotlin.Short`   |
| `int`         | `kotlin.Int`     |
| `long`        | `kotlin.Long`    |
| `char`        | `kotlin.Char`    |
| `float`       | `kotlin.Float`   |
| `double`      | `kotlin.Double`  |
| `boolean`     | `kotlin.Boolean` |

note:
Kotlin 会自动转换java的类（only在compile time, runtime不会变)

[ppt]:()

Some non-primitive built-in classes are also mapped:

| **Java type** | **Kotlin type**  |
|---------------|------------------|
| `java.lang.Object`       | `kotlin.Any!`    |
| `java.lang.Cloneable`    | `kotlin.Cloneable!`    |
| `java.lang.Comparable`   | `kotlin.Comparable!`    |
| `java.lang.Enum`         | `kotlin.Enum!`    |
| `java.lang.Annotation`   | `kotlin.Annotation!`    |
| `java.lang.Deprecated`   | `kotlin.Deprecated!`    |
| `java.lang.CharSequence` | `kotlin.CharSequence!`   |
| `java.lang.String`       | `kotlin.String!`   |
| `java.lang.Number`       | `kotlin.Number!`     |
| `java.lang.Throwable`    | `kotlin.Throwable!`    |

[ppt]:()

Java's boxed primitive types are mapped to nullable Kotlin types:

| **Java type**           | **Kotlin type**  |
|-------------------------|------------------|
| `java.lang.Byte`        | `kotlin.Byte?`   |
| `java.lang.Short`       | `kotlin.Short?`  |
| `java.lang.Integer`     | `kotlin.Int?`    |
| `java.lang.Long`        | `kotlin.Long?`   |
| `java.lang.Character`   | `kotlin.Char?`   |
| `java.lang.Float`       | `kotlin.Float?`  |
| `java.lang.Double`      | `kotlin.Double?`  |
| `java.lang.Boolean`     | `kotlin.Boolean?` |
[ppt]:()
集合类会被转成成kotlin的read-only或者mutable。
例如： 
List<T> 被转成成List<T>或者MutableList<T>。

| **Java type** | **Kotlin read-only type**  | **Kotlin mutable type** | **Loaded platform type** |
|---------------|------------------|----|----|
| `Iterator<T>`        | `Iterator<T>`        | `MutableIterator<T>`            | `(Mutable)Iterator<T>!`            |
| `Iterable<T>`        | `Iterable<T>`        | `MutableIterable<T>`            | `(Mutable)Iterable<T>!`            |
| `Collection<T>`      | `Collection<T>`      | `MutableCollection<T>`          | `(Mutable)Collection<T>!`          |
| `Set<T>`             | `Set<T>`             | `MutableSet<T>`                 | `(Mutable)Set<T>!`                 |
| `List<T>`            | `List<T>`            | `MutableList<T>`                | `(Mutable)List<T>!`                |
| `ListIterator<T>`    | `ListIterator<T>`    | `MutableListIterator<T>`        | `(Mutable)ListIterator<T>!`        |
| `Map<K, V>`          | `Map<K, V>`          | `MutableMap<K, V>`              | `(Mutable)Map<K, V>!`              |
| `Map.Entry<K, V>`    | `Map.Entry<K, V>`    | `MutableMap.MutableEntry<K,V>` | `(Mutable)Map.(Mutable)Entry<K, V>!` |
[ppt]:()

Java's arrays

| **Java type** | **Kotlin type**  |
|---------------|------------------|
| `int[]`       | `kotlin.IntArray!` |
| `String[]`    | `kotlin.Array<(out) String>!` |
[ppt]:()

## Java generics in Kotlin

Kotlin的泛型和java略有不同。

[ppt]:()

  * `Foo<? extends Bar>` becomes `Foo<out Bar!>!`
  * `Foo<? super Bar>` becomes `Foo<in Bar!>!`
  * `List` becomes `List<*>!`, i.e. `List<out Any?>!`
  
note: 更为具体的，请自己看

[ppt]:()
<!--
和java一样， runtime期间的泛型没有被保留的
``` kotlin
if (a is List<Int>) // Error: cannot check if it is really a List of Ints
// but
if (a is List<*>) // OK: no guarantees about the contents of the list
```
-->
<!--
## Java Arrays

Arrays in Kotlin are invariant, unlike Java. This means that Kotlin does not let us assign an `Array<String>` to an `Array<Any>`,
which prevents a possible runtime failure. Passing an array of a subclass as an array of superclass to a Kotlin method is also prohibited,
but for Java methods this is allowed (through [platform types](#null-safety-and-platform-types) of the form `Array<(out) String>!`).

Arrays are used with primitive datatypes on the Java platform to avoid the cost of boxing/unboxing operations.
As Kotlin hides those implementation details, a workaround is required to interface with Java code.
There are specialized classes for every type of primitive array (`IntArray`, `DoubleArray`, `CharArray`, and so on) to handle this case.
They are not related to the `Array` class and are compiled down to Java's primitive arrays for maximum performance.

Suppose there is a Java method that accepts an int array of indices:

``` java
public class JavaArrayExample {

    public void removeIndices(int[] indices) {
        // code here...
    }
}
```

To pass an array of primitive values you can do the following in Kotlin:

``` kotlin
val javaObj = JavaArrayExample()
val array = intArrayOf(0, 1, 2, 3)
javaObj.removeIndices(array)  // passes int[] to method
```

When compiling to JVM byte codes, the compiler optimizes access to arrays so that there's no overhead introduced:

``` kotlin
val array = arrayOf(1, 2, 3, 4)
array[x] = array[x] * 2 // no actual calls to get() and set() generated
for (x in array) { // no iterator created
    print(x)
}
```

Even when we navigate with an index, it does not introduce any overhead

``` kotlin
for (i in array.indices) { // no iterator created
    array[i] += 2
}
```

Finally, *in*{: .keyword }-checks have no overhead either

``` kotlin
if (i in array.indices) { // same as (i >= 0 && i < array.size)
    print(array[i])
}
```
-->

## Java Varargs

``` java
public class JavaArrayExample {
    public void removeIndices(int... indices) {
        // code here...
    }
}
```
``` kotlin
val javaObj = JavaArray()
val array = intArrayOf(0, 1, 2, 3)
javaObj.removeIndicesVarArg(*array)
```

[ppt]:()
<!--
## Operators

Since Java has no way of marking methods for which it makes sense to use the operator syntax, Kotlin allows using any
Java methods with the right name and signature as operator overloads and other conventions (`invoke()` etc.)
Calling Java methods using the infix call syntax is not allowed.

-->
## Checked Exceptions

Kotlin不强制你catch Exception
``` kotlin
fun render(list: List<*>, to: Appendable) {
    for (item in list) {
        to.append(item.toString()) 
        // Java would require us to catch IOException here
    }
}
```
[ppt]:()

## Object Methods

* `java.lang.Object`被转换为`Any`   
* Any只包含 `toString()`, `hashCode()`, `equals()`
* kotin 使用了extension functions来实现`java.lang.Object`的其他成员

[ppt]:()

### wait()/notify()


```kotlin
(foo as java.lang.Object).wait()
```

> [Effective Java](http://www.oracle.com/technetwork/java/effectivejava-136174.html) 建议我们不用`wait()` and `notify()`， 而使用concurrency 

[ppt]:()

### getClass()

``` kotlin
val fooClass = foo::class.java
//extension property
val fooClass = foo.javaClass
```
[ppt]:()
<!--
### clone()

To override `clone()`, your class needs to extend `kotlin.Cloneable`:

```kotlin

class Example : Cloneable {
    override fun clone(): Any { ... }
}
```

 Do not forget about [Effective Java](http://www.oracle.com/technetwork/java/effectivejava-136174.html), Item 11: *Override clone judiciously*.

### finalize()

To override `finalize()`, all you need to do is simply declare it, without using the *override*{:.keyword} keyword:

```kotlin
class C {
    protected fun finalize() {
        // finalization logic
    }
}
```

According to Java's rules, `finalize()` must not be *private*{: .keyword }.
-->
## Inheritance from Java classes

随便继承java抽象类和interface，当然只有有一个父类.

[ppt]:()
## Accessing static members

``` kotlin
if (Character.isLetter(a)) {
    // ...
}
```
[ppt]:()
<!--
## Java Reflection

Java reflection works on Kotlin classes and vice versa. As mentioned above, you can use `instance::class.java`,
`ClassName::class.java` or `instance.javaClass` to enter Java reflection through `java.lang.Class`.
 
Other supported cases include acquiring a Java getter/setter method or a backing field for a Kotlin property, a `KProperty` for a Java field, a Java method or constructor for a `KFunction` and vice versa.

## SAM Conversions

Just like Java 8, Kotlin supports SAM conversions. This means that Kotlin function literals can be automatically converted
into implementations of Java interfaces with a single non-default method, as long as the parameter types of the interface
method match the parameter types of the Kotlin function.

You can use this for creating instances of SAM interfaces:

``` kotlin
val runnable = Runnable { println("This runs in a runnable") }
```

...and in method calls:

``` kotlin
val executor = ThreadPoolExecutor()
// Java signature: void execute(Runnable command)
executor.execute { println("This runs in a thread pool") }
```

If the Java class has multiple methods taking functional interfaces, you can choose the one you need to call by
using an adapter function that converts a lambda to a specific SAM type. Those adapter functions are also generated
by the compiler when needed.

``` kotlin
executor.execute(Runnable { println("This runs in a thread pool") })
```

Note that SAM conversions only work for interfaces, not for abstract classes, even if those also have just a single
abstract method.

Also note that this feature works only for Java interop; since Kotlin has proper function types, automatic conversion
of functions into implementations of Kotlin interfaces is unnecessary and therefore unsupported.
-->
## Using JNI with Kotlin

``` kotlin
external fun foo(x: Int): Double
```