
```markdown
# Full Stack .NET + Angular Developer Interview Questions (2026)

## ⚙️ C# / .NET Core

1. Difference between `IEnumerable`, `IQueryable`, and `List` in C#.
2. How **Garbage Collection** works in the .NET CLR?
3. Difference between `Equals()` vs `==` in C#.
4. What are `async`/`await` and how do they improve performance?
5. Explain **Dependency Injection (DI)** in .NET Core.
6. Difference between `ref`, `out`, and `in` parameters in C#.
7. What is the difference between **Abstract class** vs **Interface**?
8. Explain **Value types** vs **Reference types** in C#.

## ASP.NET Core & Web API

9. Difference between `ControllerBase` and `Controller` in Web API.
10. How to implement **global exception handling** in ASP.NET Core?
11. How does the **Middleware pipeline** work in .NET Core?
12. Explain **JWT Authentication & Authorization** in APIs.
13. What are **Action Filters** and when to use them?
14. Explain **Microservices** architecture and how to implement it in .NET Core.
15. What is **gRPC** and how is it different from REST APIs?
16. Explain **Caching strategies** in ASP.NET Core.

## Database & Entity Framework Core

17. Write a SQL query to find the **2nd highest salary** from an Employee table.

    > [!example] Common solution analogy  
    > Think of it like finding the second-place runner in a race: first remove the winner (MAX), then find the new fastest among the rest.

18. Explain **Transactions** in SQL Server and how EF Core manages them.
19. Difference between **Lazy loading** vs **Eager loading** in EF Core.
20. What are **Migrations** in EF Core?
21. Difference between **Stored Procedure** vs **Function**.
22. Explain **ACID** properties in databases with real examples.

    > [!example] Real-world analogy  
    > ACID is like rules for sending money via bank transfer: Atomic (all or nothing — like the transfer either fully succeeds or fully fails), Consistent (money can't vanish or appear from nowhere), Isolated (your transfer doesn't see half-done transfers of others), Durable (once committed, even power outage won't undo it).

23. What are **Indexes** and how do they improve performance?
24. Difference between `DELETE` vs `TRUNCATE` vs `DROP`.

## Angular (Frontend)

25. Difference between **Angular** vs **AngularJS**.
26. Difference between **Components**, **Directives**, and **Services** in Angular.
27. Difference between **Reactive Forms** and **Template-driven Forms**.
28. What are **Observables** and how are they used with RxJS?
29. What is **Binding** and its types?
30. How to handle **HTTP requests** using `HttpClient` in Angular?
31. Explain **Change Detection** in Angular.
32. Difference between `*ngIf` vs `hidden` in Angular templates.
33. Best way to pass data between **parent and child components**.
34. How to manage state effectively using **NgRx** or **Redux**?
35. Explain **Lazy loading** modules in Angular.
36. How to implement **Route Guards** (`AuthGuard`, `CanActivate`) in Angular?

## Coding Round (DSA + Problem Solving)

37. Find the **first non-repeating character** in a string.
38. **Reverse a linked list**.
39. Check if a string is a **Palindrome**.
40. Implement **Dependency Injection** container from scratch in .NET Core (or explain built-in one deeply).
41. Write a query to fetch **Nth highest salary**.

    > [!example] Real-world analogy  
    > Imagine salary slips stacked from highest to lowest on a desk — to find the Nth, you count down N-1 slips and take the next one (using OFFSET + FETCH or DENSE_RANK/ROW_NUMBER).

42. Implement a simple **CRUD API** in ASP.NET Core with EF Core.
43. Write a program to check if two strings are **anagrams**.
44. Implement **pagination** in Angular consuming a Web API.

## System Design & Architecture

45. Explain **Monolithic** vs **Microservices** architecture.

    > [!example] Analogy  
    > Monolith = a big single-family house (easy to build/maintain at small scale, hard to change one room without affecting others).  
    > Microservices = apartment complex (each apartment independent, easier to scale/replace one unit, but more complex plumbing/communication between units).

46. What is **CQRS** pattern and when to use it?
47. Explain **SOLID Principles** with examples.

    > [!tip] Mnemonic analogy  
    > SOLID is like building a sturdy Lego castle:  
    > S — Single responsibility (each brick has one clear job)  
    > O — Open/closed (add new features without breaking old bricks)  
    > L — Liskov (substitute bricks without the tower collapsing)  
    > I — Interface segregation (small, specific connectors instead of giant messy ones)  
    > D — Dependency inversion (plug things in via sockets, not hard-wired).

48. How do you secure **APIs** and **Angular** apps with **OAuth2 & JWT**?
49. What is **Clean Architecture** in .NET Core?

50. How do you secure **APIs** and **Angular** apps with **OAuth2 & JWT**? 
51. What is **Clean Architecture** in .NET Core?

```
---
## ==what is method signature?==

