## Dependency Injection Explained (with Java)

Dependency injection is a common pattern in object-oriented programming (Java, C#, etc.), but it is one that beginners commonly have problems grasping.<br>
If you're struggling to make sense of the concept (I don't blame you, the term itself is *scary*), I hope this article will provide some help!

---

### WHAT *THE @!#%* IS IT!?

<img align="right" width="400" height="300" src="https://raw.githubusercontent.com/maow-xyz/blog/main/img/dependency_injection/graph.png">

For a simple, one sentence definition, we'll use [James Shore's:](https://www.jamesshore.com/v2/blog/2006/dependency-injection-demystified)
> \[...\] Dependency injection means giving an object its instance variables.

Over the course of this article, I will be using the following terms:
- **Injector** = A class that creates a new instance of a service (instantiation) and passes it to the client (injection).
- **Service**/Dependency = A class that provides some functionality we want to re-use elsewhere.
- **Client**/Dependent = A class that uses a service, and therefore is dependent on it.

View the graph on the right for a simple visualization of the pattern.

### No Singletons or Static Getters!

"Whuh- huh?! What?! What even is that???" I hear many people immediately saying.<br>
Don't worry! I'll break it down for you.

A **singleton** is a similar pattern in which your class only ever has a *single* instance of itself.<br>
A **static getter** is a method that is `static` and returns a value that may or may not also be `static`.

These two patterns are commonly abused by beginners to allow clients access to services.

#### DO NOT DO THIS!

```java
public class CatGenerator {
    private final Object someValue;
    
    public void CatGenerator() {
        // Singleton
        this.someValue = App.getInstance().getSomeValue();
        // *or* Static Getter
        this.someValue = App.getSomeValue();
    }

    // ...
}
```

People have already debated greatly on the reasons why singletons are bad,
so I'll just give you a [good StackOverflow answer.](https://stackoverflow.com/a/138012/15503388)<br>
~~I'm not lazy, what are you talking about?~~

Later in the next section, we'll provide the proper version of the above code.

### Putting An Example To The Pattern

To help acquaint you with this concept, I've written a small example to help:

Our **service.** This is a simple interface with a few common implementations that our client will use.
```java
public interface Fruit {
    void eat();
}

public final class Apple implements Fruit {
    @Override
    public void eat() {
        System.out.println("Cronch!");
    }
}

public final class Banana implements Fruit {
    @Override
    public void eat() {
        // Bananas don't make a sound. Fight me.
    }
}
```

#### ~~High Cholestrol~~ Improper Implementation

Let's write a simple **client,** a person who is in need of ***F R U I T.***

```java
public final class Person {
    public void beginConsumption() {
        final var fruit = new Apple();
        fruit.eat();
        System.out.println("Yummy.");
    }
}
```
`new Person().beginConsumption(); // Outputs "Cronch!" and "Yummy."`

Can you catch the issue here? Well, we're unable to use another kind of `Fruit`!<br>
We have restricted our client to `Apple`, and therefore our code is non-extensible.

#### Now with Dependency Injection! (Low Fat)

```java
public final class Person {
    private final Fruit fruit;
    
    public Person(Fruit fruit) {
        this.fruit = fruit;
    }
    
    public void beginConsumption() {
        fruit.eat();
        System.out.println("Yummy.");
    }
}
```

And here's our **injector!**

```java
public final class App {
    public static void main(String[] args) {
        final var fruit = new Banana();
        final var person = new Person(fruit);
        person.beginConsumption(); // Outputs only "Yummy."
    }
}
```

This allows us much more control over the structure of our program, allowing us to infinitely extend `Person`'s
behaviour so it can handle as many fruits as it wants.

#### Back to the Cats

<!-- Wink wink wink -->
Finally, the *arc* of `CatGenerator` is coming to a close...
```java
public class CatGenerator {
    private final Object someValue;
    
    public CatGenerator(App app) {
        this.someValue = app.getSomeValue();
    }
    
    // ...
}
```

### Dependency Injection Frameworks

The Java *ecosystem* has a set of libraries known as "dependency injection frameworks."<br>
These allow you to further untether your classes from each other by allowing the framework to become the injector.<br>
These frameworks can handle instantiating services, as well as injecting said services into clients on creation/instantiation.

Pros:
- Easy to add new dependencies, just add ~~butter~~ a new parameter to the constructor of your client.
- They give you control over when and how an object is constructed, e.g. *lazy instantiation* (when) and *singleton instances* (how).
- They're easy to use! Most frameworks will just use annotations (`@Inject`, `@Singleton`, [and other JSR-330 annotations](https://jcp.org/en/jsr/detail?id=330)) and a few fluent calls.

Cons:
- For newcomers to your project, it can be hard to track the dependency graph of said project.
- This can complicate API structure ever so slightly, as you will need to document what dependencies can be injected.
- Some frameworks (specifically runtime frameworks like Guice) may introduce small overhead. This, however, is decently negligible.

Commonly used frameworks include:
- [Dagger](https://dagger.dev)
- [Spring](https://spring.io)
- [Guice](https://github.com/google/guice)

### Other Resources

If you're still a bit confused, then unfortunately I have *failed,* but thankfully people smarter than me have some great resources:
- StackOverflow: ["What is dependency injection?"](https://stackoverflow.com/questions/130794/what-is-dependency-injection)
- StackExchange: ["Why do we need frameworks for dependency injection?"](https://softwareengineering.stackexchange.com/questions/300127/why-do-we-need-frameworks-for-dependency-injection)
- James Shore: [Dependency Injection Demystified](http://jamesshore.com/Blog/Dependency-Injection-Demystified.html)
- Martin Fowler: [Inversion of Control Containers and the Dependency Injection pattern](http://martinfowler.com/articles/injection.html)
