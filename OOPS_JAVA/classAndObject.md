# Learn Java OOP by Building a Driver Class

## Agenda

1. [Find the entities and understand abstraction](#1-find-the-entities-and-understand-abstraction)
2. [Design a class: data and behavior](#2-design-a-class-data-and-behavior)
3. [Create objects and understand state](#3-create-objects-and-understand-state)
4. [Understand reference variables](#4-understand-reference-variables)
5. [Give the driver behavior](#5-give-the-driver-behavior)
6. [Understand defaults and constructors](#6-understand-defaults-and-constructors)
7. [Pass values into a constructor](#7-pass-values-into-a-constructor)
8. [Use this to identify the current object](#8-use-this-to-identify-the-current-object)
9. [Overload and chain constructors](#9-overload-and-chain-constructors)
10. [Copy an object](#10-copy-an-object)
11. [Share a counter with static](#11-share-a-counter-with-static)
12. [Use static methods and static blocks](#12-use-static-methods-and-static-blocks)
13. [Print useful information with toString](#13-print-useful-information-with-tostring)
14. [Understand Java pass-by-value](#14-understand-java-pass-by-value)
15. [Protect state with encapsulation](#15-protect-state-with-encapsulation)
16. [Run the complete program](#16-run-the-complete-program)
17. [Practice and check your understanding](#17-practice-and-check-your-understanding)

## 1. Find the entities and understand abstraction

Before writing a class, think about the system you want to build.

An **entity** is a person, place, thing, or concept that matters to your program. Examples include Student, Car, BankAccount, Book, ShoppingCart, Flight, Hospital, and Product.

For a ride-booking application, we might begin with five entities:

- Customer: the person requesting a ride.
- Driver: the person accepting and completing a ride.
- Vehicle: the vehicle used for the journey.
- Trip: the journey itself.
- Payment: the transaction for that journey.

Does a real ride-booking system contain only these five entities? No. We choose a useful starting view so that we can understand and build the system step by step.

This introduces **abstraction**: focusing on relevant details while hiding unnecessary complexity.

### An online-class example

From a student's point of view:

```text
Log in → Open dashboard → Join class
```

Behind these steps, the system may verify identity, check permissions, establish network connections, and stream audio and video. The student does not need to manage those details to join a class.

A good interface gives us meaningful operations while keeping their internal work behind them.

### Where the OOP pillars fit

Abstraction, encapsulation, inheritance, and polymorphism are commonly described as the four pillars of OOP. Abstraction can also be understood as an overall goal supported by encapsulation, inheritance, and polymorphism.

This tutorial concentrates on classes, objects, and encapsulation. Inheritance and polymorphism can follow once these foundations are comfortable.

## 2. Design a class: data and behavior

Would you begin building a house without a plan? Usually, you first create a blueprint.

A **class** plays a similar role: it describes what its objects contain and what they can do.

In a college-management system, a student might have a name, phone number, and graduation year. A student might also join a class, solve assignments, give a contest, and attend a mock interview.

That gives us two questions for any class:

1. What information should it hold?
2. What operations should it offer?

### Apply the recipe to Driver

Create a file named `Driver.java`:

```java
public class Driver {
    int driverId;
    String name;
    double rating;
    boolean isOnline;
}
```

Here, the **fields**, also called member variables, describe each driver's data:

- `driverId` identifies the driver.
- `name` stores the driver's name.
- `rating` stores a rating.
- `isOnline` records whether the driver is available online.

We will later add `acceptRide()` and `changeStatus()` as **member methods**.

For now, these fields have package access because no access modifier is written. That lets our client access them when both files are in the same package. Later, we will make them private and explain why.

## 3. Create objects and understand state

A class describes a kind of thing. An **object** is an instance of that class created while the program runs.

Create a second file, `Client.java`:

```java
public class Client {
    public static void main(String[] args) {
        Driver d1 = new Driver();
        Driver d2 = new Driver();

        d1.driverId = 143;
        d1.name = "Ashok";
        d1.rating = 4.5;
        d1.isOnline = true;

        d2.driverId = 144;
        d2.name = "Ravi";
        d2.rating = 4.2;
        d2.isOnline = false;

        System.out.println(d1.name);
        System.out.println(d2.name);
    }
}
```

Output:

```text
Ashok
Ravi
```

Each object has its own instance fields. Changing `d1.name` does not change `d2.name`.

An object's **state** is the collection of its field values at a particular moment.

```text
Object A: driverId=143, name="Ashok", rating=4.5, isOnline=true
Object B: driverId=144, name="Ravi",  rating=4.2, isOnline=false
```

A class definition does not itself create an individual driver. It still has runtime metadata, and static fields have storage, but instance fields belong to individual objects.

## 4. Understand reference variables

Read this statement in three parts:

```java
Driver d1 = new Driver();
```

- `Driver` is the variable's reference type.
- `d1` is a reference variable.
- `new Driver()` creates a new object and invokes its constructor.
- The resulting reference value is assigned to `d1`.

A useful conceptual picture is:

```text
d1 ───→ Driver object A
```

The reference is a way to identify an object. You do not need to treat it as a directly accessible memory address.

### Declaring a variable is different from creating an object

```java
Driver driver;
```

This local declaration does not create a `Driver` object. You must assign a value before reading the local variable.

```java
Driver driver = null;
```

Now the variable has a value, but it refers to no object. Calling an instance method through it would cause a `NullPointerException`.

### Two references can identify the same object

```java
Driver alias = d1;
alias.name = "xyz";
System.out.println(d1.name); // xyz
```

```text
d1    ──┐
        ├──→ Driver object A
alias ──┘
```

Assigning `d1` to `alias` copies the reference value. It does not copy the object. This is called **aliasing**.

## 5. Give the driver behavior

Add these methods inside `Driver`:

```java
public void acceptRide(String rideId) {
    System.out.println("Ride has been accepted " + rideId);
}

public void changeStatus() {
    isOnline = !isOnline;
    System.out.println("Driver is " + isOnline);
}
```

Call them after setting `d1.isOnline` to `true`:

```java
d1.acceptRide("RIDE-101");
d1.changeStatus();
d1.changeStatus();
```

Output:

```text
Ride has been accepted RIDE-101
Driver is false
Driver is true
```

`!` reverses a boolean value. Therefore, each call to `changeStatus()` changes the object's state.

In this small example, `acceptRide()` only prints a message. A larger implementation could store an assigned ride and check whether the driver is available.

## 6. Understand defaults and constructors

So far, we have not written a constructor. Why does this still work?

```java
Driver d1 = new Driver();
```

**If a class declares no constructors, Java supplies a default constructor.** For our public class, this supplied constructor is public and takes no arguments.

Instance fields receive default values during object creation:

```text
int       → 0
String    → null
double    → 0.0
boolean   → false
```

These defaults apply to fields. Local variables do not automatically receive usable default values.

The following example illustrates the initial state:

```java
// An illustration of the field values, not the literal code Java inserts.
public Driver() {
    driverId = 0;
    name = null;
    rating = 0.0;
    isOnline = false;
}
```

Field default initialization and the compiler-supplied constructor are separate mechanisms.

### Write your own no-argument constructor

Replace the illustrative version with these starting values:

```java
public Driver() {
    driverId = 100;
    name = "Ashok";
    rating = 5.0;
    isOnline = true;
}
```

Now:

```java
Driver driver = new Driver();
System.out.println(driver.name);   // Ashok
System.out.println(driver.rating); // 5.0
```

For this simple class, think of creation as:

```text
Allocate object storage
    → fields receive default values
    → superclass construction takes place
    → instance initializers run, if present
    → this constructor's assignments run
    → the new expression produces the object's reference
```

A constructor has the same name as its class and has no return type. Even `void` would be wrong here: `public void Driver()` would declare a method.

The constructor above is a **programmer-written no-argument constructor**. The term **default constructor** specifically refers to the one Java supplies.

Once you declare any constructor, Java stops supplying a default constructor. If you only declare `Driver(String name)`, then `new Driver()` will not compile unless you also write a no-argument constructor.

## 7. Pass values into a constructor

Our no-argument constructor gives every new driver the same starting data. How can we create different drivers directly?

Use parameters:

```java
public Driver(int id, String n, double r, boolean isOn) {
    driverId = id;
    name = n;
    rating = r;
    isOnline = isOn;
}
```

Create a driver with specific values:

```java
Driver d5 = new Driver(143, "Ashok", 4.5, true);
System.out.println("Debug");
System.out.println(d5.driverId);
```

Output:

```text
Debug
143
```

The constructor finishes before execution reaches the `Debug` print. The arguments are matched to parameters by position.

Both the no-argument and four-argument constructors can exist together. This is **constructor overloading**: several constructors with different parameter lists.

## 8. Use this to identify the current object

The first version used short parameter names: `id`, `n`, `r`, and `isOn`. More descriptive names are easier to read:

```java
public Driver(int driverId, String name,
              double rating, boolean isOnline) {
    this.driverId = driverId;
    this.name = name;
    this.rating = rating;
    this.isOnline = isOnline;
}
```

In this assignment:

```java
this.name = name;
```

`this.name` means the field belonging to the object currently being initialized. The plain `name` means the parameter.

Without `this`:

```java
name = name;
```

Both names refer to the parameter, so the object's field is not updated.

`this` is also available inside instance methods. A static method has no current instance, so it cannot use `this`.

## 9. Overload and chain constructors

Suppose we sometimes know only a driver's name. A shorter constructor provides sensible starting values for the other fields:

```java
public Driver(String name) {
    this(0, name, 0.0, false);
}
```

`this(...)` invokes another constructor in the same class. This is **constructor chaining**.

```text
new Driver("Ravi")
    → Driver(String)
    → Driver(int, String, double, boolean)
    → fields become 0, "Ravi", 0.0, false
    → construction completes
```

Only one object is created. Calling another constructor does not create a second object.

### Follow a two-step constructor

This name-and-rating version first initializes the object, then updates its rating:

```java
public Driver(String name, double rating) {
    this(0, name, 0.0, false);
    this.rating = rating;
}
```

Flow:

```text
new Driver("Meera", 4.8)
    → call the four-argument constructor with rating 0.0
    → return to the two-argument constructor
    → assign rating 4.8
```

After understanding that flow, simplify it:

```java
public Driver(String name, double rating) {
    this(0, name, rating, false);
}
```

This sends the intended rating directly to the main constructor. It also gives us one place to validate constructor input later.

We place `this(...)` first in these examples, which works with the Java 22 setup used to verify this tutorial. Constructor delegation must not form a cycle.

Remember the difference:

```java
this.rating = rating; // Access this object's field.
this(0, name, rating, false); // Call another constructor.
```

## 10. Copy an object

A copy constructor accepts another `Driver`:

```java
public Driver(Driver other) {
    this.name = other.name;
    this.rating = other.rating;
    this.driverId = other.driverId;
    this.isOnline = other.isOnline;
}
```

Try:

```java
Driver original = new Driver(143, "Ashok", 4.5, true);
Driver alias = original;
Driver copy = new Driver(original);

original.name = "xyz";
System.out.println(alias.name); // xyz
System.out.println(copy.name);  // Ashok
```

```text
original ──┐
           ├──→ Object A: name="xyz"
alias ─────┘

copy ─────────→ Object B: name="Ashok"
```

A copy constructor is a convention we implement ourselves. Java does not supply one automatically.

Primitive field values are copied, and the `String` reference is copied. Sharing a `String` is safe here because strings are immutable. If we later add a mutable list of rides, copying its reference would share that list; a deeper copy would require additional work.

Our final version will delegate to the main constructor:

```java
public Driver(Driver other) {
    this(other.driverId, other.name, other.rating, other.isOnline);
}
```

This expects `other` to be non-null. It also copies the ID for learning purposes; a production registration system would need an explicit identity policy.

## 11. Share a counter with static

The next question is: how many drivers have we created?

An ordinary instance field cannot act as one shared counter:

```java
int totalDrives;
```

Each object receives its own copy. Incrementing it in a constructor updates that object's counter.

Use a static field for class-wide state:

```java
static int totalDrives;
```

A name such as `totalDrives` suggests a count of rides. Because we are counting driver objects, the final code uses `totalDrivers`.

```text
Driver class → one shared totalDrivers counter
    ↑              ↑              ↑
 object A       object B       object C
```

### Find a counting gap

Suppose we increment only inside the four-argument constructor:

```java
totalDrives++;
```

Therefore:

- The four-argument constructor increments the counter.
- The name-only constructor increments it through delegation.
- The name-and-rating constructor increments it through delegation.
- The earlier no-argument constructor does not increment it.
- The earlier copy constructor does not increment it.

### Count once per successful construction

Route every constructor through the main constructor, including these two:

```java
public Driver() {
    this(100, "Ashok", 5.0, true);
}

public Driver(Driver other) {
    this(other.driverId, other.name, other.rating, other.isOnline);
}
```

Increment once in the main constructor after successful initialization. Do not also increment in the delegating constructors.

The final counter means **successful Driver constructions during this program run**. It does not count completed rides, database registrations, or currently reachable objects. In a concurrent application, a plain `int` increment would also need a thread-safety strategy.

## 12. Use static methods and static blocks

### A registration method

Consider this declaration:

```java
static public void Register() {
    System.out.println("This is Register Method");
}
```

This is valid Java. The conventional style is:

```java
public static void register() {
    System.out.println("This is Register Method");
}
```

Call it without creating a driver:

```java
Driver.register();
```

A static method belongs to the class and needs no current object. In this tutorial, the method only prints a message; its name alone does not perform registration.

A static method can access static fields directly. To access an instance field, it needs a reference to an instance. It cannot use `this`.

### A static-block experiment

Temporarily add this inside `Driver`:

```java
static {
    totalDrivers = 15;
    System.out.println("This is executed at time of driver class");
}
```

A static block runs during class initialization, once for that initialized class. Static field initializers and static blocks execute in source order.

For example, run a fresh program with:

```java
public static void main(String[] args) {
    System.out.println("Client main started");
    Driver.register();
}
```

With the experimental block, output is:

```text
Client main started
This is executed at time of driver class
This is Register Method
```

Here, the first active use of `Driver` occurs inside `main`. The block does not automatically run before `Client.main()`.

Remove this experiment before running the final program. Starting the counter at 15 would obscure the object-count example.

## 13. Print useful information with toString

Try printing an object:

```java
System.out.println(d5);
```

If we do not override `toString()`, the inherited implementation produces text resembling `Driver@...`. That representation is not a reliable raw memory address and does not show our driver's state.

Start with a simple override:

```java
@Override
public String toString() {
    return "Ashok";
}
```

Now printing any non-null `Driver` produces `Ashok`, even if its name is `Ravi`. The method returns a fixed string.

To see state, return field values instead:

```java
@Override
public String toString() {
    return "Driver{" +
            "driverId=" + driverId +
            ", name='" + name + '\'' +
            ", rating=" + rating +
            ", isOnline=" + isOnline +
            '}';
}
```

For a new `Driver(143, "Ashok", 4.5, true)`, this produces:

```text
Driver{driverId=143, name='Ashok', rating=4.5, isOnline=true}
```

`@Override` asks the compiler to check that this method overrides an inherited method.

## 14. Understand Java pass-by-value

Java always passes arguments **by value**. The important question is: what value is being copied?

### Experiment A: a primitive value

Consider this example:

```java
int z = 10;
update(z);
System.out.println(z);
```

Add this method inside `Client`, outside `main`:

```java
public static void update(int z) {
    z += 10;
}
```

Output:

```text
10
```

The parameter and the caller's variable are separate variables:

```text
main:   z = 10
               copy 10 into the parameter
update: z = 10 → 20
main:   z is still 10
```

The matching name `z` does not connect their storage.

### Experiment B: a reference value

Now pass a driver to a method:

```java
public static void change(Driver driver) {
    driver.name = "xyz";
    System.out.println(driver.name);
}
```

Call it:

```java
Driver d5 = new Driver(143, "Ashok", 4.5, true);
change(d5);
System.out.println(d5.name);
```

Output:

```text
xyz
xyz
```

Java copies the reference value into the parameter. It does not copy the object:

```text
main's d5       ──┐
                  ├──→ Same Driver object
change's driver ──┘
```

Updating that object through `driver` is visible through `d5`.

### Experiment C: replace the parameter

```java
public static void replace(Driver driver) {
    driver = new Driver(999, "New Driver", 3.0, false);
}
```

After Experiment B:

```java
replace(d5);
System.out.println(d5.name); // xyz
```

Reassigning the local parameter does not reassign `d5`:

```text
Before reassignment:
    d5 and driver → object A

After reassignment inside replace:
    d5     → object A
    driver → object B
```

A new object was still created, so the static construction counter increases. The caller's reference continues to identify object A.

**The rule to remember:** A method can use a copied reference to mutate a shared object. Reassigning that parameter changes only the parameter.

## 15. Protect state with encapsulation

Until now, the client could write this directly:

```java
d5.rating = 100;
```

But a valid rating should be between 0 and 5. How do we keep callers from bypassing that rule?

First, make the field private:

```java
private double rating;
```

Now direct access from `Client` fails to compile. Expose controlled operations:

```java
public double getRating() {
    return rating;
}

public void setRating(double rating) {
    if (rating >= 0 && rating <= 5) {
        this.rating = rating;
    } else {
        System.out.println("The rating you give is wrong");
    }
}
```

This version rejects an invalid update by printing a message and keeping the existing value:

```java
d5.setRating(4.8);
d5.setRating(8.0);
System.out.println(d5.getRating());
```

Output:

```text
The rating you give is wrong
4.8
```

### Close the constructor gap

A constructor that directly assigns `this.rating = rating` can still accept an invalid value. The same rule must apply during construction and later updates.

In the final code, use one validation helper:

```java
private static void validateRating(double rating) {
    if (!(rating >= 0 && rating <= 5)) {
        throw new IllegalArgumentException("Rating must be between 0 and 5");
    }
}
```

Both the constructor and setter call it before assigning the field. The negated valid-range check also rejects `Double.NaN`, which is not a meaningful rating.

An exception reports failure to the caller. The client can handle it with `try` / `catch`; otherwise it propagates. This is an intentional improvement over the original print-only setter.

```java
public void setRating(double rating) {
    validateRating(rating);
    this.rating = rating;
}
```

### Understand getters and setters

We can also provide getters and setters for ID, name, online status, and the static count.

```java
public int getDriverId() { return driverId; }
public void setDriverId(int driverId) { this.driverId = driverId; }
public String getName() { return name; }
public void setName(String name) { this.name = name; }
public boolean isOnline() { return isOnline; }
public void setOnline(boolean online) { isOnline = online; }
```

`isOnline()` is a conventional boolean getter name. `setOnline(boolean online)` assigns a chosen status, while `changeStatus()` toggles the current status.

Consider this static accessor pattern:

```java
public static int getTotalDrives() {
    return totalDrives;
}

public static void setTotalDrives(int totalDrives) {
    Driver.totalDrives = totalDrives;
}
```

`Driver.totalDrives` identifies the class field when the parameter has the same name. However, a public setter lets callers replace the count with any number. The final code renames the field to `totalDrivers`, provides a getter, and omits this setter to protect the counting rule.

Encapsulation is more than adding getters and setters. It means keeping state and its operations together, while controlling access so that the object's rules remain true.

Our final class makes all instance fields private. ID and name setters remain to demonstrate accessors; a larger application would decide whether changing identity should be allowed.

## 16. Run the complete program

The following files combine the lessons into one runnable example. The early direct-field examples now use getters and setters.

Before compiling, check the following:

- Remove `import java.sql.Driver;`. It names an unrelated JDBC interface, not our class.
- Use `public static void main(String[] args)` for the entry point used here.
- Use the correct variable name, such as `d5`, consistently.
- Put each public class in its own file with the matching name.
- Use `Driver` from the same folder and unnamed package; no import is needed.

### Driver.java

```java
public class Driver {
    private int driverId;
    private String name;
    private double rating;
    private boolean isOnline;
    private static int totalDrivers;

    public Driver() {
        this(100, "Ashok", 5.0, true);
    }

    public Driver(int driverId, String name, double rating, boolean isOnline) {
        validateRating(rating);
        this.driverId = driverId;
        this.name = name;
        this.rating = rating;
        this.isOnline = isOnline;
        totalDrivers++;
    }

    public Driver(String name) {
        this(0, name, 0.0, false);
    }

    public Driver(String name, double rating) {
        this(0, name, rating, false);
    }

    public Driver(Driver other) {
        this(other.driverId, other.name, other.rating, other.isOnline);
    }

    public void acceptRide(String rideId) {
        System.out.println("Ride has been accepted " + rideId);
    }

    public void changeStatus() {
        isOnline = !isOnline;
        System.out.println("Driver is " + isOnline);
    }

    public static void register() {
        System.out.println("This is Register Method");
    }

    private static void validateRating(double rating) {
        if (!(rating >= 0 && rating <= 5)) {
            throw new IllegalArgumentException("Rating must be between 0 and 5");
        }
    }

    public int getDriverId() { return driverId; }
    public void setDriverId(int driverId) { this.driverId = driverId; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public double getRating() { return rating; }

    public void setRating(double rating) {
        validateRating(rating);
        this.rating = rating;
    }

    public boolean isOnline() { return isOnline; }
    public void setOnline(boolean online) { isOnline = online; }
    public static int getTotalDrivers() { return totalDrivers; }

    @Override
    public String toString() {
        return "Driver{" +
                "driverId=" + driverId +
                ", name='" + name + '\'' +
                ", rating=" + rating +
                ", isOnline=" + isOnline +
                '}';
    }
}
```

### Client.java

```java
public class Client {
    public static void main(String[] args) {
        Driver.register();
        Driver d1 = new Driver();
        Driver d2 = new Driver("Ravi");
        Driver d3 = new Driver("Meera", 4.8);
        Driver d5 = new Driver(143, "Ashok", 4.5, true);
        Driver copy = new Driver(d5);
        Driver alias = d5;

        System.out.println("Default: " + d1);
        System.out.println("Name only: " + d2);
        System.out.println("Name and rating: " + d3);
        System.out.println("Debug");
        System.out.println("Driver ID: " + d5.getDriverId());
        System.out.println("Objects created: " + Driver.getTotalDrivers());

        d5.acceptRide("RIDE-101");
        d5.changeStatus();

        int z = 10;
        update(z);
        System.out.println("After update: " + z);

        change(d5);
        System.out.println("After change: " + d5.getName());
        System.out.println("Alias name: " + alias.getName());
        System.out.println("Copy name: " + copy.getName());

        replace(d5);
        System.out.println("After replace: " + d5.getName());
        System.out.println("Objects after replace: " + Driver.getTotalDrivers());

        d5.setRating(4.8);
        try {
            d5.setRating(8.0);
        } catch (IllegalArgumentException exception) {
            System.out.println(exception.getMessage());
        }
        System.out.println("Rating after invalid update: " + d5.getRating());
        System.out.println("Final: " + d5);
    }

    public static void update(int z) {
        z += 10;
    }

    public static void change(Driver driver) {
        driver.setName("xyz");
        System.out.println("Inside change: " + driver.getName());
    }

    public static void replace(Driver driver) {
        driver = new Driver(999, "New Driver", 3.0, false);
    }
}
```

### Compile and run

Open a terminal in the `oop-code` folder and run:

```sh
javac Driver.java Client.java
java Client
```

A JDK is required. This example was compiled and run with Java 22.0.1.

### Expected output

```text
This is Register Method
Default: Driver{driverId=100, name='Ashok', rating=5.0, isOnline=true}
Name only: Driver{driverId=0, name='Ravi', rating=0.0, isOnline=false}
Name and rating: Driver{driverId=0, name='Meera', rating=4.8, isOnline=false}
Debug
Driver ID: 143
Objects created: 5
Ride has been accepted RIDE-101
Driver is false
After update: 10
Inside change: xyz
After change: xyz
Alias name: xyz
Copy name: Ashok
After replace: xyz
Objects after replace: 6
Rating must be between 0 and 5
Rating after invalid update: 4.8
Final: Driver{driverId=143, name='xyz', rating=4.8, isOnline=false}
```

### Follow the complete execution

1. `Client.main()` calls `Driver.register()`. `Driver` is initialized before the static method runs; the counter starts at zero. The experimental static block is absent.
2. Five `new Driver(...)` expressions create `d1`, `d2`, `d3`, `d5`, and `copy`. All constructor paths reach the four-argument constructor, so the counter becomes five.
3. `alias = d5` copies a reference. No object is created and the counter stays at five.
4. Printing the drivers calls their `toString()` methods and shows the constructor results.
5. `acceptRide()` prints a message. `changeStatus()` changes `d5.isOnline` from `true` to `false`.
6. `update(z)` changes its local parameter. The caller still prints `10`.
7. `change(d5)` changes the shared object's name to `xyz`. Both `d5` and `alias` see it. `copy` still has the name `Ashok`.
8. `replace(d5)` creates a sixth object and assigns its reference to the local parameter. The caller's `d5` still refers to the original object.
9. `setRating(4.8)` succeeds. `setRating(8.0)` throws an exception before assignment; the catch block prints its message. The rating remains `4.8`.
10. The final print shows the original driver's current state: ID 143, name `xyz`, rating 4.8, offline.

## 17. Practice and check your understanding

Try predicting each result before running it.

### Exercise 1: constructor availability

A class declares only `Driver(String name)`. Will `new Driver()` compile?

<details>
<summary>Answer</summary>

No. Once you declare a constructor, Java does not supply a default constructor. Add a no-argument constructor if you want that call to work.

</details>

### Exercise 2: alias or copy?

```java
Driver first = new Driver("Ashok", 4.5);
Driver second = first;
second.setName("Ravi");
System.out.println(first.getName());
```

<details>
<summary>Answer</summary>

`Ravi`. Both variables identify the same object. Replace the second line with `Driver second = new Driver(first);` and the output becomes `Ashok`.

</details>

### Exercise 3: constructor counting

Starting in a fresh process, what does this print?

```java
Driver a = new Driver();
Driver b = new Driver("Ravi");
Driver c = a;
Driver d = new Driver(a);
System.out.println(Driver.getTotalDrivers());
```

<details>
<summary>Answer</summary>

`3`. There are three `new` expressions. Constructor delegation does not create extra objects, and assigning `a` to `c` creates no object.

</details>

### Exercise 4: failed validation

What happens if you call `new Driver(10, "Ravi", 8.0, true)` using the final class?

<details>
<summary>Answer</summary>

The constructor throws `IllegalArgumentException`. It does not successfully return a Driver, and the counter is not incremented because validation happens first.

</details>

### Exercise 5: assignment versus mutation

Inside a method receiving a `Driver driver` parameter, explain the difference:

```java
driver.setName("xyz");
driver = new Driver("Another Driver");
```

<details>
<summary>Answer</summary>

The first statement changes the object identified by the copied reference. The caller can observe that mutation. The second statement assigns a different reference to the local parameter; it does not replace the caller's reference.

</details>

### Build the next small improvement

Add a `goOnline()` method and a `goOffline()` method. Then decide how `acceptRide()` should behave when the driver is offline. Write down the rule first, implement it, and observe the resulting state and output.

This extends the same recipe: identify the requirement, choose the state it depends on, and expose an operation that keeps the rule in one place.

