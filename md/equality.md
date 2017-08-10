# Equality
[ppt]:()

In Kotlin there are two types of equality:

* Referential equality (two references point to the same object)
* Structural equality (a check for `equals()`)

[ppt]:()

## Referential equality

* ===
* !==

[ppt]:()

## Structural equality

* ==
* !=

``` kotlin
a == b
//等价于
a?.equals(b) ?: (b === null)
```
note:
I.e. if `a` is not `null`, it calls the `equals(Any?)` function, otherwise (i.e. `a` is `null`) it checks that `b` is referentially equal to `null`.


