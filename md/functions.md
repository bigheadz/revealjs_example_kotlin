
# Functions
[ppt]:()
## 定义

keyword: fun

``` kotlin
fun double(x: Int): Int {
    return 2*x
}
```
[ppt]:()
## 使用

``` kotlin
val result = double(2)

// create instance of class Sample and call foo
Sample().foo() 
```
<!--
### Infix notation

* They are member functions or [extension functions](extensions.html)
* They have a single parameter
* They are marked with the `infix` keyword

``` kotlin
// Define extension to Int
infix fun Int.shl(x: Int): Int {
...
}

// call extension function using infix notation
1 shl 2

// is the same as
1.shl(2)
```
-->
[ppt]:()
### 参数
``` kotlin
fun powerOf(number: Int, exponent: Int) {
...
}
```
[ppt]:()
### 默认参数

name: Type = defaultValue, 

``` kotlin
fun read(b: Array<Byte>, off: Int = 0, len: Int = b.size) {
...
}
```

[ppt]:()

重载带默认参数的函数
``` kotlin
open class A {
    open fun foo(i: Int = 10) { ... }
}

class B : A() {
    override fun foo(i: Int) { ... }  // no default value allowed
}
```
[ppt]:()
### 命名参数

``` kotlin
fun reformat(str: String,
             normalizeCase: Boolean = true,
             upperCaseFirstLetter: Boolean = true,
             divideByCamelHumps: Boolean = false,
             wordSeparator: Char = ' ') {
...
}
```

``` kotlin
reformat(str)
reformat(str, true, true, false, '_') //默认参数

```
[ppt]:()
``` kotlin
//可读性更好
reformat(str,
    normalizeCase = true,
    upperCaseFirstLetter = true,
    divideByCamelHumps = false,
    wordSeparator = '_'
)

//只定义一部分参数
//java 不能这样用
reformat(str, wordSeparator = '_')
```

[ppt]:()
### 返回Unit
返回void的时候， 可以写返回Unit, return也可以不写
``` kotlin
fun printHello(name: String?): Unit {
    if (name != null)
        println("Hello ${name}")
    else
        println("Hi there!")
    // `return Unit` or `return` is optional
}
```
或者直接省略
``` kotlin
fun printHello(name: String?) {
    ...
}
```
[ppt]:()
### 单表达式函数

只返回一个表达式可以简写成
``` kotlin
fun double(x: Int): Int = x * 2
```
返回结果可推测的时候， 返回类型也可以省略
``` kotlin
fun double(x: Int) = x * 2
```
[ppt]:()
### 显式返回类型

* ``function(){代码块}``   
这样的形式必须写返回类型（except Unit)， 复杂控制流会导致推测类型过于困难

[ppt]:()
### Varargs

``` kotlin
fun <T> asList(vararg ts: T): List<T> {
    val result = ArrayList<T>()
    for (t in ts) // ts is an Array
        result.add(t)
    return result
}

val list = asList(1, 2, 3)
```
note:
只能标记一个``vararg``.
如果vararg不在最后面， 那后面的参数可以通过named parameter方式传进来， 或者后续参数类型是函数， 可以通过lambda表达式传进来

[ppt]:()
**spread** operation

```kotlin
val a = arrayOf(1, 2, 3)
val list = asList(-1, 0, *a, 4)
```
<!--
## Function Scope
* top level functions
* member functions
* extension functions
-->
[ppt]:()
### Local Functions

local functions可以访问外层函数的局部变量

``` kotlin
fun dfs(graph: Graph) {
    val visited = HashSet<Vertex>()
    fun dfs(current: Vertex) {
        if (!visited.add(current)) return
        for (v in current.neighbors)
            dfs(v)
    }

    dfs(graph.vertices[0])
}
```
[ppt]:()
### Member Functions

``` kotlin
class Sample() {
    fun foo() { print("Foo") }
}

//call
Sample().foo() // creates instance of class Sample and calls foo
```

[ppt]:()
## 泛型函数

``` kotlin
fun <T> singletonList(item: T): List<T> {
    // ...
}
```