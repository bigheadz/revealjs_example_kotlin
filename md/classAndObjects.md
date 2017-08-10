# Kotlin

Swift of Android

[ppt]:()

## Data classes
仅仅用于封装、保存数据用的data class
```
    data class User(val name: String, val age: Int)
```
[ppt]:()

编译器默认为你实现了下面的成员方法:

- equals()/hashCode()
- toString() ```"User(name=John, age=42)"```
- componentN() ```corresponding to the properties in their order of declaration```
- copy()(see below) 

> 前提是对应的属性声明在主构造器中。
如果上面的函数在class声明， 编译器将不会生成对应的方法。
[ppt]:()

为了使DataClass有意义：

- 至少有一个参数
- 参数带有val / var
- != abstract, open, sealed or inner

```
data class User(val name: String = "", val age: Int = 0)
```
[ppt]:()

### Copying

拷贝一个data class， 并改变其中部分：
```
fun copy(name: String = this.name, age: Int = this.age) = User(name, age)
```
This allows us to write:
```
val jack = User(name = "Jack", age = 1)
val olderJack = jack.copy(age = 2)
```
[ppt]:()

### Destructuring Declarations


```
val jane = User("Jane", 35) 
val (name, age) = jane
println("$name, $age years of age") // prints "Jane, 35 years of age"
```

[ppt]:()

## Sealed Classes

用于指定变量属于有限种类的类.
Sealed class的子类必须在同一个文件内声明。
```
sealed class Expr
data class Const(val number: Double) : Expr()
data class Sum(val e1: Expr, val e2: Expr) : Expr()
object NotANumber : Expr()
```
> 注意： 继承sealed class子类的类， 可以声明到任何地方， 不受同文件的限制.

[ppt]:()

Sealed classes的好处, ``when expression``:
```
var expr: Expr = ...
when(expr) {
    is Const -> expr.number
    is Sum -> eval(expr.e1) + eval(expr.e2)
    NotANumber -> Double.NaN
    // the `else` clause is not required 
    // because we've covered all the cases
}
```

[ppt]:()

## Generics

```
class Box<T>(t: T) {
    var value = t
}
```
```
val box: Box<Int> = Box<Int>(1)
//如果类型可以被推测， 那么可以省略
val box = Box(1)
```

[ppt]:()

### Variance
 
Kotlin没有`` wildcard types``， 有:
* declaration-site variance
* type projections

[ppt]:()

## Nested Classes
```
class Outer {
    private val bar: Int = 1
    class Nested {
        fun foo() = 2
    }
}

val demo = Outer.Nested().foo() // == 2
```
[ppt]:()

### Inner classes

用 ```inner``` 标识， 持有一个外部类对象的引用。
```
class Outer {
    private val bar: Int = 1
    inner class Inner {
        fun foo() = bar
    }
}

val demo = Outer().Inner().foo() // == 1
```

[ppt]:()

### Anonymous inner classes

使用``object expression``创建匿名内部类：
```
window.addMouseListener(object: MouseAdapter() {
    override fun mouseClicked(e: MouseEvent) {
        // ...
    }
                                                                                                            
    override fun mouseEntered(e: MouseEvent) {
        // ...
    }
})
//If the object is an instance of a functional Java interface 
//(i.e. a Java interface with a single abstract method)
val listener = ActionListener { println("clicked") }
```
[ppt]:()

## Enum Classes

```
enum class Direction {
    NORTH, SOUTH, WEST, EAST
}
```
每个枚举常量都是一个对象.
[ppt]:()
## Objects
```kotlin
window.addMouseListener(object : MouseAdapter() {
    override fun mouseClicked(e: MouseEvent) {
        // ...
    }

    override fun mouseEntered(e: MouseEvent) {
        // ...
    }
})
```
[ppt]:()
```kotlin
open class A(x: Int) {
    public open val y: Int = x
}

interface B {...}

val ab: A = object : A(1), B {
    override val y = 15
}
```
[ppt]:()
## Delegation
```kotlin
interface Base {
    fun print()
}

class BaseImpl(val x: Int) : Base {
    override fun print() { print(x) }
}

class Derived(b: Base) : Base by b

fun main(args: Array<String>) {
    val b = BaseImpl(10)
    Derived(b).print() // prints 10
}

```
[ppt]:()
## Delegated Properties

```kotlin
class Example {
    var p: String by Delegate()
}

class Delegate {
    operator fun getValue(thisRef: Any?, property: KProperty<*>): String {
        return "$thisRef, thank you for delegating '${property.name}' to me!"
    }
 
    operator fun setValue(thisRef: Any?, property: KProperty<*>, value: String) {
        println("$value has been assigned to '${property.name} in $thisRef.'")
    }
}

```
[ppt]:()