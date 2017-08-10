# Exceptions
[ppt]:()
## Exception Classes


``` kotlin
throw MyException("Hi There!")
```

``` kotlin
try {
    // some code
}
catch (e: SomeException) {
    // handler
}
finally {
    // optional finally block
}
```
[ppt]:()
### Try is an expression

``` kotlin
val a: Int? = 
    try { parseInt(input) } catch (e: NumberFormatException) { null }
```

[ppt]:()
<!--
## Checked Exceptions

Kotlin does not have checked exceptions. There are many reasons for this, but we will provide a simple example.

The following is an example interface of the JDK implemented by `StringBuilder` class

``` java
Appendable append(CharSequence csq) throws IOException;
```

What does this signature say? It says that every time I append a string to something (a `StringBuilder`, some kind of a log, a console, etc.)
I have to catch those `IOExceptions`. Why? Because it might be performing IO (`Writer` also implements `Appendable`)...
So it results into this kind of code all over the place:

``` kotlin
try {
    log.append(message)
}
catch (IOException e) {
    // Must be safe
}
```

And this is no good, see [Effective Java](http://www.oracle.com/technetwork/java/effectivejava-136174.html), Item 65: *Don't ignore exceptions*.

Bruce Eckel says in [Does Java need Checked Exceptions?](http://www.mindview.net/Etc/Discussions/CheckedExceptions):

> Examination of small programs leads to the conclusion that requiring exception specifications could both enhance developer productivity and enhance code quality, but experience with large software projects suggests a different result – decreased productivity and little or no increase in code quality.

Other citations of this sort:

* [Java's checked exceptions were a mistake](http://radio-weblogs.com/0122027/stories/2003/04/01/JavasCheckedExceptionsWereAMistake.html) (Rod Waldhoff)
* [The Trouble with Checked Exceptions](http://www.artima.com/intv/handcuffs.html) (Anders Hejlsberg)
-->

## The Nothing type

`throw` is an expression in Kotlin:

``` kotlin
val s = person.name ?: throw IllegalArgumentException("Name required")
```

`throw` 返回的Type是 `Nothing`.
``Nothing``没有值， 代表无法到达的代码。


[ppt]:()

you can use `Nothing` to mark a function that never returns:

``` kotlin
fun fail(message: String): Nothing {
    throw IllegalArgumentException(message)
}
```

``` kotlin
val s = person.name ?: fail("Name required")
println(s)     // 's' is known to be initialized at this point
```
