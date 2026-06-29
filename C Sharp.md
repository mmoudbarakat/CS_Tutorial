
### Ref: C# 7.0 in a Nutshell_ The Definitive Reference


![[C 7.0 in a Nutshell_ The Definitive Reference ( PDFDrive )_2.pdf]]





# C# Access Modifiers

## 1. public
- **Accessible from**: Anywhere.
- **Example**:
```csharp
public class Car
{
    public string Brand { get; set; }
}
```

## 2. private
- **Accessible from**: Only within the containing class.
- **Example**:
```csharp
public class Car
{
    private int speed;
}
```

## 3. protected
- **Accessible from**: Containing class and derived classes.
- **Example**:
```csharp
public class Vehicle
{
    protected int wheels;
}

public class Bike : Vehicle
{
    public void SetWheels()
    {
        wheels = 2;
    }
}
```

## 4. internal
- **Accessible from**: Anywhere in the same assembly (project).
- **Example**:
```csharp
internal class Engine
{
    public int HorsePower { get; set; }
}
```

## 5. protected internal
- **Accessible from**:
  - Same assembly
  - OR from derived classes even outside the assembly.
- **Example**:
```csharp
public class Animal
{
    protected internal void Eat() { }
}

public class Dog : Animal
{
    public void Bark()
    {
        Eat(); // Accessible
    }
}
```

## 6. private protected
- **Accessible from**:
  - Containing class
  - OR derived classes **within the same assembly** only.
- **Example**:
```csharp
public class Plant
{
    private protected void Grow() { }
}

public class Flower : Plant
{
    public void Bloom()
    {
        Grow(); // Accessible only if within the same assembly
    }
}
```

# Summary Table

| Modifier             | Accessible from...                                    |
|----------------------|--------------------------------------------------------|
| `public`             | Anywhere                                                |
| `private`            | Containing class only                                   |
| `protected`          | Containing class and derived classes                    |
| `internal`           | Same assembly                                           |
| `protected internal` | Same assembly OR any derived class (even outside)       |
| `private protected`  | Same assembly + derived classes only                   |

---

````markdown
 Immutability in C#

What Is Immutability?

Immutability means that once an object is created, its state cannot be changed. In C#, this is useful for:
- Avoiding side effects
- Simplifying multithreaded code
- Enabling functional programming patterns

---

## ✅ Immutable Class Example

```csharp
public class Person
{
    public string Name { get; }
    public int Age { get; }

    public Person(string name, int age)
    {
        Name = name;
        Age = age;
    }

    public Person WithAge(int newAge)
    {
        return new Person(Name, newAge);
    }
}
````

### Usage

```csharp
var person1 = new Person("Mahmoud", 24);
var person2 = person1.WithAge(25);

Console.WriteLine(person1.Age); // 24
Console.WriteLine(person2.Age); // 25
```

🟢 `person1` remains unchanged.  
🟢 `person2` is a **new** instance with a different age.  
This is the core of **immutability**.

---

##  Using `record` for Immutability (C# 9.0+)

```csharp
public record Product(string Name, decimal Price);
```

### Usage

```csharp
var product1 = new Product("Laptop", 1500m);
var product2 = product1 with { Price = 1400m };

Console.WriteLine(product1.Price); // 1500
Console.WriteLine(product2.Price); // 1400
```

`record` types are **immutable by default**, and `with` expressions let you create **copies with modified values**.

---

##  Why Use `WithX()` Methods?

The method:

```csharp
public Person WithAge(int newAge)
{
    return new Person(Name, newAge);
}
```

- Creates a new object with the same data, except for the changed `Age`.
    
- Prevents mutation of the original object.
    
- Enables **functional-style coding**.
    

This avoids problems like:

```csharp
person.Age = 30; // ❌ Not allowed — no setters
```

Instead:

```csharp
var olderPerson = person.WithAge(30); // ✅ Immutable pattern
```

---

##  How to Make Objects Immutable

1. Use `readonly` fields or properties.
    
2. Avoid `set` accessors.
    
3. Use constructors to set data.
    
4. Use `record` or custom `WithX()` methods.
    

---

## What is an exception ?
# C# Exception Handling

An **Exception** is an object that describes an error that occurred during the execution of a program. Using `try-catch` blocks prevents your application from crashing by handling these errors gracefully.

## Code Example
```csharp
using System;

