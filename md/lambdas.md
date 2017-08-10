# Higher-Order Functions and Lambdas
[ppt]:()
## Higher-Order Functions

何为高阶？

[ppt]:()

以函数为参数或者返回函数
``` kotlin
fun <T> lock(lock: Lock, body: () -> T): T {
    lock.lock()
    try {
        return body() // ! 调用函数
    }
    finally {
        lock.unlock()
    }
}

```
``` kotlin

//call
fun toBeSynchronized() = sharedResource.operation()

val result = lock(lock, ::toBeSynchronized)

//or call
val result = lock(lock, { sharedResource.operation() })
```


[ppt]:()

```kotlin
fun use() {
    useHighFunction(::topFuncion)
    useHighFunction(Foo()::memberFuntion)
    useHighFunction(Foo.Companion::staticFuncion)
}

fun useHighFunction(callFun: () -> Unit) {
    callFun()
}

```

[ppt]:()

先来粗略看看lambda：

* surrounded by curly braces {}
* parameters before `->` (may be omitted),
* The body goes after `->` (when present).

[ppt]:()



``` kotlin
fun local(lock: Lock, oper: ()-> Unit)

//函数的最后参数是function， 可以写到括号的外面
lock (lock) {
    sharedResource.operation()
}
```
 

``` kotlin
// Another example
fun <T, R> List<T>.map(transform: (T) -> R): List<R> {
    val result = arrayListOf<R>()
    for (item in this)
        result.add(transform(item))
    return result
}

//called
//有且只有一个function参数的时候， 可以省略掉()
val doubled = ints.map { value -> value * 2 }
```

[ppt]:()
### `it`: implicit name of a single parameter

``` kotlin
//只有一个参数可以省略 -> 用it代替这个参数
ints.map { it * 2 }
```
``` kotlin
//链式调用
strings.filter { it.length == 5 }.sortBy { it }.map { it.toUpperCase() }
```
[ppt]:()
### Underscore for unused variables (since 1.1)

``` kotlin
map.forEach { _, value -> println("$value!") }
```
<!--
### Destructuring in Lambdas (since 1.1)

later
 
## Inline Functions

later

## Lambda Expressions and Anonymous Functions

A lambda expression or an anonymous function is a "function literal", i.e. a function that is not declared,
but passed immediately as an expression. Consider the following example:

``` kotlin
max(strings, { a, b -> a.length < b.length })
```

Function `max` is a higher-order function, i.e. it takes a function value as the second argument.
This second argument is an expression that is itself a function, i.e. a function literal. As a function, it is equivalent to

``` kotlin
fun compare(a: String, b: String): Boolean = a.length < b.length
```
-->

[ppt]:()

### Function Types

``` kotlin
//`less` is of type `(T, T) -> Boolean`
fun <T> max(collection: Collection<T>, less: (T, T) -> Boolean): T? {
    var max: T? = null
    for (it in collection)
        if (max == null || less(max, it))
            max = it
    return max
}
```

``` kotlin
val compare: (x: T, y: T) -> Int = ...
```

[ppt]:()
### Lambda语法

``` kotlin
val sum = { x: Int, y: Int -> x + y }
//or
val sum: (Int, Int) -> Int = { x, y -> x + y }
```

``` kotlin
ints.filter { it > 0 } // this literal is of type '(it: Int) -> Boolean'
```

[ppt]:()

return

``` kotlin
ints.filter {
    val shouldFilter = it > 0 
    shouldFilter
}
//equals to 
ints.filter {
    val shouldFilter = it > 0 
    return@filter shouldFilter
}
```

[ppt]:()

### Anonymous Functions

``` kotlin
fun(x: Int, y: Int): Int = x + y

fun(x: Int, y: Int): Int {
    return x + y
}

ints.filter(fun(item) = item > 0)
```

[ppt]:()

### 闭包

* unlike java， 可以访问并改变外部的变量
``` kotlin
var sum = 0
ints.filter { it > 0 }.forEach {
    sum += it
}
print(sum)
```

note: 
lambda expression, anonymous function, local function, object expression都可以访问闭包类的变量

[ppt]:()

<!-- 
### Function Literals with Receiver

Kotlin provides the ability to call a function literal with a specified _receiver object_.
Inside the body of the function literal, you can call methods on that receiver object without any additional qualifiers.
This is similar to extension functions, which allow you to access members of the receiver object inside the body of the function.
One of the most important examples of their usage is [Type-safe Groovy-style builders](type-safe-builders.html).

The type of such a function literal is a function type with receiver:

``` kotlin
sum : Int.(other: Int) -> Int
```

The function literal can be called as if it were a method on the receiver object:

``` kotlin
1.sum(2)
```

The anonymous function syntax allows you to specify the receiver type of a function literal directly.
This can be useful if you need to declare a variable of a function type with receiver, and to use it later.

``` kotlin
val sum = fun Int.(other: Int): Int = this + other
```

Lambda expressions can be used as function literals with receiver when the receiver type can be inferred from context.

``` kotlin
class HTML {
    fun body() { ... }
}

fun html(init: HTML.() -> Unit): HTML {
    val html = HTML()  // create the receiver object
    html.init()        // pass the receiver object to the lambda
    return html
}


html {       // lambda with receiver begins here
    body()   // calling a method on the receiver object
}
```
-->