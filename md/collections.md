# Collections
[ppt]:()
Kotlin的集合分成可变和不可变。

``` kotlin
val numbers: MutableList<Int> = mutableListOf(1, 2, 3)
val readOnlyView: List<Int> = numbers
println(numbers)        // prints "[1, 2, 3]"
numbers.add(4)
println(readOnlyView)   // prints "[1, 2, 3, 4]"
readOnlyView.clear()    // -> does not compile

val strings = hashSetOf("a", "b", "c", "c")
assert(strings.size == 3)
```
note: 
It is important to understand up front the difference between a read-only view of a mutable collection, and an actually immutable collection. Both are easy to create, but the type system doesn't express the difference, so keeping track of that (if it's relevant) is up to you.

[ppt]:()

* `listOf()`, `mutableListOf()`
* `setOf()`, `mutableSetOf()`,
* `mapOf(a to b, c to d)`

[ppt]:()

``` kotlin
val items = listOf(1, 2, 3)

//the returned list is guaranteed to never change
class Controller {
    private val _items = mutableListOf<String>()
    val items: List<String> get() = _items.toList()
}
```
note: 

这个可能就是关键了， 可以保证对外的函数保证其内部不会变化

[ppt]:()

一些有用的扩展函数:

``` kotlin
val items = listOf(1, 2, 3, 4)
items.first() == 1
items.last() == 4
items.filter { it % 2 == 0 }   // returns [2, 4]

val rwList = mutableListOf(1, 2, 3)
rwList.requireNoNulls()        // returns [1, 2, 3]

// prints "No items above 6"
if (rwList.none { it > 6 }) println("No items above 6")  
val item = rwList.firstOrNull()
```

note: 
... as well as all the utilities you would expect such as sort, zip, fold, reduce and so on.
<!--
Maps follow the same pattern. They can be easily instantiated and accessed like this:

``` kotlin
val readWriteMap = hashMapOf("foo" to 1, "bar" to 2)
println(readWriteMap["foo"])  // prints "1"
val snapshot: Map<String, Int> = HashMap(readWriteMap)
```
-->