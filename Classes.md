# Classes, Interfaces, Inheritance, and Objects


As an introductory example let's convert the follow Java class to Kotlin: 

```java
public final class Person {

    private static final String DEFAULT_PROFESSION = "slacker";

    private final String name;
    private String profession;

    public Person(@NotNull String name) {
        this(name, DEFAULT_PROFESSION);
    }

    public Person(@NotNull String name, @NotNull String profession) {
        this.name = name;
        this.profession = profession;
    }

    @NotNull
    public final String getName() {
        return name;
    }

    @NotNull
    public final String getProfession() {
        return profession;
    }

    public final void setProfession(@NotNull String profession) {
        this.profession = profession;
    }

    public final void print() {
        System.out.println(name + ":" + profession);
    }
}
```

The result is surprisingly concise:  

```kotlin
class Person(val name: String, var profession: String = Person.DEFAULT_PROFESSION) {

    fun print() {
        println(name + ":" + profession)
    }

    companion object {
        const val DEFAULT_PROFESSION = "slacker"
    }
}
```

A couple interesting things to observe from this example:

* The default visibility in Java is _package private_, whereas in Kotlin it is _public_. That is the reason why all the
occurrences of the`public` keyword were omitted in the Kotlin code.
* In Kotlin, all classes and methods **are _final_ by default**. That is why you see so many `final`s in the Java version.
* Like Swift, Kotlin has _properties_ instead of _fields_. Thus you do not need to define _setters_ and _getters_ if they are trivial. Kotlin tries to be so succint that you can define the primary constructor of the class in the class' body. Moreover you can reuse the parameters of the constructor to define properties by prefixing them with `var` and `val`.
* The constructor's parameters can have default values, so you do not need to overload it.
* Kotlin classes do not have static fields. However you can associate an object &mdash; the companion object &mdash &mdash; with a class.

Creating an object of the class `Person` is simple You don't use the `new` keyword: 
```kotlin
val genius = Person("Albert Enstein", "Theoretical Physicist")
```

## Properties

Kotlin has properties. So behind the curtains it creates a backing field and generates trivial _setters_ and _getters_. Properties must always be initialized even if they are nullable.

```kotlin
class Rectangle(width: Int, height: Int) {
    var width: Int = width
    var height: Int = height
    var name: String? = null 
}
```
Properties can be accessed directly using dot notation: 

```kotlin
val square = Rectangle(2, 2)
square.name = "Square"
println("This ${square.name} is ${square.width}x${square.height}")
```
Mutable properties are declared using `var` and read-only ones are declared using `val`:
```kotlin
class FooBar(foo: Int, bar: Int) {
    var foo = foo
    val bar = bar
}

fun main(args: Array<String>) {
    val foobar = FooBar(1, 2)
    foobar.foo = 3
    foobar.bar = 4 //Error:(15, 5) Kotlin: Val cannot be reassigned
}
```

You can also define the propeties in the constructor: 
```kotlin
class Foobar(var foo:Int, val bar: Int)
```

### Custom setters and getter and computed properties

If the trivial _setter_ and _getter_ pair does not suffice you can define customs ones. The backing field for the property can be accessed using the `field` identifier.

```kotlin
class Square(length: Int) {
    var side = length
        get() {
            println("getting side ")
            return field
        }
        set(value) {
            if (value != field) {
                println("Square changed side length")
            }
            field = value
        }
}
```
You can have computed properties by defining custom _getters_:
```kotlin
class Square(var side: Int) {
    val area 
        get() = side * side
    val perimeter
        get() = 4 * side
}
```

### Accessing properties from Java

Kotlin properties can be accessed from Java using the JavaBeans notation, id est, `set` prefix for _setters_ and `get` prefix for _getters_.

```java
Square square = new Square(4);
square.setSide(8);
System.out.println(String.format("The square has side %d, area %d and perimeter %d.", 
                square.getSide(),
                square.getArea(),
                square.getPerimeter()));
```

If the property has an _is_ prefix, then it is a bit different : 

_Kotlin_
```kotlin
class Dog(var isHappy: Boolean)
```
_Java_
```java
final Dog dog = new Dog(false);
dog.setHappy(true);
System.out.println ("The dog is " + (dog.isHappy() ? "happy." : "not happy.");
```

## Methods

Kotlin methods are very similar to top level functions. If necessary you can use `this` like in Java to refer to the local instance.

```kotlin
class Rectangle(var width: Int, var height: Int) {
    fun set(width: Int, height: Int) {
        this.width = width
        this.height = height
    }
    
    fun calculateArea(): Int = width * height
}
```

### Extensions Methods and Properties

You can "add" new methods and properties to existing classes even if you do not have access to their souce code. Let's add a `capitalize()` method and a `isPalindrome` property to `String`.

```Kotlin
fun String.capitalize() = substring(0, 1).toUpperCase() + substring(1)
val String.isPalindrome
    get() = this == this.reversed()

fun main(args: Array<String>) {
    println("hello world!".capitalize()) //Hello world!
    println("Foobar is a palindrome: ${"Foobar".isPalindrome}") //false
    println("Radar is a palindrome: ${"radar".isPalindrome}") // true
}
```
Beware that extensions on Kotlin are pure syntactic sugar. You are not actually adding any new methods or properties to the receiver class. You only get the convenience of using the extensions as if they were defined by the receiver class. Therefore extensions have access only to public members of the receiver class. Extension properties have no backing field. That is why the `String.isPalindrome` has to be defined thru a custom _getter_.

## Visibility modifiers  

## Constructors and initialization blocks

## Enum classes

## Data Classes

## Nested and Inner classes
A nested class is a class defined inside another class, but it is independent of its outer class. On the other hand an inner class is also a class defined inside another class, but it has a implicit reference to its outer class and previleged access to its private members. In Java inner classes are the default whereas in Kotlin nested classes are the default.

_Java_
```Java
public class Outer {
    private int counter = 0;
    
    public static class Nested {
        public int foo;
    }
    
    public class Inner {
        public Outer increment() {
            counter++;
            return Outer.this;
        }
    }
}
```

_Kotlin_
```kotlin
class Outer {
    private var counter = 0

    class Nested {
        var foo: Int = 0
    }

    inner class Inner {
        fun increment(): Outer {
            counter++
            return this@Outer
        }
    }
}
```

Also note the different syntax to access the instance of the outer class `this@Outer`.


