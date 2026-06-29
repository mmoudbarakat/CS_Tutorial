
## 👑 The Priority Queue Data Structure

### A. C# Example

A Priority Queue is an advanced extension of a standard queue. Instead of serving elements strictly on a first-come, first-served basis (**FIFO**), every element possesses an associated **priority value**. Elements with higher priorities are dequeued before elements with lower priorities.

In modern C#, this is efficiently backed by a binary heap structure via the built-in `PriorityQueue<TElement, TPriority>` collection.

C#

```cs
using System;
using System.Collections.Generic;

class PriorityQueueExample
{
    static void Main()
    {
        // 1. Initialization: Lower integer value = higher priority (Min-Heap behavior)
        // Format: PriorityQueue<Element_Type, Priority_Type>
        PriorityQueue<string, int> emergencyRoom = new PriorityQueue<string, int>();

        // 2. Enqueue Operation: Adding elements with an explicit priority rank
        emergencyRoom.Enqueue("Patient A (Broken Arm)", 3);
        emergencyRoom.Enqueue("Patient B (Heart Attack)", 1); // Highest Priority
        emergencyRoom.Enqueue("Patient C (Mild Fever)", 5);   // Lowest Priority
        emergencyRoom.Enqueue("Patient D (Deep Cut)", 2);

        // 3. Peek Operation: Inspecting the highest priority item without removing it
        Console.WriteLine($"Next patient to treat (Peek): {emergencyRoom.Peek()}"); 
        // Output: Patient B (Heart Attack)

        // 4. Dequeue Operation: Extracting the absolute highest priority element
        while (emergencyRoom.Count > 0)
        {
            string treatedPatient = emergencyRoom.Dequeue();
            Console.WriteLine($"Treating: {treatedPatient}");
        }
        // Execution Order Output:
        // 1. Patient B (Heart Attack) [Priority 1]
        // 2. Patient D (Deep Cut)     [Priority 2]
        // 3. Patient A (Broken Arm)   [Priority 3]
        // 4. Patient C (Mild Fever)   [Priority 5]
    }
}
```

#### Visual Structural Drawing (Binary Heap Layout)

Under the hood, a priority queue is typically structured as a **Binary Heap** (represented here as a Min-Heap tree where the lowest value sits at the top). It is structurally mapped flat inside a standard sequential array.

Plaintext

```
        Logical Tree Representation                Physical Array Index Mapping
               ┌───[1]───┐                         ┌───┬───┬───┬───┐
               │ (Heart) │                         │ 1 │ 2 │ 3 │ 5 │
               ▼         ▼                         └───┴───┴───┴───┘
            ┌─[2]─┐     [3]                         0   1   2   3
            │(Cut)│   (Broken)                    
            ▼     ▼                                Parent Index: (i - 1) / 2
          [5]    null                              Left Child:   (2 * i) + 1
        (Fever)                                    Right Child:  (2 * i) + 2
```

---

### B. Metaphor

> [!info] **The Airport Boarding Gate** ✈️
> 
> Imagine standing in line at an airport gate. In a standard queue, passengers board exactly in the chronological order they arrived.
> 
> A **Priority Queue** completely upends this. No matter how late the First-Class passengers or active military personnel arrive, they skip directly to the absolute front of the line. The gate agent checks the _priority tier printed on the ticket_, not the time stamp of arrival. A late-arriving First-Class passenger will always board ahead of an Economy passenger who has been waiting in line for hours.

---

### C. Pseudo Code Operations

The implementation below models a **Min-Priority Queue** utilizing an internal binary heap array structure.

#### 1. Enqueue / Insert (Push with Priority)

Plaintext

```c
procedure enqueue(Heap : Array, value : Element, priority : int)
    heapSize := length(Heap)
    newNode := create Node(value, priority)
    
    // Step 1: Place new node at the very end of the tree array
    Heap[heapSize] := newNode
    
    // Step 2: "Bubble Up" (Heapify-Up) to restore structural heap traits
    current := heapSize
    while current > 0 do
        parent := (current - 1) / 2
        if Heap[current].priority >= Heap[parent].priority then
            break // Priority structure is correct, exit loop
        end if
        swap(Heap[current], Heap[parent])
        current := parent
    end while
end procedure
```

#### 2. Dequeue / Extract-Min (Remove Highest Priority Element)

Plaintext

```c
procedure dequeue(Heap : Array)
    if length(Heap) == 0 then return "Queue Underflow Error"
    
    highestPriorityItem := Heap[0] // Root always holds the top priority
    lastItem := Heap[length(Heap) - 1]
    
    // Move last item to root position and truncate the end slot
    Heap[0] := lastItem
    remove last slot from Heap
    
    // "Bubble Down" (Heapify-Down) from the top root
    current := 0
    while (2 * current + 1) < length(Heap) do
        leftChild := (2 * current) + 1
        rightChild := (2 * current) + 2
        smallest := leftChild
        
        if rightChild < length(Heap) and Heap[rightChild].priority < Heap[leftChild].priority then
            smallest := rightChild
        end if
        
        if Heap[current].priority <= Heap[smallest].priority then
            break // Structural balance achieved
        end if
        
        swap(Heap[current], Heap[smallest])
        current := smallest
    end while
    
    return highestPriorityItem
end procedure
```

#### 3. Peek / Top (Access Top Priority Item)

Plaintext

```c
procedure peek(Heap : Array)
    if length(Heap) == 0 then return "Queue is Empty"
    return Heap[0].value // Instantly read the root node
end procedure
```

#### 4. Search and Arbitrary Access (Non-Standard Operations)

> [!warning] **Design Constraint Violation**
> 
> Sifting through a binary heap for an arbitrary target element breaks encapsulation boundaries and exhibits poor efficiency because nodes are only sorted relative to parents, not siblings.

Plaintext

```c
procedure search(Heap : Array, target : Element)
    // Must execute a linear scan across the flat array backbone
    for i := 0 to length(Heap) - 1 do
        if Heap[i].value == target then
            return true
        end if
    end for
    return false
end procedure
```

---

### D. Differences From Similar Data Structures

#### 1. Priority Queue vs. Standard Queue

- **Ordering Mechanics:** Standard queues operate strictly via **FIFO** temporal sequence mechanics. Priority Queues dynamically serve elements entirely based on an independent **priority metric value**, regardless of execution insertion order.
    

#### 2. Priority Queue vs. Sorted Array / Sorted List

- **Insertion Efficiency:** To keep a classic array perfectly sorted after a new insertion, elements must shift linearly, inflicting a costly $O(n)$ footprint. A Priority Queue leverages a binary tree-heap structure, allowing elements to bypass strict sequential ordering while guaranteeing top priority tracking in light, efficient $O(\log n)$ bounds.
    

---

### E. Time Complexity

|**Operation**|**Worst Case**|**Best Case**|**Description**|
|---|---|---|---|
|**Enqueue (Insertion)**|$O(\log n)$|$O(1)$|Element bubbles up along a single tree path length.|
|**Dequeue (Deletion)**|$O(\log n)$|$O(\log n)$|Root item extraction forces tree structural restructuring downward.|
|**Peek (Access Front)**|$O(1)$|$O(1)$|The highest priority is safely stored at array index location `0`.|
|**Search by Value**|$O(n)$|$O(1)$|Requires linearly scanning the unstructured array layers.|
|**Space Complexity**|$O(n)$|$O(n)$|Directly relative to total elements held inside tracking arrays.|
### PriorityQueue Hierarchy: 

![[PriorityQueue-Hierarchy.webp]]
