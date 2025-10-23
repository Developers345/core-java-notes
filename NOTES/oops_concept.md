## Core concepts and terminology

- **Superclass:** The parent/base class that provides common state and behavior.
- **Subclass:** The child/derived class that extends the superclass and may add/override behavior.
- **extends:** Keyword to establish class inheritance.
- **super:** Reference to the immediate superclass; used to access parent members and constructors.
- **is-a relationship:** Subclass “is a” kind of its superclass; use inheritance only if this holds.

Java uses `extends` for class inheritance and supports single inheritance for classes; interfaces can be multiply implemented. Inheritance enables code reuse and polymorphic behavior via method overriding and dynamic dispatch.

---

## Step 1: Declaring a simple inheritance hierarchy

```java
// Superclass (parent)
class Animal {
    String name;

    public void eat() {
        System.out.println("I can eat");
    }
}

// Subclass (child)
class Dog extends Animal {
    public void bark() {
        System.out.println("Woof!");
    }
}

public class Demo {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.name = "Buddy";   // inherited field
        d.eat();            // inherited method
        d.bark();           // subclass method
    }
}
```

The subclass automatically inherits accessible fields and methods. Use `extends` to form “Dog is-an Animal,” which allows `Dog` to reuse `Animal`’s behavior.

---

## Step 2: Method overriding and dynamic dispatch

Overriding replaces a superclass method with a specialized implementation in the subclass.

```java
class Animal {
    public void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Dog barks");
    }
}

class Cat extends Animal {
    @Override
    public void sound() {
        System.out.println("Cat meows");
    }
}

public class Demo {
    public static void main(String[] args) {
        Animal a1 = new Dog(); // upcasting
        Animal a2 = new Cat();
        a1.sound(); // Dog barks
        a2.sound(); // Cat meows
    }
}
```

- **Dynamic dispatch:** The method called depends on the runtime type (`Dog`, `Cat`), not the reference type (`Animal`).
- **Override rules:** Same method signature; return type may be covariant (a subtype). Access cannot be more restrictive than the superclass method; exceptions must be the same or narrower (checked). Use `@Override` for safety.

---

## Step 3: Using super for parent members and constructors

- **Access parent implementation:** `super.method()` calls the overridden parent method.
- **Call parent constructor:** First statement in a subclass constructor can be `super(args)`.

```java
class Animal {
    protected String name;
    public Animal(String name) { this.name = name; }
    public void describe() { System.out.println("Animal: " + name); }
}

class Dog extends Animal {
    public Dog(String name) { super(name); }

    @Override
    public void describe() {
        super.describe(); // keep base description
        System.out.println("Species: Dog");
    }
}
```

If you don’t call `super(...)`, Java inserts `super()` implicitly. If the parent lacks a no-arg constructor, you must call an available one explicitly.

---

## Step 4: Access control and inheritance visibility

- **public:** Inherited everywhere.
- **protected:** Inherited to subclasses; visible within same package and subclasses (even across packages via subclass reference).
- **default (package-private):** Inherited only within the same package.
- **private:** Not inherited; accessible only within the declaring class.

Modifiers that affect inheritance:
- **final class:** Cannot be extended.
- **final method:** Cannot be overridden.
- **static method:** Hidden (not overridden); dispatch is compile-time.
- **abstract method/class:** Requires subclass to implement or remain abstract.

---

## Step 5: Types of inheritance supported in Java

- **Single inheritance:** A class extends exactly one class.
- **Multilevel inheritance:** A extends B, B extends C.
- **Hierarchical inheritance:** Multiple subclasses extend the same superclass.

Java does not support **multiple inheritance** for classes (to avoid diamond conflicts). Use **interfaces** to implement multiple types.

```java
interface Runner { void run(); }
interface Swimmer { void swim(); }

class Dog extends Animal implements Runner, Swimmer {
    public void run() { System.out.println("Dog runs"); }
    public void swim() { System.out.println("Dog swims"); }
}
```

Interfaces define contracts; classes can implement many. Interfaces can have default methods; conflicts must be resolved explicitly by the implementing class.

---

## Step 6: Upcasting, downcasting, and instanceof

```java
Animal a = new Dog();       // upcasting: safe
a.sound();                  // Dog’s override

if (a instanceof Dog) {     // type check
    Dog d = (Dog) a;        // downcasting: requires explicit cast
    d.bark();
}
```

- **Upcasting:** Reference to a superclass; enables polymorphism but limits visible methods to the reference type.
- **Downcasting:** Regains subclass-specific methods; validate with `instanceof` to avoid `ClassCastException`.

---

## Step 7: Constructors, initialization order, and fields