public class ExceptionDemo
{
    public static void Main()
    {
        try
        {
            // Code that might cause an error
            int[] numbers = { 1, 2, 3 };
            Console.WriteLine(numbers[10]); // Index out of range
        }
        catch (IndexOutOfRangeException ex)
        {
            // Runs only if the specific error occurs
            Console.WriteLine("Error: You tried to access an index that doesn't exist.");
            Console.WriteLine(\$"Message: {ex.Message}");
        }
        catch (Exception ex)
        {
            // Catch-all for any other errors
            Console.WriteLine(\$"Generic Error: {ex.Message}");
        }
        finally
        {
            // Always executes (useful for closing database connections/files)
            Console.WriteLine("Execution complete.");
        }
    }
}
```

## Key Components
- **try**: The block where you "try" your risky code.
- **catch**: The block that handles the "thrown" exception.
- **finally**: The block that runs no matter what happens.
---
### What is operator overloading?

In C#, **Operator Overloading** allows you to define custom behavior for built-in operators (like `+`, `-`, `*`, `==`) when they are used with your own objects.

Here is a clean, **Obsidian-formatted** example using a `Money` object to demonstrate how to "add" two currency objects together using the `+` operator:
# C# Operator Overloading

Operator overloading allows you to define how operators (like `+`, `-`, or `==`) work with your custom classes or structs. This makes your objects feel like native data types.

## Rules for Overloading
- Use the `operator` keyword.
- The method must be `public static`.
- At least one parameter must be the containing type (your class/struct).

## Code Example: Adding Money Objects
```csharp
using System;

public class Money
{
    public decimal Amount { get; set; }
    public string Currency { get; set; }

    public Money(decimal amount, string currency)
    {
        Amount = amount;
        Currency = currency;
    }

    // Overloading the '+' operator
    public static Money operator +(Money m1, Money m2)
    {
        if (m1.Currency != m2.Currency)
        {
            throw new InvalidOperationException("Cannot add different currencies.");
        }

        return new Money(m1.Amount + m2.Amount, m1.Currency);
    }

    public override string ToString() => \$"{Amount} {Currency}";
}

public class Program
{
    public static void Main()
    {
        Money wallet1 = new Money(50.00m, "USD");
        Money wallet2 = new Money(25.50m, "USD");

        // Using the overloaded + operator
        Money total = wallet1 + wallet2;

        Console.WriteLine(\$"Total Balance: {total}"); 
        // Output: Total Balance: 75.50 USD
    }
}
```

## Common Operators to Overload

| Operator | Usage |
| :--- | :--- |
| `+`, `-`, `*`, `/` | Arithmetic operations between objects. |
| `==`, `!=` | Custom equality logic (Must be overloaded in pairs). |
| `>`, `<` | Comparison logic (Must be overloaded in pairs). |

---
## What is a Finalizer?

# C# Finalizers

A **Finalizer** is a special method used to clean up an object before the Garbage Collector (GC) removes it from memory. It acts as a safety net to release "unmanaged resources" like file handles, database connections, or window handles.

## Key Rules
- Syntax: Uses the tilde `~` followed by the class name.
- No Access Modifiers: You cannot use `public` or `private`.
- No Parameters: You cannot pass data into a finalizer.
- Automatic: It is called by the Garbage Collector, never manually by the developer.

## Code Example
```csharp
using System;

public class FileManager
{
    public string FileName { get; set; }

    public FileManager(string name)
    {
        FileName = name;
        Console.WriteLine(\$"{FileName} is now open.");
    }

    // This is the FINALIZER
    ~FileManager()
    {
        // Cleanup unmanaged code here
        Console.WriteLine(\$"{FileName} was cleaned up by the Garbage Collector.");
    }
}

public class Program
{
    public static void Main()
    {
        // Create an object and let it go out of scope
        CreateObject();

        // Note: GC.Collect is used here ONLY to demonstrate the finalizer firing.
        // In real apps, you should let the system handle this.
        GC.Collect();
        GC.WaitForPendingFinalizers();

        Console.WriteLine("Done.");
    }

    static void CreateObject()
    {
        var file = new FileManager("SecretData.txt");
    }
}
```

## Comparison: Finalizer vs. Dispose


| Feature | Finalizer (`~Name`) | Dispose (`IDisposable`) |
| :--- | :--- | :--- |
| **Trigger** | Automatic (Garbage Collector) | Manual (Calling `.Dispose()`) |
| **Timing** | Undefined (whenever the GC runs) | Immediate (right when you want it) |
| **Best For** | Unmanaged resource safety net | Managed & Unmanaged cleanup |

---
## What is an Indexer?

# C# Indexers

An **Indexer** allows an object to be indexed just like an array. It enables you to use the `[]` bracket notation on a class instance to get or set internal data, making the object act like a collection.

## Key Rules
- Use the `this` keyword to define the indexer.
- They can be overloaded (e.g., indexing by `int` or by `string`).
- They do not have a name of their own; they are identified by the `this` keyword.

## Code Example: A Custom Collection
```csharp
using System;

public class Sentence
{
    private string[] words;

    public Sentence(string text)
    {
        words = text.Split(' ');
    }

    // Defining the INDEXER
    public string this[int index]
    {
        get 
        { 
            return words[index]; 
        }
        set 
        { 
            words[index] = value; 
        }
    }