## Method Signature

A Method Signature is the unique identifier used by a compiler to differentiate between methods within the same scope. It acts as a "fingerprint" for the method.

> [!abstract] Key Rule  
> The signature is what allows Method Overloading. You can have multiple methods with the same name as long as their signatures are distinct.

## 1. Components of a Signature

In most languages (Java, C#, C++), a signature consists of:

- Method Name: The identifier (e.g., `calculate`).
- Parameter List:
    
    - The number of parameters.
    - The data types of parameters.
    - The order of parameter types.
    

## 2. What is EXCLUDED

Changing these elements does not create a new signature and will result in a "method already defined" error:

- ❌ Return Type: (e.g., `int` vs `void`).
- ❌ Parameter Names: (e.g., `int x` vs `int y`).
- ❌ Access Modifiers: (e.g., `public` vs `private`).
- ❌ Exception Lists: (e.g., `throws IOException`).

---

## 3. Comparison Examples

|Method Pair|Same Signature?|Why?|
|---|---|---|
|`void move(int x, int y)`  <br>`void move(int a, int b)`|YES|Parameter names don't matter.|
|`int add(int x)`  <br>`double add(int x)`|YES|Return types are ignored.|
|`void log(String s, int i)`  <br>`void log(int i, String s)`|NO|The order of types is different.|
|`void save()`  <br>`void save(Data d)`|NO|The number of parameters is different.|

---

## 4. Practical Application

- [[Method Overloading]]: Creating multiple methods with the same name but different signatures.
- [[Method Overriding]]: A subclass providing a specific implementation for a method that exists in its parent class (must match the signature exactly). [1, 2, 3]

---

## ==What is the Order of constructor execution?==

In a typical inheritance hierarchy, the order of constructor execution follows a ==Top-Down (Base to Derived) approach==.

Think of it like building a house: you must lay the foundation before you can frame the walls.

## 1. The Standard Execution Flow

When you instantiate a derived (child) class:

1. Base Class Constructor runs first.
2. Derived Class Constructor runs last.

If you have a multi-level hierarchy (Grandparent $\rightarrow$ Parent $\rightarrow$ Child), the order is:  
`Grandparent()` $\rightarrow$ `Parent()` $\rightarrow$ `Child()`

## 2. Why this order?

A derived class often relies on variables or methods inherited from the base class. By running the base constructor first, the language ensures that the "parent" part of the object is fully initialized and safe to use before the "child" code starts modifying things.

## 3. Detailed Step-by-Step (Internal)

Technically, the process looks like this:

- Call: You call the `Child` constructor.
- Delegation: The `Child` constructor immediately (and often invisibly) calls the `Parent` constructor.
- Execution: The `Parent` constructor finishes its work.
- Completion: Control returns to the `Child` constructor to finish its own initialization.

## 4. Language-Specific Obsidian Note

> [!info] Implementation Details
> 
> - Java/C#: Uses the `super()` or `base()` keyword. If you don't call it explicitly, the compiler inserts a call to the default (no-argument) base constructor automatically.
> - C++: Uses an Initialization List. Base constructors must be initialized before the derived class body starts.
> - Python: You must explicitly call `super().__init__()` inside the child's `__init__` method, or the parent will not be initialized.

---

## 5. Quick Comparison: Constructors vs. Destructors

|Phase|Order|Analogy|
|---|---|---|
|Construction|Base $\rightarrow$ Derived|Building from the ground up.|
|Destruction|Derived $\rightarrow$ Base|Taking down the roof before the foundation.|

---
## ==What is destructor and how many can be there in a class?==

Unlike constructors, ==a class can have only one destructor== in almost every programming language that supports them. [1, 2]

## Why only one?

The primary reason is that destructors cannot be overloaded. [3, 4]

- No Parameters: Destructors do not take any arguments, so there is no way for the compiler to distinguish between multiple versions.
- Automatic Invocation: Destructors are called automatically by the system when an object goes out of scope or is deleted. Since the system doesn't "know" which version to choose, only one well-defined cleanup path is allowed. [5, 6, 7, 8, 9]

## Language-Specific Behavior

- C++: You can define exactly one destructor (e.g., `~MyClass()`). If you don't write one, the [compiler provides a default](https://www.geeksforgeeks.org/cpp/destructors-c/). It can be declared as `virtual` to ensure proper cleanup in inheritance hierarchies.
- C#: A class can have exactly one destructor (internally called a finalizer), which is [invoked by the Garbage Collector](https://www.tutorialspoint.com/article/how-many-destructors-can-we-have-in-one-class-in-chash).
- Python: Python uses a single `__del__` method as its destructor, which is called when the object's [reference count reaches zero](https://www.geeksforgeeks.org/dsa/what-is-destructor-in-programming/).
- Java: Java does not have destructors. Instead, it relies on garbage collection and formerly used the `finalize()` method (now deprecated), preferring the [`try-with-resources` pattern](https://www.quora.com/How-many-destructors-can-a-class-have) for cleanup. [1, 6, 10, 11, 12, 13, 14]

Summary Table

|Feature [15, 16, 17]|Constructor|Destructor|
|---|---|---|
|Quantity|Multiple (Overloading)|Exactly One|
|Parameters|Can have arguments|Cannot have arguments|
|Invocation|Explicitly or implicitly on creation|Automatically on destruction|

---
## ==What happens when constructor is private ?==

When a constructor is marked as `private`, it means ==the class cannot be instantiated from outside of its own scope==. Any attempt to use the `new` keyword (e.g., `MyClass obj = new MyClass();`) in another class will result in a compilation error.

Here is how this is used in software design, formatted for your Obsidian vault:

## Private Constructors

> [!danger] Restricted Access  
> A private constructor prevents external classes from creating instances. The class controls its own "lifecycle."

## 1. Common Use Cases

Even though you can't use `new` externally, private constructors are powerful for specific design patterns:

- [[Singleton Pattern]]: Ensures only one instance of a class ever exists. The class creates the instance internally and provides a `getInstance()` method.
- Utility/Helper Classes: Classes that only contain `static` methods (like `Math` in Java) use a private constructor to prevent people from creating useless objects.
- [[Factory Pattern]]: Forces users to create objects through a specific method (e.g., `User.createAdmin()`) instead of the constructor directly.
- Constructor Chaining: You might have multiple public constructors that all eventually call one "master" private constructor to centralize initialization logic.

## 2. How to Access It?

Since you can't call it from the outside, you must provide an internal way to get an object:

1. Static Factory Method: A `static` method inside the class that calls the private constructor and returns the object.
2. Nested Classes: Inner classes can often access the private members (including constructors) of their outer class.

---

## 3. Impact on Inheritance

> [!warning] Breaking Inheritance  
> A class with only private constructors cannot be subclassed (inherited).
> 
> Why? As we discussed, a Child constructor _must_ call a Parent constructor. If the Parent's constructor is private, the Child cannot "see" it to call it, and the code won't compile.

---

## 4. Comparison Table

|Constructor Type|Can `new` be used?|Can be Subclassed?|Use Case|
|---|---|---|---|
|Public|Yes, anywhere|Yes|Standard objects.|
|Protected|Only in package/subclass|Yes|Frameworks/Base classes.|
|Private|No (Internal only)|No|Singletons, Utility classes.|

---
## ==Access modifiers simple analogy:==

## The Estate Analogy (Access Modifiers)

Imagine you own a large mansion. Who is allowed into which rooms?

## 1. `public` — The Front Porch 🏠

Anyone walking down the street can step onto your porch.

- The Rule: Visible to everyone, inside or outside the project.

## 2. `private` — The Personal Safe 🔐

Only you (the Class itself) have the combination. Even your children living in the same house cannot open it.

- The Rule: Visible only within the same class.

## 3. `protected` — The Family Living Room 👨‍👩‍👧‍👦

The "outside world" is blocked by the front door, but you and your children (Subclasses) can hang out here freely.

- The Rule: Visible to the class and its subclasses (even if the child moves to a different neighborhood/assembly).

## 4. `internal` (C#) / `default` (Java) — The House Party 🥂

Anyone who was invited to the party (is in the same Project/Assembly/Package) can roam these rooms. If you aren't on the guest list (in a different project), you can't get in.

- The Rule: Visible only within the same Project/Assembly.

## 💡 The "Memory Trick" Summary

|Modifier|Analogy|Who can see it?|
|---|---|---|
|`public`|The Porch|Everyone|
|`internal`|The Party|Only people in the same house (Project)|
|`protected`|The Family|Only you and your kids (Subclasses)|
|`private`|The Safe|Only you|

---

## ==If an Abstract class has concrete method so can its child class have same method name and can it modify it ?==


==Yes, a child class can have a method with the same name==, but how it "modifies" it depends on how the parent defined it.

In C#, there are two ways to handle this in your Obsidian notes:

## 1. Method Overriding (The Proper Way)

If the abstract class marks the concrete method as `virtual`, the child class can override it to change its behavior.

- Parent: "Here is a default way to do this, but you can change it if you want."
- Child: Uses the `override` keyword to provide a new version.

```csharp
public abstract class Animal {
    // 'virtual' means it's a concrete method that CAN be changed
    public virtual void MakeSound() {
        Console.WriteLine("Generic animal sound");
    }
}

public class Dog : Animal {
    // 'override' modifies the existing behavior
    public override void MakeSound() {
        Console.WriteLine("Bark!");
    }
}
```

## 2. Method Hiding (The "Shadowing" Way)

If the parent method is not marked `virtual`, the child can still use the same name, but it must use the `new` keyword. This doesn't technically "modify" the parent's method; it just hides it.

- Parent: "This is how I do it."
- Child: "I'm ignoring yours and making my own version from scratch."

```csharp
public abstract class Parent {
    public void Secret() { Console.WriteLine("Parent's Secret"); }
}

public class Child : Parent {
    // 'new' hides the parent method (shadowing)
    public new void Secret() { Console.WriteLine("Child's Secret"); }
}
```


|Feature|`override`|`new` (Shadowing)|
|---|---|---|
|Requirement|Parent must be `virtual`|Parent can be any concrete method|
|Relationship|Modifies the original behavior|Replaces/Hides the original|
|Polymorphism|Calling from a `Parent` variable runs the Child version|Calling from a `Parent` variable runs the Parent version|

---
## ==Abstract vs Static class vs interface:==

This is the "Big Three" of architectural design in C#. To keep your Obsidian vault clean, think of them as different levels of Contract vs. Capability.

## C# Architecture: Abstract vs. Static vs. Interface

## 1. The Comparison Table

|Feature|Interface|Abstract Class|Static Class|
|---|---|---|---|
|Analogy|A "Contract" (What it does)|A "Blueprint" (What it is)|A "Toolbox" (Utility)|
|Instantiation|❌ No|❌ No|❌ No|
|Inheritance|✅ Multiple (Many)|✅ Single (One)|❌ None|
|State (Fields)|❌ No (Only Props)|✅ Yes (Instance fields)|✅ Yes (Static only)|
|Methods|Usually just signatures|Concrete + Abstract|Static methods only|

---

## 2. Code Examples

## Interface: The "Can-Do" Contract

Use this when different types of objects need to share a capability (e.g., both a `Car` and a `Person` can `Move()`).

```csharp
public interface IMovable {
    void Move(); // No implementation, just the rule
}
```

## Abstract Class: The "Is-A" Blueprint

Use this when objects share a common identity and you want to provide default logic.

```csharp
public abstract class Vehicle {
    public int Fuel { get; set; } // Can have data (state)
    
    public void Honk() { // Concrete: Default behavior
        Console.WriteLine("Beep!"); 
    }
    
    public abstract void Drive(); // Abstract: Child MUST define this
}
```

## Static Class: The "Helper" Toolbox

Use this for functions that don't belong to a specific object (like Math or File helpers). 
Use if you want to say "I just need a **global tool**, I don't need an object.

```csharp
public static class Calculator {
    public static int Add(int a, int b) => a + b;
}
```

---

## 3. Combining Them (The Real World)

In a real project, you often use them together:

```csharp
// Child inherits from ONE base class and MANY interfaces
public class Tesla : Vehicle, IMovable {
    public override void Drive() => Console.WriteLine("Driving silently...");
    public void Move() => Console.WriteLine("Tesla is moving");
}
```

---
## ==Field vs a property:==

To get why these words were chosen, forget code for a second and think about Real Estate versus Retail.

## 1. The "Field" (Raw Land) 🌾

In English, a field is a piece of raw, open land. It’s just space.

- In Coding: A Field is just a "storage bin" in memory. It has no brain, no security, and no rules. If it’s public, anyone can walk across it, dump trash on it, or dig a hole.
- The Secret: It represents State (the raw data).

## 2. The "Property" (A Managed House) 🏠

In English, property implies ownership, boundaries, and rules. You don't just walk into someone's house; you go through the door.

- In Coding: A Property is a "smart wrapper" around that raw land. It has a front door (get) and a back door (set). You can put a security guard at the door to check ID.
- The Secret: It represents Interaction (how you access the data).

---

## 💡 The Visual Difference:

Imagine you have a `Person` class and their `Age`.

## The Field (The "Bin")

```csharp
public int age; 
```

- Problem: Someone can set `person.age = -500;`. The field is just a bin; it can't say "No."

## The Property (The "Gatekeeper")

```csharp
private int _age; // The hidden Field (the bin)

public int Age // The Property (the gatekeeper)
{
    get { return _age; }
    set {
        if (value > 0) // The "Security Guard" logic
            _age = value;
    }
}
```

---

## 🧠 Why the Confusion?

The confusion happens because of Auto-Properties:  
`public int Age { get; set; }`

This _looks_ like a field because it's only one line, but the `{ get; set; }` tells C#: _"Hey, create a hidden bin for me, but give me the 'Gatekeeper' powers just in case I want to add rules later."_

---
