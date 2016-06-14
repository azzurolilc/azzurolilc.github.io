#Scala, Play Framework and Akka

Based on [Bittiger-Scala-Play-Framework](https://drive.google.com/file/d/0B1ocrybUr2qWN0dURk9BcjZuQlU/view)

@astray1988  [CodeDemo&ppt](https://github.com/astray1988/scala-talk-bittiger)

##Scala

Static Language, strongly typed, runs on JVM, OOP combine Functional

####Variables and Values
**variables:**

```
// mutable, strongly typed
var x: Int = 1
```

**values:**

```
// immutable
val y: Int = 1
```


####Functions

**Lambda Function**
```
def y(x:Int) = x+1
def y = (x:Int) => x+1

// anonymous function
(x:Int) => x+1
```


**Closure**
```
var x = 1
def fn(y:Int) = x+y

fn(1)
//Int = 2

x = 3
fn(1)
//Int = 4
```


**Partial Function**

Must use case, error check @compile 

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

####Traits and Classes


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
#####More features



###Pitfalls

1. Heavy in concepts and keywords Implicits, for comprehensions, lazy, case class, partially applied functions, :/, ~>, :: etc.

2. Steep learning curve

3. Coding style: functional v.s. Non-functional

4. Syntactic Diabetes same function, too many ways of expression

	- Too many ways to write the same thing 

	- Short v.s. readable

	- Advanced features getting in the way

	- Solution: Style Guides

5. Backward Compatibility Issues




##Play Framework

Full-stack framework, scalable, asynchronous, fast, runs on JVM

Build System Console: SBT | JVM => Akka/db => Play Framework => JBoss/Nginx

Companies uses play framework: twitter, linkedin, coursera etc...

###Akka

Concurrent, Distributed, Fault-Tolerant, Background Processing

###Play
Support both Scala and Java, support JSON, JVM libaries, Angular etc.

Template Engine, Http request/response peocessing, Integrated Cache, RESTful Routing Engine, Asset Compilation, Internationalization, Testing Tools

Structure: MVC

> === Result Helper in Play ===
>
> Ok //200
>
> NotFound (h1:pageNotFound) //404
>
> BadRequest(view,html.form(formWithErrors)) //400
>
> InternalServerError('Oops') //500
>
> Redirect('targetURL') //302

[Play Framework Site](https://www.playframework.com)

[Play Package Manager: Activator](http://www.lightbend.com)
```
// In terminal
> activator new <app name>

// Unit Test
> sbt test

```
[Play Demo @DY](https://github.com/astray1988/scala-talk-bittiger/tree/master/play-demo)

Play Swagger : Interactive API

Community:  scalacenter, MOOC courses etc.

Conference: Scala Days, akka, play, spark

[Scala Reference](https://github.com/astray1988/scala-reference-CN)


