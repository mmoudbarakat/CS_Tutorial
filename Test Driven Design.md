## The Definitive Guide to Test-Driven Development (TDD)

> [!abstract] Definition  
> Test-Driven Development (TDD) is a software development process relying on the repetition of a very short development cycle: first the developer writes an (initially failing) automated test case that defines a desired improvement or new function, then produces the minimum amount of code to pass that test, and finally refactors the new code to acceptable standards.

---

## 1. The Red-Green-Refactor Cycle

The heartbeat of TDD is a three-phase cycle that ensures every line of code is necessary and verified.

## Phase 1: RED (Fail)

Write a test for a small bit of functionality. You must run the test and watch it fail.

- Why? This proves the test is actually checking something and isn't a "False Positive" (a test that passes even if the logic is broken).

## Phase 2: GREEN (Pass)

Write the simplest code possible to make the test pass.

- Note: At this stage, "ugly" code is allowed. You can hardcode values or use messy logic. The goal is strictly to satisfy the test requirements.

## Phase 3: REFACTOR (Clean)

Now that you have a passing test (a safety net), clean up the code.

- Remove duplication.
- Improve naming.
- Extract methods.
- Requirement: The test must remain Green throughout this process.

---

## 2. The Three Laws of TDD

Uncle Bob (Robert C. Martin) formalized the discipline into three laws to keep the cycle tight:

1. You are not allowed to write any production code unless it is to make a failing unit test pass.
2. You are not allowed to write any more of a unit test than is sufficient to fail; and compilation failures are failures.
3. You are not allowed to write any more production code than is sufficient to pass the one failing unit test.

---

## 3. Technical Strategy: Inside-Out vs. Outside-In

## Inside-Out (Bottom-Up)

You start with the smallest components (e.g., a `Calculator` class) and build toward the user interface.

- Best for: Libraries, algorithmic logic, and data processing.

## Outside-In (Top-Down / London School)

You start with the user's requirement (e.g., an API endpoint) and use Mocks to represent the components that haven't been built yet.

- Best for: Web APIs, UI-driven applications, and ensuring the "big picture" works.

---

## 4. C# Implementation Example

Requirement: Create a system that calculates a 10% discount for "Premium" members.

## Step 1: Write the Test (RED)

```csharp
[Test]
public void CalculateDiscount_PremiumUser_ReturnsTenPercent()
{
    // Arrange
    var calculator = new DiscountCalculator();
    var user = new User { IsPremium = true };
    
    // Act
    var discount = calculator.GetDiscount(100, user);
    
    // Assert
    Assert.AreEqual(10.0m, discount);
}
```

