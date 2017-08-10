# Calling Kotlin from Java

Kotlin code can be called from Java easily.

[ppt]: title

## Properties

Kotlin的property编译成下面的java属性:

 * A getter method 
 * A setter method
 * A private field

[ppt]: Properties

For example, **```var firstName: String```**:

``` java
private String firstName;

public String getFirstName() {
    return firstName;
}

public void setFirstName(String firstName) {
    this.firstName = firstName;
}
```
[ppt]: Properties

如果属性以 **`is`**开头, 规则略有不同：
* `isOpen`  
* getter ==>  `isOpen()`  
* setter ==>  `setOpen()`

>This rule applies for properties of any type, not just `Boolean`.

[ppt]: Properties

## Package-Level Functions

在文件中直接声明的函数和属性会被编译到一个以包名+文件名为类名的java的静态属性。


[ppt]: Package-Level_Functions

举个栗子：
``` kotlin
// example.kt
package demo
class Foo
fun bar() {
}
```

``` java
// Java
new demo.Foo(); //class 还是原来的规则
demo.ExampleKt.bar(); //static的文件名被包裹了一层
```

[ppt]: Package-Level_Functions

用`@JvmName`指定生成的java class的名字:

``` kotlin
@file:JvmName("DemoUtils")

package demo

class Foo

fun bar() {
}

```

``` java
// Java
new demo.Foo();
demo.DemoUtils.bar();
```
[ppt]: Package-Level_Functions

* 默认， 不允许生成的java class重名  
* ``@JvmMultifileClass``允许合并

``` kotlin
// oldutils.kt
@file:JvmName("Utils")B
@file:JvmMultifileClass
package demo
fun foo() {
}
```
``` kotlin
// newutils.kt
@file:JvmName("Utils")
@file:JvmMultifileClass
package demo
fun bar() {
}
```
``` java
// Java
demo.Utils.foo();
demo.Utils.bar();
```

[ppt]: Package-Level_Functions

## Instance Fields
如果你不喜欢用getter和setter， 用 `@JvmField`。

[ppt]: Instance_Fields

``` kotlin
class C(id: String) {
    @JvmField val ID = id
}
```
``` java
// Java
class JavaClient {
    public String getID(C c) {
        return c.ID;
    }
}
```

[ppt]: Instance_Fields

前提條件： 
* backing field 
* 非 private
* 非 `open`
* 非 `override`
* 非 `const` 
* 非 delegated property.

[ppt]: Instance_Fields

## Static Fields

在companion object 和 named object中会生成static的field

[ppt]: Static_Fields

下面情况会暴露成field, 而不会生成 getter, setter

 - `@JvmField` annotation;
 - `lateinit` modifier;
 - `const` modifier.
 
[ppt]: Static_Fields

@JvmField
```
class Key(val value: Int) {
    companion object {
        @JvmField 
        val COMPARATOR: Comparator<Key> = compareBy<Key> { it.value }
    }
}
```

``` java
// Java
Key.COMPARATOR.compare(key1, key2);
// public static final field in Key class
```

[ppt]: Static_Fields

lateinit

``` kotlin
object Singleton {
    lateinit var provider: Provider
}
```

``` java
// Java
Singleton.provider = new Provider();
// public static non-final field in Singleton class
```

[ppt]: Static_Fields

const
``` kotlin
// file example.kt
object Obj {
    const val CONST = 1
}
class C {
    companion object {
        const val VERSION = 9
    }
}
const val MAX = 239
```

In Java:

``` java
int c = Obj.CONST;
int v = C.VERSION;
int d = ExampleKt.MAX;
```
[ppt]: Static_Fields

## Static Methods

* package-level
* companion + @JvmStatic
* named Object + @JvmStatic

[ppt]:Static_Methods

companion object
``` kotlin
class C {
    companion object {
        @JvmStatic fun foo() {}
        fun bar() {}
    }
}
```

``` java
C.foo(); // works fine
C.bar(); // error: not a static method
C.Companion.foo(); // instance method remains
C.Companion.bar(); // the only way it works
```

[ppt]:Static_Methods

named objects:
``` kotlin
object Obj {
    @JvmStatic fun foo() {}
    fun bar() {}
}
```

``` java
Obj.foo(); // works fine
Obj.bar(); // error
Obj.INSTANCE.bar(); // works, a call through the singleton instance
Obj.INSTANCE.foo(); // works too
```

[ppt]:Static_Methods

`@JvmStatic` 作用到property上面就是产生java可调用的get set方法.

[ppt]:Static_Methods

## Visibility

|kotlin|java|
|:------:|:------:|
|protected |protected|
|public|public|
|private members|private members|
|private top-level|package-local declarations|
|internal|public|
Note:
    note that Java allows accessing protected members from other classes in the same package
  and Kotlin doesn't, so Java classes will have broader access to the code
  
[ppt]:Visibility


<!--
 KClass

Sometimes you need to call a Kotlin method with a parameter of type `KClass`.
There is no automatic conversion from `Class` to `KClass`, so you have to do it manually by invoking the equivalent of
the `Class<T>.kotlin` extension property:

```kotlin
kotlin.jvm.JvmClassMappingKt.getKotlinClass(MainView.class)
```
-->

## Handling signature clashes