Initialization order when creating a subclass instance:
1. **Parent static initializers** run.
2. **Child static initializers** run.
3. **Parent instance initializers/fields** run.
4. **Parent constructor** executes.
5. **Child instance initializers/fields** run.
6. **Child constructor** executes.

Fields are not overridden; they’re hidden if redeclared, which is confusing. Prefer unique names and avoid hiding fields.

---

## Step 8: Overriding vs overloading

- **Overriding:** Same signature, different class in hierarchy; enables polymorphism.
- **Overloading:** Same name, different parameters within the same class; chosen at compile-time by parameter types.

```java
class Printer {
    public void print(Object o) { System.out.println(o); }         // overload 1
    public void print(int i)    { System.out.println(i); }         // overload 2
}

class FancyPrinter extends Printer {
    @Override
    public void print(Object o) { System.out.println("[ " + o + " ]"); } // override
}
```

---

## Step 9: Abstract classes vs interfaces

- **Abstract class:** Can hold state and some shared code; cannot be instantiated; subclasses must implement abstract methods.
- **Interface:** Pure type contract; no instance state; supports multiple implementation; default/static methods permitted.

Use an abstract class when you need shared state/template methods; use interfaces for capabilities across disparate hierarchies.

---

## Step 10: Composition vs inheritance

Prefer **composition** (“has-a”) when the relationship isn’t strictly “is-a” or when you want to avoid tight coupling.

```java
class Engine { void start() { /* ... */ } }

class Car {                  // Car has-an Engine (composition)
    private final Engine engine = new Engine();
    void start() { engine.start(); }
}
```

Composition gives better flexibility, testability, and avoids fragile base class problems. Reach for inheritance to model true specialization; otherwise compose.

---

## Step 11: Real-world example with polymorphic processing

```java
abstract class Shape {
    public abstract double area();
}

class Rectangle extends Shape {
    private final double w, h;
    public Rectangle(double w, double h) { this.w = w; this.h = h; }
    @Override public double area() { return w * h; }
}

class Circle extends Shape {
    private final double r;
    public Circle(double r) { this.r = r; }
    @Override public double area() { return Math.PI * r * r; }
}

class Canvas {
    public double totalArea(Shape[] shapes) {
        double sum = 0;
        for (Shape s : shapes) sum += s.area(); // dynamic dispatch
        return sum;
    }
}

public class Demo {
    public static void main(String[] args) {
        Shape[] shapes = { new Rectangle(2, 3), new Circle(1.5) };
        System.out.println(new Canvas().totalArea(shapes)); // polymorphic
    }
}
```

---

## Common pitfalls and best practices

- **Avoid deep inheritance trees:** Favor small, meaningful hierarchies.
- **Don’t expose mutable fields to subclasses:** Use `protected` methods over `protected` state; prefer private fields with controlled access.
- **Document invariants:** If subclasses rely on parent assumptions, state them clearly.
- **Use final where appropriate:** Stabilize APIs; prevent accidental overriding.
- **Beware method contracts:** Overridden methods must honor Liskov Substitution Principle (don’t weaken guarantees).
- **Resolve interface default conflicts:** Explicitly implement to choose or combine behavior.
- **Prefer composition for behavior reuse:** Mixin-like reuse via interfaces + delegation often beats inheritance.

---

## Quick reference table

| Topic | What it means | Key rules | Example |
|---|---|---|---|
| Overriding | Subclass replaces method | Same signature; covariant return; not more restrictive | Dog overrides sound() |
| Access | Visibility in inheritance | public, protected, default, private | protected fields visible in subclass |
| super | Parent access | Call parent methods/constructors | super(name), super.describe() |
| Final | Prevent changes | final class/method cannot be extended/overridden | final String is immutable |
| Interfaces | Multiple types | implements many; default/static methods | Dog implements Runner, Swimmer |

> Sources: 

---

## Memory aids

- **“is-a” test:** If you can say “Dog is an Animal” without awkwardness, inheritance fits.
- **“override oath”:** Same signature; broader visibility; narrower checked exceptions; covariant return.
- **“compose when in doubt”:** If behavior reuse is the goal, prefer composition + interfaces.

---

## Practice tasks

- **Implement a multilevel hierarchy:** `Vehicle -> Car -> ElectricCar`. Override `start()` differently at each level.
- **Polymorphic collection:** Create `Payment` interface with `process()`. Implement `CardPayment`, `UPIPayment`, put them in a list, and process.
- **Override contracts:** Write a base `Repository` with `find(id)` and create a `CachingRepository` that respects original null/exception semantics.

If you want, I can tailor examples to your upcoming test focus areas (e.g., access rules, covariant return, default methods conflicts) or convert this into a printable cheat sheet for quick revision.
