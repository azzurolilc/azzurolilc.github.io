#Scala, Play Framework and Akka

##Scala

####Variables and Values
**variables:**

```
var a = 1
```

**values:**

```
val b = 1
```


####Functions

**Lambda Function**



**Closure**



**Partial Function**

Must use case

NullPointException handling: 
```
function(val).getOrElse('default')
```

**Higher-order Function**
import, here _ equal to wildcard, all methods in foo.bar
```
import foo.bar._ 
```


####Scala Collections

In Scala, by default all collections are immutable.

Conversions Between Java and Scala Collections:
init scala list:
```
val list = List(1,2,3,4)
```
```
//convert to java:
val javaList = list.asJava
```
convert to scala:
```
//One extra step: scalaList => scala.mutable.Buffer => List
val scalaList = list.asScala.toList 
```

**Traits and Classes**


Trait is similar to Interfaces in Java.
Scala allow multiple inheritance on traits.
Traits runs order: trait linearization.
```

abstract class Language {
  val name: String
  override def toString = name
}


trait JVM {
  override def toString = super.toString + " runs on JVM"
}

trait Static {
  override def toString = super.toString + " is Static"
}


class Scala extends Language with Static with JVM {
  override val name: String = "Scala"
}


println(new Scala)


```


case class is immutable
```
case class Car(brand:String)
// apply: new alternative?
val car1 = Car('BMW')
 
//unapply
val car2 = new Car('BMW')

```


###Advanced Features

#####Case Class and Pattern Match

pattern match: value and variable type, class
```
var x = 1
value match {
    case string : String => string
    case num : Int => { num match{
                            case 1 => 'One'
                            case 2 => 'Two'
                            case 3 => 'Three'
                            }
                        }
    case doubleNum : Double => doublenum+1
    case _ : 'Other thing'  
}

trait Animal
case class Cat(name:String) extends Animal
case class Dog(name:String) extends Animal
case class Chicken(name:String) extends Animal

def checkAnimal(animal:Animal) = animal match{
    case Dog(name) => s"This dog name is $name"
    case Cat(_) => s"This cat is cute"
    case _ => "Other animals"
}
```

#####Futures


```
package Future

import java.util.concurrent.TimeUnit
import scala.concurrent.ExecutionContext.Implicits.global // thread pool
import scala.concurrent.duration._
import scala.concurrent.{Await, Future}
import scala.util.{Failure, Success}

/**
  * Created by dylan on 6/5/16.
  */
object MultipleFutureDemo {
  def main(args: Array[String]): Unit = {
    val f1 = Future {
      TimeUnit.SECONDS.sleep(2)
      65 + 54
    }

    val f2 = Future {
      TimeUnit.SECONDS.sleep(2)
      1 / 0
    } recover {
      case e: ArithmeticException => 0
      case e: RuntimeException => 0
    }

    val f3 = Future {
      TimeUnit.SECONDS.sleep(3)
      23 * 42
    }

    // calculate sum of f1, f2, f3

    val f4 = for {
      r1 <- f1
      r2 <- f2
      r3 <- f3
    } yield r1 + r2 + r3
  }
}
```


#####Implicit



Implicit: within current Scope
```
package Implicit

class Duck {
  def makeDuckNoise() = "gua gua"
}

class Chicken {
  def makeChickenNoise() = "ge ge"
}

class Ducken(chicken: Chicken) extends Duck {
  override def makeDuckNoise() = chicken.makeChickenNoise()
}

object ImplicitDemo extends App {
  implicit def chickenToDuck(chicken: Chicken) = new Ducken(chicken)

  def giveMeADuck(duck: Duck) = duck.makeDuckNoise()

  println(giveMeADuck(new Duck))
  println(giveMeADuck(new Ducken(new Chicken)))
  println(giveMeADuck(new Chicken))
}
```
More features



###Pitfalls

**Heavy in concepts and keywords** Implicits, for comprehensions, lazy, case class, partially applied functions, :/, ~>, :: etc.

**Steep learning curve**

**Coding style: functional v.s. Non-functional**

**Syntactic Diabetes** same function, too many ways of expression

Too many ways to write the same thing 
Short v.s. readable
Advanced features getting in the way

Solution: Style Guides



**Backward Compatibility Issues**











##Play Framework
-----
companies uses play framework: twitter, linkedin, coursera

###Akka



###Play



JVM => Akka => Play => JBoss/Nginx

MVC

Activator: tutorials

play swagger interactive api
Scala community Scala Center