    public void Display() => Console.WriteLine(string.Join(" ", words));
}

public class Program
{
    public static void Main()
    {
        var mySentence = new Sentence("The quick brown fox");

        // Accessing the object using array-like syntax
        string firstWord = mySentence[0]; 
        Console.WriteLine(firstWord); // Output: The

        // Modifying the object using the indexer
        mySentence[3] = "wolf";
        
        mySentence.Display(); // Output: The quick brown wolf
    }
}
```

## Why Use Indexers?
- **Readability**: It simplifies the syntax for classes that represent a list or dictionary.
- **Abstraction**: You can hide a complex internal data structure behind a simple array-like interface.
- **Flexibility**: You can add validation logic inside the `set` block to prevent invalid data from being added.
## Another Ex:

# C# Indexer Implementation: JointAccount

An **Indexer** in this context allows the `JointAccount` class to manage a private collection of `Account` objects. By using an indexer, you can access specific accounts directly on the `JointAccount` instance using `[]` syntax, while the class maintains internal validation to prevent errors like `IndexOutOfRangeException`.

## Code Example
```csharp
using System;

internal class Account 
{ 
    public string Owner { get; set; } 
}

internal class JointAccount
{
    private const int MaxAccountSize = 3;
    private Account[] _accountList = new Account[MaxAccountSize];

    // The Indexer
    public Account this[int index]
    {
        get
        {
            // Boundary validation: Prevents crashes from invalid indices
            if (index >= 0 && index <= MaxAccountSize - 1)
            {
                return _accountList[index];
            }
            else
            {
                // Returns null if the index is out of bounds
                return null;
            }
        }
        set
        {
            // Ensures we only write to valid slots in the array
            if (index >= 0 && index <= MaxAccountSize - 1)
            {
                _accountList[index] = value;
            }
        }
    }
}

public class Program
{
    public static void Main()
    {
        JointAccount joint = new JointAccount();

        // Using the 'set' accessor via indexer
        joint[0] = new Account { Owner = "Alice" };
        joint[1] = new Account { Owner = "Bob" };

        // Using the 'get' accessor via indexer
        Console.WriteLine(joint[0]?.Owner); // Output: Alice
        
        // Testing bounds (Returns null instead of crashing)
        Console.WriteLine(joint[5] == null ? "Invalid Account" : "Valid"); 
    }
}
```

## Key Components of this Pattern
- **Encapsulation**: The internal `_accountList` array is private. External code cannot resize it or replace the entire array; they can only interact with individual slots you've exposed.
- **Validation**: The `if` statements inside the `get` and `set` accessors provide "safe" access. Instead of the program crashing with a standard [IndexOutOfRangeException](https://microsoft.com "C# IndexOutOfRangeException Documentation"), the `get` accessor simply returns `null`.
- **The `this` Keyword**: In C#, the `this` keyword is mandatory to define an indexer. It tells the compiler that the class itself can be indexed like an array.
---
# C# Nested Types

A **Nested Type** is a class, struct, or enum defined inside another class or struct. In Object-Oriented Programming, this is used to group types that are logically tightly coupled or to hide a helper class that shouldn't be accessible outside its "parent" container.

## Key Rules
- **Access Modifiers**: Unlike top-level classes (which are typically `public` or `internal`), nested types can be `private`, `protected`, or `private protected`.
- **Relationship**: The nested type can access all members of the outer type, including its private fields.
- **Reference Syntax**: Outside the container, you refer to it using `OuterClass.NestedClass`.

## Code Example: A Car and its Engine
```csharp
using System;

public class Car
{
    public string Model { get; set; }
    private bool _isRunning = false;

    public Car(string model) => Model = model;

    public void StartCar()
    {
        // The outer class creates an instance of the nested class
        Engine engine = new Engine();
        engine.Ignite();
    }

    // NESTED TYPE: Logically, an Engine belongs specifically to a Car
    public class Engine
    {
        public void Ignite()
        {
            // Note: Nested types can conceptually logic-check against the outer container
            Console.WriteLine("Engine ignition sequence started...");
        }
    }
}

public class Program
{
    public static void Main()
    {
        // Standard usage
        Car myCar = new Car("Tesla Model S");
        myCar.StartCar();

        // Accessing the nested type from outside (requires 'public' modifier on Engine)
        Car.Engine externalEngine = new Car.Engine();
        externalEngine.Ignite();
    }
}
```

## Why use Nested Types?
1. **Encapsulation**: If a class (like `AccountValidator`) is only ever used inside `Bank`, nesting it as `private` keeps the global namespace clean.
2. **Access Privileges**: It allows the inner class to modify private state in the outer class without exposing that state to the rest of the program.
3. **Logical Grouping**: It signals to other developers that the nested type has no purpose or meaning without the parent class.
---
