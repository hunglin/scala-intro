# Control Flow
- Everything is an expression.
- Sometimes that expression returns a Unit 



## If / Then Conditionals
```scala
scala> val x = if(1 == 1) "true" else "false"
x: String = true
```

## While / Do Loops

```scala
scala> var i = 0
i: Int = 0

scala> val x = while(i < 2) { println(i); i += 1 }
0
1
x: Unit = ()

scala> do { println(i); i += 1 } while (i < 4)
2
3
```



## Pattern Matching
```scala
scala> val x = 1 match {
     |   case 1 => "true"
     |   case _ => "false"
     | }
x: String = true
```



## For comprehensions
```scala
scala> for (i <- 0 to 1) println(i)
0
1

scala> for (i <- 0 until 1) println(i)
0

scala> for (i <- Seq("aString", "bString")) println(i)
aString
bString

scala> for (a <- Option.empty[String]; b <- Option("bString")) println(a+b) 

scala> for (a <- Option("aString"); b <- Option("bString")) println(a+b)
aStringbString
```



//## Exception Handling
