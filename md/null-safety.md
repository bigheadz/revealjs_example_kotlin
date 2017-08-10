# Null Safety
[ppt]:()

## Nullable types and Non-Null Types

Kotlin's type system is aimed at eliminating the danger of null references from code, also known as the [The Billion Dollar Mistake](http://en.wikipedia.org/wiki/Tony_Hoare#Apologies_and_retractions).

[ppt]:()
一般来说， 抛出NullPointerException的情况
* `throw NullPointerException()`
* Usage of the `!!` operator that is described below
* External Java code has caused it

[ppt]:()

``` kotlin
var a: String = "abc"
a = null // compilation error
```
To allow nulls
``` kotlin
var b: String? = "abc"
b = null // ok
```
[ppt]:()

``` kotlin
//it's safe
val l = a.length
//it's not safe
val l = b.length // error: variable 'b' can be null
```
[ppt]:()

``` kotlin
val l = if (b != null) b.length else -1
```

``` kotlin
if (b != null && b.length > 0) {
    print("String of length ${b.length}")
} else {
    print("Empty string")
}
```

note:
that this only works where `b` is immutable (i.e. a local variable which is not modified between the check and the
usage or a member *val*{: .keyword } which has a backing field and is not overridable), because otherwise it might
happen that `b` changes to *null*{: .keyword } after the check.

[ppt]:()

## Safe Calls

safe call operator, written `?.`:

``` kotlin
val result: Int? = b?.length
```

[ppt]:()

chains:
``` kotlin
bob?.department?.head?.name
```
note: 
Such a chain returns *null*{: .keyword } if any of the properties in it is null.

[ppt]:()

[`let`](/api/latest/jvm/stdlib/kotlin/let.html)会忽略null

``` kotlin
val listWithNulls: List<String?> = listOf("A", null)
for (item in listWithNulls) {
     item?.let { println(it) } // prints A and ignores null
}
```

[ppt]:()

## Elvis Operator

``` kotlin
val l: Int = if (b != null) b.length else -1
```
equals to this using ``?:``

``` kotlin
val l = b?.length ?: -1   
```

[ppt]:()

*throw* and *return* 都是表达式:

``` kotlin
fun foo(node: Node): String? {
    val parent = node.getParent() ?: return null
    val name = node.getName() ?: 
        throw IllegalArgumentException("name expected")
    // ...
}
```
[ppt]:()
## The `!!` Operator


``` kotlin
val l = b!!.length
```

[ppt]:()

## Safe Casts

``` kotlin
val aInt: Int? = a as? Int
```

[ppt]:()

## Collections of Nullable Type

``` kotlin
val nullableList: List<Int?> = listOf(1, 2, null, 4)
val intList: List<Int> = nullableList.filterNotNull()
```
