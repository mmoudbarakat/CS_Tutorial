
## 🟫 The Stack Data Structure

### A. C# Example

A Stack is a linear data structure that follows the **LIFO (Last In, First Out)** principle. The element that is added last is the first one to be removed. It restricts data access so that elements can only be added or removed from a single point called the **Top**.

C#

```cs
using System;
using System.Collections.Generic;

class StackExample
{
    static void Main()
    {![[CSharp-Stack-Hierarchy.webp]]

![[Stack-diagram.webp]]
        // 1. Initialization using C#'s built-in Stack collection
        Stack<string> browserHistory = new Stack<string>();

        // 2. Push Operation: Adding elements to the top
        browserHistory.Push("google.com");
        browserHistory.Push("github.com");
        browserHistory.Push("stackoverflow.com");

        // 3. Peek Operation: Looking at the top element without removing it
        Console.WriteLine($"Current Page (Top): {browserHistory.Peek()}"); // Output: stackoverflow.com

        // 4. Pop Operation: Removing the top element
        string poppedPage = browserHistory.Pop();
        Console.WriteLine($"Navigated back from: {poppedPage}"); // Output: stackoverflow.com

        // 5. Checking the new top element
        Console.WriteLine($"New Current Page (Top): {browserHistory.Peek()}"); // Output: github.com
    }
}
```

#### Visual Structural Drawing

Think of data entering and leaving from the exact same terminal point. Elements lower down are completely inaccessible until the items above them are cleared away.

Plaintext

```
      |               |
      |  [ Element3 ] |  <-- Top of Stack (Only accessible point)
      |---------------|
      |  [ Element2 ] |
      |---------------|
      |  [ Element1 ] |  <-- Bottom of Stack
      \_______________/
      
   Push: Element4 ➔ Enters from Top
   Pop:  Element3 ➔ Leaves from Top
```

---

### B. Metaphor

> [!info] **The Cafeteria Spring-Loaded Tray Dispenser** 🍽️
> 
> Imagine the metal tray dispenser in a university cafeteria. When the kitchen staff washes a new plate, they place it directly on **top** of the stack. The heavy weight pushes the entire column down into the counter. When a hungry student walks up, they can only grab the absolute top tray that is visible.
> 
> A student cannot magically reach down into the middle of the dispenser to pull out a tray from the bottom without completely crashing or removing every single tray stacked above it first.

---

### C. Pseudo Code Operations

A standard stack can be implemented using either an array or a linked list. Below is an array-based implementation pseudo-code tracking a `top` index variable initialized to `-1`.

#### 1. Push (Insert at Top)

Plaintext

```c
procedure push(Stack : Array, topRef : int, maxSize : int, value : Element)
    if topRef == maxSize - 1 then
        return "Stack Overflow Error" // Stack is fully saturated
    end if
    topRef := topRef + 1
    Stack[topRef] := value
end procedure
```

#### 2. Pop (Delete / Remove from Top)

Plaintext

```c
procedure pop(Stack : Array, topRef : int)
    if topRef == -1 then
        return "Stack Underflow Error" // Stack is empty
    end if
    poppedValue := Stack[topRef]
    topRef := topRef - 1
    return poppedValue
end procedure
```

#### 3. Peek / Top (Access Current Top Element)

Plaintext

```c
procedure peek(Stack : Array, topRef : int)
    if topRef == -1 then
        return "Stack is Empty"
    end if
    return Stack[topRef] // Simply read the value without altering the index pointer
end procedure
```

#### 4. Search and Arbitrary Access (Non-Standard Operations)

> [!warning] **Design Constraint Violation**
> 
> Stacks are _deliberately_ not designed for linear searches or random access by index position. To accomplish this, you must programmatically empty the stack completely into a temporary structure.

Plaintext

```c
procedure search(Stack : Array, topRef : int, target : Element)
    // To scan without destroying data, we iterate backwards from the top index
    for i := topRef down to 0 do
        if Stack[i] == target then
            return true // Found element within stack depths
        end if
    end for
    return false
end procedure
```

---

### D. Differences From Similar Data Structures

#### 1. Stack vs. Queue

- **Processing Ordering:** A stack enforces **LIFO** (Last In, First Out). A queue enforces **FIFO** (First In, First Out), mimicking a physical line at a retail counter where the first person to arrive is served first.
    
- **Access Points:** Stacks expose only _one_ structural end (Top). Queues expose _two_ separate operational ends (Elements are inserted at the Rear and removed from the Front).
    

#### 2. Stack vs. Array / List

- **Access Restrictions:** Arrays and lists allow unrestricted, arbitrary random lookups and updates at any internal coordinate via zero-based indexing in $O(1)$ time. A stack deliberately strips away this capability, exposing a highly disciplined interface that enforces a strict chronological ordering of data mutation.
    

---

### E. Time Complexity

|**Operation**|**Worst Case**|**Best Case**|**Description**|
|---|---|---|---|
|**Push (Insertion)**|$O(1)$|$O(1)$|Instantaneous increment of the top pointer index.|
|**Pop (Deletion)**|$O(1)$|$O(1)$|Instantaneous decrement of the top pointer index.|
|**Peek (Access Top)**|$O(1)$|$O(1)$|Simple immediate lookup at the precise offset location tracked by `top`.|
|**Search by Value**|$O(n)$|$O(1)$|Requires iterating down through up to $n$ elements to find a matching value.|
|**Space Complexity**|$O(n)$|$O(n)$|Scale is perfectly proportional to the collection footprint size.|

![[Stack-diagram.webp]]

## Hierarchy of Stack Class:

![[CSharp-Stack-Hierarchy.webp]]