@JvmName来处理签名冲突

[ppt]:Handling_signature_clashes

下面两个签名一样的
``` kotlin
fun List<String>.filterValid(): List<String>
fun List<Int>.filterValid(): List<Int>
```
如果不想改kotlin的名字:
``` kotlin
fun List<String>.filterValid(): List<String>

@JvmName("filterValidInt")
fun List<Int>.filterValid(): List<Int>
```

[ppt]:Handling_signature_clashes

处理x和getX：

``` kotlin
val x: Int
    @JvmName("getX_prop")
    get() = 15

fun getX() = 10
```

[ppt]:Handling_signature_clashes

## Overloads Generation
通常带有默认参数的kotlin函数只生成一个对应的java函数（没有默认参数部分）
用@JvmOverloads生成多个函数

``` kotlin
@JvmOverloads fun f(a: String, b: Int = 0, c: String = "abc") {
    ...
}
```

``` java
// Java
void f(String a, int b, String c) { }
void f(String a, int b) { }
void f(String a) { }
```

[ppt]:Overloads_Generation

* 也可用于构造函数， 静态函数
* 不可用于抽象函数和接口

<!---
Note that, as described in [Secondary Constructors](classes.html#secondary-constructors), if a class has default
values for all constructor parameters, a public no-argument constructor will be generated for it. This works even
if the @JvmOverloads annotation is not specified.
-->

[ppt]:Overloads_Generation

## Checked Exceptions

kotlin没有checked Exceptions

[ppt]:Checked_Exceptions

``` kotlin
// example.kt
package demo

fun foo() {
    throw IOException()
}
```

``` java
// Java
try {
  demo.Example.foo();
}
catch (IOException e) { 
// error: foo() does not declare IOException in the throws list
}
```

`@Throws`出场了

``` kotlin
@Throws(IOException::class)
fun foo() {
    throw IOException()
}
```

[ppt]:Checked_Exceptions

## Null-safety

Kotlin会在运行时检测public function( non-nulls parameter) 如果发现参数为空， 会抛出`NullPointerException`

[ppt]:Null-safety

<!--

## Variant generics

When Kotlin classes make use of [declaration-site variance](generics.html#declaration-site-variance), there are two 
options of how their usages are seen from the Java code. Let's say we have the following class and two functions that use it:

``` kotlin
class Box<out T>(val value: T)

interface Base
class Derived : Base

fun boxDerived(value: Derived): Box<Derived> = Box(value)
fun unboxBase(box: Box<Base>): Base = box.value
```

A naive way of translating these functions into Java would be this:
 
``` java
Box<Derived> boxDerived(Derived value) { ... }
Base unboxBase(Box<Base> box) { ... }
``` 

The problem is that in Kotlin we can say `unboxBase(boxDerived("s"))`, but in Java that would be impossible, because in Java 
  the class `Box` is *invariant* in its parameter `T`, and thus `Box<Derived>` is not a subtype of `Box<Base>`. 
  To make it work in Java we'd have to define `unboxBase` as follows:
  
``` java
Base unboxBase(Box<? extends Base> box) { ... }  
```  

Here we make use of Java's *wildcards types* (`? extends Base`) to emulate declaration-site variance through use-site 
variance, because it is all Java has.

To make Kotlin APIs work in Java we generate `Box<Super>` as `Box<? extends Super>` for covariantly defined `Box` 
(or `Foo<? super Bar>` for contravariantly defined `Foo`) when it appears *as a parameter*. When it's a return value,
we don't generate wildcards, because otherwise Java clients will have to deal with them (and it's against the common 
Java coding style). Therefore, the functions from our example are actually translated as follows:
  
``` java
// return type - no wildcards
Box<Derived> boxDerived(Derived value) { ... }
 
// parameter - wildcards 
Base unboxBase(Box<? extends Base> box) { ... }
```

NOTE: when the argument type is final, there's usually no point in generating the wildcard, so `Box<String>` is always
  `Box<String>`, no matter what position it takes.

If we need wildcards where they are not generated by default, we can use the `@JvmWildcard` annotation:

``` kotlin
fun boxDerived(value: Derived): Box<@JvmWildcard Derived> = Box(value)
// is translated to 
// Box<? extends Derived> boxDerived(Derived value) { ... }
```

On the other hand, if we don't need wildcards where they are generated, we can use `@JvmSuppressWildcards`:

``` kotlin
fun unboxBase(box: Box<@JvmSuppressWildcards Base>): Base = box.value
// is translated to 
// Base unboxBase(Box<Base> box) { ... }
```

NOTE: `@JvmSuppressWildcards` can be used not only on individual type arguments, but on entire declarations, such as 
functions or classes, causing all wildcards inside them to be suppressed.

-->

<!--
### Translation of type Nothing
 
The type [`Nothing`](exceptions.html#the-nothing-type) is special, because it has no natural counterpart in Java. Indeed, every Java reference type, including
`java.lang.Void`, accepts `null` as a value, and `Nothing` doesn't accept even that. So, this type cannot be accurately
represented in the Java world. This is why Kotlin generates a raw type where an argument of type `Nothing` is used:

``` kotlin
fun emptyList(): List<Nothing> = listOf()
// is translated to
// List emptyList() { ... }
```
-->