_The code won't even compile because `DiscountCalculator` doesn't exist yet (Law #2)._

## Step 2: Minimum Code (GREEN)

```csharp
public class DiscountCalculator {
    public decimal GetDiscount(decimal amount, User user) {
        return 10.0m; // Hardcoded to pass the test as fast as possible (Law #3)
    }
}
```

## Step 3: Refactor for Reality

Now we add a second test for a "Standard" user, which will fail. We then refactor the logic to handle both cases properly.

---

## 5. Benefits of TDD

- The Safety Net: You can refactor massive parts of the system with the push of a button to ensure nothing broke.
- Design Tool: TDD forces you to think about the interface (how the code is used) before the implementation (how it works). This naturally leads to loose coupling.
- Self-Documenting: The test suite becomes a living document of how the system is supposed to behave.
- Reduced Debugging: Because you only write a few lines of code at a time, if something breaks, you know exactly where the error is.

---

## 6. Common Pitfalls

- Testing Implementation, Not Behavior: If you test private methods or internal state, your tests will break every time you refactor, even if the result is the same. Test the public API.
- Skipping the Refactor Step: This leads to "TDD-induced spaghetti code," where you have thousands of tests but the production code is a mess.
- Slow Tests: If the suite takes 10 minutes to run, developers will stop doing TDD. Keep unit tests isolated from DBs and APIs.

---

> [!summary] Final Thought  
> TDD is not about "Testing." It is a design philosophy. It moves the act of verification from the end of the project to the very beginning, ensuring that every line of code written has a reason to exist.

---

### ==Create new test project:==

This step-by-step guide follows the Red-Green-Refactor cycle to build a simple `StringCalculator` that adds numbers from a string (e.g., `"1,2"` returns `3`).

## Phase 0: Setup in Visual Studio

1. Create the Solution:
    
    - Open VS → Create a new project.
    - Search for Blank Solution and name it `TDD_Demo`.
    
2. Add the Logic Project:
    
    - Right-click Solution → Add → New Project → Class Library (.NET 8/9).
    - Name it `CalculatorLibrary`.
    
3. Add the Test Project:
    
    - Right-click Solution → Add → New Project → xUnit Test Project.
    - Name it `CalculatorLibrary.Tests`.
    
4. Connect them:
    
    - In the Test project, right-click Dependencies → Add Project Reference → Check `CalculatorLibrary`.
    

---

## Phase 1: The First Test (RED)

We want a function that returns `0` if the string is empty.

1. Write the Test (in `UnitTest1.cs`):

```csharp
using CalculatorLibrary;

namespace CalculatorLibrary.Tests;

public class StringCalculatorTests
{
    [Fact]
    public void Add_EmptyString_ReturnsZero()
    {
        // Arrange
        var calculator = new StringCalculator();

        // Act
        int result = calculator.Add("");

        // Assert
        Assert.Equal(0, result);
    }
}
```

2. Run the Test:

- Press `Ctrl + R, A` (Run all tests).
- Result: It fails to even compile because `StringCalculator` doesn't exist. This is your first RED.

---

## Phase 2: Make it Pass (GREEN)

1. Create the Class (in the `CalculatorLibrary` project):

```csharp
namespace CalculatorLibrary;

public class StringCalculator
{
    public int Add(string numbers)
    {
        return 0; // Simplest code to pass
    }
}
```

2. Run the Test:

- Result: GREEN (Passed).

---

## Phase 3: Add More Logic (The RED Cycle Again)

Now we want `"1"` to return `1`.

1. Add a new Test:

```csharp
[Fact]
public void Add_SingleNumber_ReturnsThatNumber()
{
    var calculator = new StringCalculator();
    int result = calculator.Add("1");
    Assert.Equal(1, result);
}
```

2. Run Tests:

- The first test passes, but the second fails (Expected 1, got 0). RED.

3. Update Logic to GREEN:

```csharp
public int Add(string numbers)
{
    if (string.IsNullOrEmpty(numbers)) return 0;
    return int.Parse(numbers); 
}
```

4. Run Tests: All GREEN.

---

## Phase 4: Refactor

Now we add `"1,2"` (Should return `3`).

1. The Test (RED):

```csharp
[Fact]
public void Add_TwoNumbers_ReturnsSum()
{
    var calculator = new StringCalculator();
    int result = calculator.Add("1,2");
    Assert.Equal(3, result);
}
```

2. The Code (GREEN):

```csharp
public int Add(string numbers)
{
    if (string.IsNullOrEmpty(numbers)) return 0;

    var parts = numbers.Split(',');
    int total = 0;
    foreach (var part in parts)
    {
        total += int.Parse(part);
    }
    return total;
}
```

3. The Refactor:  
Now that we have 3 tests protecting us, we can make the code "cleaner" using LINQ:

```csharp
public int Add(string numbers)
{
    if (string.IsNullOrEmpty(numbers)) return 0;

    return numbers.Split(',')
                  .Select(int.Parse)
                  .Sum();
}
```

4. Final Check: Run all tests. If they are still green, your refactor was successful!

---

## Useful Shortcuts in VS 2026

- `Ctrl + R, T`: Run the test your cursor is currently in.
- `Ctrl + R, A`: Run every test in the solution.
- `Alt + Shift + Enter`: Live Testing mode (runs tests automatically as you type).

