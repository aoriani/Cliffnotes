Classes, Interfaces, Inheritance, and Objects
=======

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

The result is suprisingly concise:  

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

* The default visibility in Java is package private, whereas in Kotlin it is public. That is the reason why all the occurrences 
of the`public` keyword were omitted in the Kotlin code.
* In Kotlin all classes and methods are final by default. That is why you see so many `final`s in the Java version. 

