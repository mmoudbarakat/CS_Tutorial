
## 🟨 The Queue Data Structure

### A. C# Example

A Queue is a linear data structure that follows the **FIFO (First In, First Out)** principle. The element that is added first is the first one to be removed. It exposes two distinct access boundaries: elements are inserted at the **Rear** (Enqueue) and removed from the **Front** (Dequeue).

C#

```cs
using System;
using System.Collections.Generic;

class QueueExample
{
    static void Main()
    {
        // 1. Initialization using C#'s built-in Queue collection
        Queue<string> printJobQueue = new Queue<string>();

        // 2. Enqueue Operation: Adding elements to the Rear
        printJobQueue.Enqueue("TaxStatement.pdf");
        printJobQueue.Enqueue("Resume.docx");
        printJobQueue.Enqueue("FlightTickets.pdf");

        // 3. Peek Operation: Inspecting the Front element without removing it
        Console.WriteLine($"Next in line to print (Front): {printJobQueue.Peek()}"); // Output: TaxStatement.pdf

        // 4. Dequeue Operation: Removing the Front element
        string processedJob = printJobQueue.Dequeue();
        Console.WriteLine($"Finished printing: {processedJob}"); // Output: TaxStatement.pdf

        // 5. Checking the new Front element
        Console.WriteLine($"New Next in line (Front): {printJobQueue.Peek()}"); // Output: Resume.docx
    }
}
```

#### Visual Structural Drawing

Unlike a stack, data enters from one end and exits from the completely opposite end, creating an open pipeline flow.

Plaintext

```
      Dequeue (Remove)                                      Enqueue (Insert)
     ◄─── [Front] ─── ┌───────┬───────┬───────┬───────┐ ─── [Rear] ◄───
                      │ Job 1 │ Job 2 │ Job 3 │ Job 4 │
                      └───────┴───────┴───────┴───────┘
                       Index 0   Index 1   Index 2   Index 3
```

---

### B. Metaphor

> [!info] **The Fast-Food Drive-Thru Lane** 🚗
> 
> Imagine driving your car into a single-lane fast-food drive-thru. The first car to pull up to the speaker box and place an order (**First In**) is naturally the very first car that arrives at the pickup window to pay and leave (**First Out**).
> 
> A trailing car cannot cut the line or exit the lane early from the front, nor can the first car magically reverse out of the entrance once trapped in the pipeline track. Everyone must wait their turn chronologically.

---

### C. Pseudo Code Operations

A standard queue is typically implemented using either a dynamic linked list or a fixed array (often wrap-around styled as a _Circular Queue_). Below is the standard array-based implementation tracking both a `front` and a `rear` index indicator.

#### 1. Enqueue (Insert at Rear)

Plaintext

```c
procedure enqueue(Queue : Array, frontRef : int, rearRef : int, maxSize : int, value : Element)
    if (rearRef + 1) % maxSize == frontRef then
        return "Queue Overflow Error" // Circular safety overflow check
    end if
    
    if frontRef == -1 then // If filling an completely empty structure
        frontRef := 0
    end if
    
    rearRef := (rearRef + 1) % maxSize
    Queue[rearRef] := value
end procedure
```

#### 2. Dequeue (Delete / Remove from Front)

Plaintext

```c
procedure dequeue(Queue : Array, frontRef : int, rearRef : int, maxSize : int)
    if frontRef == -1 then
        return "Queue Underflow Error" // Structure is empty
    end if
    
    dequeuedValue := Queue[frontRef]
    
    if frontRef == rearRef then // Queue has been completely emptied
        frontRef := -1
        rearRef := -1
    else
        frontRef := (frontRef + 1) % maxSize
    end if
    
    return dequeuedValue
end procedure
```

#### 3. Peek / Front (Access Current Front Element)

Plaintext

```c
procedure peek(Queue : Array, frontRef : int)
    if frontRef == -1 then
        return "Queue is Empty"
    end if
    return Queue[frontRef]
end procedure
```

#### 4. Search and Arbitrary Access (Non-Standard Operations)

> [!warning] **Design Constraint Violation**
> 
> Queues are _deliberately_ restricted to pipeline mechanics. To scan internally without destroying data alignment, you must iterate sequentially across indices from the `front` boundary up to the `rear` location.

Plaintext

```c
procedure search(Queue : Array, frontRef : int, rearRef : int, maxSize : int, target : Element)
    if frontRef == -1 then return false
    
    current := frontRef
    while true do
        if Queue[current] == target then
            return true
        end if
        if current == rearRef then break
        current := (current + 1) % maxSize
    end while
    
    return false
end procedure
```

---

### D. Differences From Similar Data Structures

#### 1. Queue vs. Stack

- **Processing Ordering:** A queue enforces **FIFO** (First In, First Out). A stack enforces **LIFO** (Last In, First Out), processing data in reverse-chronological order.
    
- **Access Points:** Queues manipulate data from two ends simultaneously (`Front` for extraction, `Rear` for ingestion). Stacks perform all operations via a single shared interface terminal (`Top`).
    

#### 2. Queue vs. Array / List

- **Access Restrictions:** Arrays allow unchecked $O(1)$ random lookups and cell overrides across any slot coordinate. Queues abstract away this random access capability completely, guaranteeing that elements can only enter chronologically at the back and exit at the front.
    

---

### E. Time Complexity

|**Operation**|**Worst Case**|**Best Case**|**Description**|
|---|---|---|---|
|**Enqueue (Insertion)**|$O(1)$|$O(1)$|Direct allocation at the tracked `rear` index pointer.|
|**Dequeue (Deletion)**|$O(1)$|$O(1)$|Immediate extraction and forward increment of the `front` index.|
|**Peek (Access Front)**|$O(1)$|$O(1)$|Read lookup instantly via the boundary tracking address offset.|
|**Search by Value**|$O(n)$|$O(1)$|Requires linearly scanning through up to $n$ active elements sequentially.|
|**Space Complexity**|$O(n)$|$O(n)$|Footprint is perfectly linear relative to the number of queued elements.|

![[CSharp-Queue.webp]]

## Hierarchy of Queue Class:

![[CSharp-Stack-Hierarchy.webp]]
