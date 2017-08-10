# Destructuring Declarations
[ppt]:()

``` kotlin
class Person {
    ...
    operator fun  component1(): String {}
    operator fun  component2(): Int {}
}
//可以这样用
val (name, age) = person 
//实际被编译成
val name = person.component1()
val age = person.component2()
```
``` kotlin
println(name)
println(age)
//也可以这样用
for ((a, b) in collection) { ... }
```
[ppt]:()

Example: Returning Two Values from a Function
 
``` kotlin
data class Result(val result: Int, val status: Status)
fun function(...): Result {
    // computations
    
    return Result(result, status)
}

// Now, to use this function:
val (result, status) = function(...)
```
note:
因为data class 自动定义 `componentN()` functions, 所以解构可以正常使用

[ppt]:()
Example: 最佳遍历map的方式:

``` kotlin
for ((key, value) in map) {
   // do something with the key and the value
}
```
note:
To make this work, we should 

* present the map as a sequence of values by providing an `iterator()` function,
* present each of the elements as a pair by providing functions `component1()` and `component2()`.
  
And indeed, the standard library provides such extensions:

``` kotlin
operator fun <K, V> Map<K, V>.iterator(): Iterator<Map.Entry<K, V>> = entrySet().iterator()
operator fun <K, V> Map.Entry<K, V>.component1() = getKey()
operator fun <K, V> Map.Entry<K, V>.component2() = getValue()
```  
  
So you can freely use destructuring declarations in *for*{: .keyword }-loops with maps (as well as collections of data class instances etc).

[ppt]:()
不用的参数
``` kotlin
val (_, status) = getResult()
```
[ppt]:()

Destructuring in Lambdas (since 1.1)

``` kotlin
map.mapValues { entry -> "${entry.value}!" }
map.mapValues { (key, value) -> "$value!" }
```

``` kotlin
{ a -> ... } // one parameter
{ a, b -> ... } // two parameters
{ (a, b) -> ... } // a destructured pair
{ (a, b), c -> ... } // a destructured pair and another parameter
```
``` kotlin
map.mapValues { (_, value) -> "$value!" }
map.mapValues { (_, value: String) -> "$value!" }
```