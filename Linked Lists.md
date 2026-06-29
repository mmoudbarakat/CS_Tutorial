
## 🔗 The Linked List Data Structure

### A. C# Example

A linked list is a linear data structure where elements are not stored in contiguous memory locations. Instead, each element is a separate object called a **Node**, which contains a data field and a reference (or pointer) to the next node in the sequence.

C#

```cs
using System;

// 1. Define the Node structure
public class Node
{
    public int Data { get; set; }
    public Node Next { get; set; }

    public Node(int data)
    {
        this.Data = data;
        this.Next = null;
    }
}

// 2. Define the Singly Linked List
public class LinkedList
{
    public Node Head { get; private set; }

    // Add node to the end of the list
    public void Append(int data)
    {
        Node newNode = new Node(data);
        if (Head == null)
        {
            Head = newNode;
            return;
        }

        Node current = Head;
        while (current.Next != null)
        {
            current = current.Next;
        }
        current.Next = newNode;
    }

    // Print all nodes in the list
    public void Display()
    {
        Node current = Head;
        while (current != null)
        {
            Console.Write($"{current.Data} -> ");
            current = current.Next;
        }
        Console.WriteLine("null");
    }
}
```

#### Visual Structural Drawing

Unlike arrays, nodes are scattered arbitrarily in memory. They maintain an ordered sequence exclusively through their pointers.

Plaintext

```
 Head Pointer
   │
   ▼
 ┌───────────┐      ┌───────────┐      ┌───────────┐
 │ Data: 12  │      │ Data: 99  │      │ Data: 37  │
 ├───────────┤      ├───────────┤      ├───────────┤
 │ Next: ────┼────> │ Next: ────┼────> │ Next: null│ 
 └───────────┘      └───────────┘      └───────────┘
   Node #1            Node #2            Node #3
 (Memory: 0x04)     (Memory: 0x8F)     (Memory: 0x12)
```

---

### B. Metaphor

> [!info] **The Scavenger Hunt** 🗺️
> 
> Imagine a high-stakes city-wide scavenger hunt. You are given a clue card at the starting location (**Head**). This card does not tell you where the final treasure is, nor does it tell you where any other clues are located. It only contains a single piece of text (**Data**) and the physical address of the very next clue box (**Next Pointer**). To find a specific item or reach the end, you cannot skip ahead; you must physically travel from one address to the next, tracing the chain link by link.

---

### C. Pseudo Code Operations

#### 1. Traversal / Access by Index

Plaintext

```c
procedure accessAt(Head : Node, index : int)
    current := Head
    count := 0
    while current != null do
        if count == index then
            return current.Data
        end if
        count := count + 1
        current := current.Next
    end while
    return -1 // Index out of bounds
end procedure
```

#### 2. Search by Value

Plaintext

```c
procedure search(Head : Node, target : int)
    current := Head
    while current != null do
        if current.Data == target then
            return true // Element found
        end if
        current := current.Next
    end while
    return false // Element not found
end procedure
```

#### 3. Insertion (At Start vs Middle)

Plaintext

```c
procedure insertAtStart(HeadRef : NodePointer, value : int)
    newNode := create Node(value)
    newNode.Next := HeadRef.Value
    HeadRef.Value := newNode
end procedure

procedure insertAfterNode(prevNode : Node, value : int)
    if prevNode == null then return
    newNode := create Node(value)
    newNode.Next := prevNode.Next
    prevNode.Next := newNode
end procedure
```

#### 4. Deletion

Plaintext

```c
procedure deleteValue(HeadRef : NodePointer, target : int)
    current := HeadRef.Value
    prev := null
    
    // If head node itself holds the target
    if current != null and current.Data == target then
        HeadRef.Value := current.Next
        return
    end if
    
    // Search for the target while tracking the previous node
    while current != null and current.Data != target do
        prev := current
        current := current.Next
    end while
    
    // If target was not present in the list
    if current == null then return
    
    // Unlink the node from the linked list
    prev.Next := current.Next
end procedure
```

---

### D. Differences From Similar Data Structures

#### 1. Linked List vs. Array

- **Memory Allocation:** Arrays are allocated in single, contiguous memory blocks at compile-time/runtime instantiation. Linked Lists are dynamically allocated node-by-node scattered throughout the heap memory space.
    
- **Resizing:** Arrays have fixed capacity limits (requiring a slow $O(n)$ copy operation to scale out). Linked lists are unboundedly dynamic; they can shrink or grow indefinitely as long as memory exists.
    
- **Cost of Manipulation:** Arrays require heavy pointer shifting to insert or delete elements at the front/middle. Linked lists simply rewrite two address pointers, keeping manipulation fluid.
    

#### 2. Linked List vs. Dynamic Array / `List<T>`

- While a `List<T>` wraps a standard array to offer dynamic growth behaviors, it still suffers from linear $O(n)$ performance bottlenecks during internal resizing operations and early-index item manipulation. Linked lists bypass these system pointer operations completely but exchange it for higher pointer overhead memory costs.
    

---

### E. Time Complexity

|**Operation**|**Worst Case**|**Best Case**|**Description**|
|---|---|---|---|
|**Access by Index**|$O(n)$|$O(1)$|Must traverse nodes sequentially from Head to find specific indices.|
|**Search by Value**|$O(n)$|$O(1)$|Must linear-scan sequential reference linkages down the list length.|
|**Insertion (Start)**|$O(1)$|$O(1)$|Uniquely efficient; requires editing only the root structural pointer address.|
|**Insertion (End)**|$O(n)$|$O(1)$|$O(n)$ unless a local reference pointer tail node track loop exists.|
|**Deletion (Start)**|$O(1)$|$O(1)$|Instant pointer shift mechanics to node next target.|
|**Deletion (Middle)**|$O(n)$|$O(1)$|The unlinking action is $O(1)$, but finding the predecessor takes $O(n)$ time.|
|**Space Complexity**|$O(n)$|$O(n)$|Higher overhead than arrays due to memory allocation for pointers.|

