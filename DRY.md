### **Don't Repeat Yourself (DRY) Principle**

The **Don't Repeat Yourself (DRY)** principle is a fundamental concept in software development that encourages the elimination of duplication. It emphasizes the importance of maintaining a single source of truth for any piece of knowledge in a program, reducing redundancy and making the code easier to maintain.

The DRY principle suggests that:

- **Every piece of knowledge must have a single, unambiguous, authoritative representation** within a system.
    
- Code duplication should be avoided, and **reusable components** or methods should be created instead.
    

### **Why DRY?**

- **Maintainability**: When the same logic is repeated in multiple places, updating it becomes a tedious and error-prone process. DRY ensures that when one piece of functionality changes, you only need to change it in one place.
    
- **Readability**: Redundant code can clutter the system, making it harder to read and understand.
    
- **Reduced Bugs**: Duplication often leads to inconsistencies or bugs when updates are not applied uniformly.
    
- **Scalability**: With DRY, it's easier to add new features or modify existing ones since the functionality is modular and encapsulated.
    

---

### **How to Achieve DRY in .NET**

In .NET, DRY can be achieved through several techniques such as creating reusable methods, services, libraries, and using concepts like **inheritance**, **composition**, and **dependency injection**.

Here’s a look at different approaches to implementing DRY, with examples:

---

### 1. **Create Reusable Methods/Functions**

By encapsulating logic in functions or methods, you can call the same logic from multiple places instead of repeating it.

#### Example: Reusable Method

```csharp
public class MathUtility
{
    public int Add(int a, int b)
    {
        return a + b;
    }
}
```

Instead of writing `a + b` multiple times in your code, you call `Add()` wherever needed.

```csharp
public class Calculator
{
    private readonly MathUtility _mathUtility;

    public Calculator(MathUtility mathUtility)
    {
        _mathUtility = mathUtility;
    }

    public void Calculate()
    {
        int result = _mathUtility.Add(5, 10);
        Console.WriteLine(result);
    }
}
```

Now, if the addition logic changes, it only needs to be modified in one place (`Add` method).

---

### 2. **Use Inheritance (Object-Oriented Programming)**

Inheritance allows you to reuse the functionality of a base class in derived classes, avoiding code duplication across multiple classes.

#### Example: Using Inheritance

```csharp
public class Animal
{
    public void Eat()
    {
        Console.WriteLine("Eating...");
    }

    public void Sleep()
    {
        Console.WriteLine("Sleeping...");
    }
}

public class Dog : Animal
{
    public void Bark()
    {
        Console.WriteLine("Barking...");
    }
}

public class Cat : Animal
{
    public void Meow()
    {
        Console.WriteLine("Meowing...");
    }
}
```

Here, both `Dog` and `Cat` share common behavior (`Eat`, `Sleep`) through inheritance, which avoids code repetition.

---

### 3. **Use Interfaces and Dependency Injection**

Interfaces define a contract for reusable functionality. By injecting dependencies via **dependency injection (DI)**, you can create loosely-coupled and reusable components.

#### Example: Using DI and Interfaces

```csharp
public interface INotificationService
{
    void SendNotification(string message);
}

public class EmailNotificationService : INotificationService
{
    public void SendNotification(string message)
    {
        Console.WriteLine($"Sending Email: {message}");
    }
}

public class SMSNotificationService : INotificationService
{
    public void SendNotification(string message)
    {
        Console.WriteLine($"Sending SMS: {message}");
    }
}

public class UserService
{
    private readonly INotificationService _notificationService;

    // Dependency Injection
    public UserService(INotificationService notificationService)
    {
        _notificationService = notificationService;
    }

    public void CreateUser(string userName)
    {
        // Create the user
        Console.WriteLine($"User {userName} created.");

        // Send notification
        _notificationService.SendNotification($"Welcome, {userName}!");
    }
}
```

Here, the **`UserService`** doesn't depend on a specific notification method but instead uses a generic `INotificationService` interface, which can be injected with different implementations (Email, SMS, etc.). This avoids repeating the logic for sending notifications.

---

### 4. **Use Extension Methods**

Extension methods allow you to add functionality to existing types without modifying their source code.

#### Example: Extension Method

```csharp
public static class StringExtensions
{
    public static bool IsValidEmail(this string input)
    {
        return input.Contains("@");
    }
}
```

Now, any string can use the `IsValidEmail()` method without modifying the `string` class.

```csharp
string email = "example@example.com";
bool isValid = email.IsValidEmail();  // Uses extension method
```

This prevents you from having to duplicate the email validation logic in multiple places.

---

### 5. **Use Generics**

Generics allow you to write methods and classes that can handle any data type without duplication.

#### Example: Using Generics

```csharp
public class Repository<T>
{
    private readonly List<T> _data = new List<T>();

    public void Add(T entity)
    {
        _data.Add(entity);
    }

    public T Get(int index)
    {
        return _data[index];
    }
}
```

You can use this repository class for different types of data without duplicating logic.

```csharp
var userRepo = new Repository<User>();
userRepo.Add(new User("John Doe"));

var productRepo = new Repository<Product>();
productRepo.Add(new Product("Laptop"));
```

By using **generics**, you avoid duplicating the same repository code for different types.

---

### 6. **Use Services for Repeated Business Logic**

Business logic that is used by multiple parts of the application can be encapsulated into services that can be reused across controllers, views, or other services.

#### Example: Reusable Business Logic Service

```csharp
public class DiscountService
{
    public decimal CalculateDiscount(decimal price, decimal discountPercentage)
    {
        return price * (discountPercentage / 100);
    }
}

public class OrderService
{
    private readonly DiscountService _discountService;

    public OrderService(DiscountService discountService)
    {
        _discountService = discountService;
    }

    public void ProcessOrder(decimal price, decimal discountPercentage)
    {
        var discountAmount = _discountService.CalculateDiscount(price, discountPercentage);
        Console.WriteLine($"Discount Amount: {discountAmount}");
    }
}
```

Here, the **`DiscountService`** contains the logic for calculating discounts. This logic can now be reused across different services or controllers in the application.

---

### 7. **Avoiding Hardcoding Constants**

Repeatedly using hardcoded values in your code leads to duplication. Instead, store common values in constants or configuration files.

#### Example: Using Constants

```csharp
public class PaymentService
{
    public const decimal TAX_RATE = 0.05m;

    public decimal CalculateTotal(decimal amount)
    {
        return amount + (amount * TAX_RATE);
    }
}
```

Here, **`TAX_RATE`** is stored as a constant instead of being hardcoded throughout the code.

---

### **Summary**

The **DRY principle** helps to:

- Improve maintainability by reducing redundancy.
    
- Make the codebase easier to understand and modify.
    
- Avoid errors due to inconsistent logic.
    

---
