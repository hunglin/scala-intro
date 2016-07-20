# Implicits


## Why implicit?
### How do you extend third party libraries to meet your specific coding requirements?
* Ruby has modules, Smalltalk let packages add to each other's classes. It's powerful but dangerous since the changes are global
* C# 3.0 has static extension methods => more local and more restrictive, and safer
* Scala: using implicit conversions and parameters to avoid tedious and boilerplate details.


## Implicit Rules
Need `implicit` keyword:
```tut:silent
implicit def convert(x: Double) = x.toString
```

An inserted implicit conversion must be in scope as a single identifier
```tut
object MyConversions {
  implicit def convert1(x: String): Int = x.size
  implicit def convert2(x: Float): Int = x.toInt
}
val x: Int = "123"
import MyConversions.convert1 // has convert1 in scope
val x: Int = "123"
import MyConversions._ // has both in scope, but dangerous
```


, or be associated with the source or target type of the conversion
```tut:silent
class Euro { ??? }
class Dollar {
  // toEuro() is in scope
}
object Dollar {
  implicit def toEuro(x: Dollar): Euro = ???
}

```

One at a time
```
compiler will never do convert1(convert2(x))
```
Explicit first


## Implicit Conversion
Compiler does the trick: When compiler sees a type error, it looks for implicit conversions (therefore, increase the compilation time)
```tut
val x: Int = 1.9
```
Converting an expected type:
```tut
implicit def convert1(x: Double) = x.toInt
val x: Int = 1.9
```
Converting the receiver:
```tut
(1 to 4).foreach(print)
Map("a" -> 1, "b" -> 2)
```


## Implicit Classes
```tut
case class Rectangle(width: Int, height: Int)
implicit class RectangleMaker(width: Int) {
  def x(height: Int) = Rectangle(width, height)
}
val myRectangle = 3 x 4
```
```
// Automatically generated by compiler
implicit def RectangleMaker(width: Int) = new RectangleMaker(width)
```


## Implicit Parameters
```tut
def maxList[T](elements: List[T])(implicit ordering: Ordering[T]): T =
  elements match {
    case List() =>
      throw new IllegalArgumentException("empty list!")
    case List(x) => x
    case x :: rest =>
      val maxRest = maxList(rest)(ordering)    // (ordering) is implicit
      if (ordering.gt(x, maxRest)) x           // ordering is explicit
      else maxRest
}
```


implicitly[]
```tut
def maxList[T](elements: List[T])(implicit foo: Ordering[T]): T =  // name doesn't matter
  elements match {
    case List() =>
      throw new IllegalArgumentException("empty list!")
    case List(x) => x
    case x :: rest =>
      val maxRest = maxList(rest)                        // (ordering) is gone
      if (implicitly[Ordering[T]].gt(x, maxRest)) x      // use implicitly[]  
      else maxRest
}
```


context bound: less code is better
```tut
def maxList[T : Ordering](elements: List[T]): T =
  elements match {
    case List() =>
      throw new IllegalArgumentException("empty list!")
    case List(x) => x
    case x :: rest =>
      val maxRest = maxList(rest)
      if (implicitly[Ordering[T]].gt(x, maxRest)) x
      else maxRest
}
```



## Implicit Search
## TypeClasses
## Contexts