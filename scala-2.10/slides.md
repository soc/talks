# Scala-Puzzlers

!SLIDE intro
# Scala-Puzzlers

Kriegsgeschichten aus Sprach- und Bibliotheksdesign <br />
*Oder: WTF, wer hat das gemergt?*

<img height="50%" src="images/scala.png"/>
####  Simon Ochsenreither | [simon@ochsenreither.de](mailto:simon@ochsenreither.de)

!SLIDE left top
# Ich

!SLIDE left top
# Um was gehts?

  * **Keine Einführung** in die Sprache …
  * … aber jedes Code-Beispiel wird erklärt
  * Kleine & größere **Probleme beim Design** der …
    * Sprache
    * Standardbibliothek
  * Was kann man daraus lernen?

!SLIDE left top
# Komplexitätsbudget

!SLIDE left top
# Scala

Diagramm StackOverflow/GitHub

!SLIDE left top
# Kapitel

  * Sprache
  * Bibliothek
  * Werkzeuge
  * Prozesse

!SLIDE left top
# Die Ungleichung ...

``` text/x-scala
def add(a: Int, b: Int) { a + b }
println("Ergebnis: " + add(42, 23))
```

!NOTE
# Methoden *und* Prozeduren

  * Methoden:

``` text/x-scala
def add(a: Int, b: Int) = a + b
add(42, 23)
```

  * Methoden ohne Rückgabewert heißen Prozeduren
  * Prozeduren haben den Rückgabetyp `Unit` (~`void`)
  * 3 verschiedene, equivalente Deklarationmöglichkeiten

``` text/x-scala
def printHello1() { println("Hello") }
def printHello2() = println("Hello")
def printHello3(): Unit = { println("Hello") }
```

!SLIDE left top
# Abstrakte Methoden in Traits

  * Wenn der Rückgabewert nicht inferiert werden kann, muss er angegeben werden

``` text/x-scala
trait Foo {
  def foo(): Unit
}

trait Bar {
  def bar()
}
```


!SLIDE left top
# Methoden *und* Operatoren

  * Fast freie **Methodenbenennung**
  * Methoden mit einem Argument können **Operator-Notation** nutzen

``` text/x-scala
val three: Int = 1 + 2
def hello = "Hello GPN".take(5)
```

``` text/x-scala
val three: Int = 1.+(2)
def hello = "Hello GPN" take 5
```

!SLIDE left top
# Die Sache mit dem **Doppelpunkt** …
  * Wenn eine Methode mit `:` endet, …
  * … dann ist sie **rechts**assoziativ …
  * … aber nur in **Operator-Notation**!

``` text/x-scala
val list1 =  1  :: 2  :: 3  :: Nil

val list2 = Nil.::(3).::(2).::(1)

assert(list1 == list2)
```
!SLIDE
# Die Sache mit dem **Doppelpunkt** …

<img height="100%" src="images/munch-schrei.jpg"/>

!SLIDE left top
# Oktale Literale

``` text/x-scala
val bond = 007
```

``` text/x-scala
val abc = "\\141\\142\\143"
```

!SLIDE left top
# Java?

``` text/x-scala
// Note: \\u000A is Unicode for linefeed (LF)
val linefeed = "\\u000A"
print(linefeed)
```

!SLIDE left top conclusion
# Fazit: Weniger ist mehr!

!SLIDE left top
# Statische Methoden


!SLIDE left top
# Konstruktoren und Extraktoren

  * `unapply`

``` text/x-scala
val (one, two) = (1, 2)
```

``` text/x-scala
val 1 = 2
```

``` text/x-scala
case class Person(firstName: String,
                  lastName:  String)

val joe = Person("Joe", "Gulasch")
val Person(firstName, _) = joe
```

!SLIDE left top
# Optimistische APIs

 * Was passiert, wenn man vergisst das Verhalten im Fehlerfall zu spezifizieren?
 * ListBuffer? (Rekursives off-by-one)

!SLIDE left top
# Optimistische APIs (Multi-threaded version)

 * CompositeThrowable

``` text/x-scala

def failOnce(i: Int) =
  if (i == 42) throw new RuntimeException(s"Failed: $i")
  else i
(1 to 1000).map(failOnce)
```

``` text/x-scala
def failOften(i: Int) =
  if (i % 42 == 0) throw new RuntimeException(s"Failed: $i")
  else i
(1 to 1000).map(failOften)
```

!SLIDE left top
# Optimistische APIs

!SLIDE left top
#Testing/Dokumentation/Reflection

`scala.mobile`

!SLIDE left top
#Doku ≠ Doku


``` text/x-scala
/** A basic pretty-printing library, based
  * on Lindig's strict version of Wadler's
  * adaptation of Hughes' pretty-printer.
  */
abstract class Document
```

``` text/x-scala
/** This class turns a regular expression
  * over `A` into a `NondetWordAutom` over
  * `A` using the celebrated position
  * automata construction (also called 
  * ''Berry-Sethi'' or ''Glushkov'').
  */
abstract class BaseBerrySethi
```

!SLIDE left top
#Sprache -> Bibliothek

classOf
isInstanceOf
asInstanceOf


!SLIDE left top
#Kompatibilität

  * **Code-** oder **Binär**kompatibilität?

  * PHP: 
  * C++: 
  * Java: Never change anything ever!
  * C#: 
  * Python: “where code goes to die”

  * Mit der Zeit stabiler oder weniger stabil?
  * Xamarin
  * MiMa

!SLIDE left top
# Scala Improvement Process

  * Je nach Größe der Änderung/Verbesserung sind manchmal verschiedene Prozesse notwendig

  * String Interpolation ([SIP-11](https://docs.google.com/document/d/1NdxNxZYodPA-c4MLr33KzwzKFkzm9iW9POexT9PkJsU/edit?hl=en_US))
  * Implicit Classes ([SIP-13](http://docs.scala-lang.org/sips/pending/implicit-classes.html))
  * Futures and Promises ([SIP-14](http://docs.scala-lang.org/sips/pending/futures-promises.html))
  * Value Classes ([SIP-15](https://docs.google.com/document/d/10TQKgMiJTbVtkdRG53wsLYwWM2MkhtmdV25-NZvLLMA/edit?hl=en_US))
  * Macros (experimental, [SIP-16](http://docs.scala-lang.org/overviews/macros/overview.html))
  * trait Dynamic ([SIP-17](https://docs.google.com/document/d/1XaNgZ06AR7bXJA9-jHrAiBVUwqReqG4-av6beoLaf3U/edit))
  * Feature Imports ([SIP-18](https://docs.google.com/document/d/1nlkvpoIRkx7at1qJEZafJwthZ3GeIklTFhqmXMvTX9Q/edit))
  * Reflection (experimental, [overview](http://docs.scala-lang.org/overviews/reflection/overview.html))

  * Rewritten Pattern Matcher

!SLIDE
# Thanks!

!SLIDE
# Questions!



