# Type Checks and Casts
[ppt]:()

## `is` and `!is` Operators

``` kotlin
if (obj is String) {
    print(obj.length)
}

if (obj !is String) { // same as !(obj is String)
    print("Not a String")
}
else {
    print(obj.length)
}
```

[ppt]:()

## Smart Casts

The compiler is smart enough

[ppt]:()

``` kotlin
fun demo(x: Any) {
    if (x is String) {
        print(x.length) // x is automatically cast to String
    }
}
```

``` kotlin
    if (x !is String) return
    print(x.length) // x is automatically cast to String
```

``` kotlin
    // x is automatically cast to string on the right-hand side of `||`
    if (x !is String || x.length == 0) return

    // x is automatically cast to string on the right-hand side of `&&`
    if (x is String && x.length > 0) {
        print(x.length) // x is automatically cast to String
    }
```

``` kotlin
when (x) {
    is Int -> print(x + 1)
    is String -> print(x.length + 1)
    is IntArray -> print(x.sum())
}
```

[ppt]:()

注意， 只有编译器认为两句代码之间变量不会发生变化的时候， 才会smart cast

[ppt]:()

## "Unsafe" cast operator

as失败会抛出异常

``` kotlin
//null cast cause exception
val x: String = y as String

//null can be cast to String?
val x: String? = y as String?
```

[ppt]:()

## "Safe" (nullable) cast operator

as? 失败的时候会返回null

``` kotlin
val x: String? = y as? String
```
