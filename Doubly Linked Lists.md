
## 🔗 The Doubly Linked List (DLL) Data Structure

### A. C# Example

A Doubly Linked List is an extension of the Singly Linked List. The crucial difference is that each **Node** contains **two pointers** (or references): one pointing to the _next_ node, and another pointing to the _previous_ node. This allows for bidirectional traversal.

C#

```cs
using System;

// 1. Define the Doubly Node structure
public class DbNode
{
    public int Data { get; set; }
    public DbNode Next { get; set; }
    public DbNode Prev { get; set; }

    public DbNode(int data)
    {
        this.Data = data;
        this.Next = null;
        this.Prev = null;
    }
}

// 2. Define the Doubly Linked List
public class DoublyLinkedList
{
    public DbNode Head { get; private set; }
    public DbNode Tail { get; private set; } // Tracks the end for O(1) rear operations

    // Add node to the end of the list
    public void Append(int data)
    {
        DbNode newNode = new DbNode(data);
        if (Head == null)
        {
            Head = newNode;
            Tail = newNode;
            return;
        }

        Tail.Next = newNode;
        newNode.Prev = Tail;
        Tail = newNode; // Move tail pointer to the new end
    }

    // Print all nodes from Head to Tail
    public void DisplayForward()
    {
        DbNode current = Head;
        while (current != null)
        {
            Console.Write($"{current.Data} <-> ");
            current = current.Next;
        }
        Console.WriteLine("null");
    }
}
```

#### Visual Structural Drawing

Because each node contains pointers to both its neighbors, the structure forms a two-way chain.

Plaintext

```
      Head Pointer                                                 Tail Pointer
        │                                                                │
        ▼                                                                ▼
   ┌───────────┐                ┌───────────┐                ┌───────────┐
   │ Prev: null│ <──────────────┼─ Prev     │ <──────────────┼─ Prev     │
   ├───────────┤                ├───────────┤                ├───────────┤
   │ Data: 45  │                │ Data: 11  │                │ Data: 82  │
   ├───────────┤                ├───────────┤                ├───────────┤
   │ Next ─────┼──────────────> │ Next ─────┼──────────────> │ Next: null│
   └───────────┘                └───────────┘                └───────────┘
     Node #1                      Node #2                      Node #3
```

---

### B. Metaphor

> [!info] **The Choreographed Conga Line** 💃🕺
> 
> Imagine a line of dancers on a stage. In a singly linked list, each dancer only places their hand on the shoulder of the person _ahead_ of them; they have no structural touchpoint to who is behind them.
> 
> In a **Doubly Linked List**, each dancer places their left hand on the shoulder of the person **in front** of them (`Next`), and their right hand on the shoulder of the person **behind** them (`Prev`). This dual connection means the line can fluidly pass signals or backstep positions in either direction without anyone losing their place or needing to go back to the front of the line to pass a message backwards.

---

### C. Pseudo Code Operations

#### 1. Bidirectional Access / Traversal

Since we have both `Head` and `Tail` references, we can optimize traversal based on whether the target index is closer to the front or back.

Plaintext

```c
procedure accessAt(Head : DbNode, Tail : DbNode, length : int, index : int)
    if index < 0 or index >= length then return -1
    
    // Optimize: Start from Head if index is in the first half
    if index < length / 2 then
        current := Head
        for i := 0 to index - 1 do
            current := current.Next
        end for
    else // Start from Tail if index is closer to the end
        current := Tail
        for i := length - 1 down to index + 1 do
            current := current.Prev
        end for
    end if
    return current.Data
end procedure
```

#### 2. Search by Value

Plaintext

```c
procedure search(Head : DbNode, target : int)
    current := Head
    while current != null do
        if current.Data == target then
            return true
        end if
        current := current.Next
    end while
    return false
end procedure
```

#### 3. Insertion (At a Specific Position)

Plaintext

```c
procedure insertBefore(HeadRef : DbNodePointer, targetNode : DbNode, value : int)
    if targetNode == null then return
    
    newNode := create DbNode(value)
    newNode.Next := targetNode
    newNode.Prev := targetNode.Prev
    
    if targetNode.Prev != null then
        targetNode.Prev.Next := newNode
    else
        HeadRef.Value := newNode // Target was Head
    end if
    
    targetNode.Prev := newNode
end procedure
```

#### 4. Deletion (Direct O(1) Removal given Node Reference)

Unlike a singly linked list, if you hold a pointer to the target node, you do not need to look up its predecessor to unlink it.

Plaintext

```c
procedure deleteNode(HeadRef : DbNodePointer, TailRef : DbNodePointer, delNode : DbNode)
    if delNode == null then return

    // If node to be deleted is the Head node
    if HeadRef.Value == delNode then
        HeadRef.Value := delNode.Next
    end if

    // If node to be deleted is the Tail node
    if TailRef.Value == delNode then
        TailRef.Value := delNode.Prev
    end if

    // Change next only if node to be deleted is NOT the last node
    if delNode.Next != null then
        delNode.Next.Prev := delNode.Prev
    end if

    // Change prev only if node to be deleted is NOT the first node
    if delNode.Prev != null then
        delNode.Prev.Next := delNode.Next
    end if
end procedure
```

---

### D. Differences From Similar Data Structures

#### 1. Doubly Linked List vs. Singly Linked List

- **Memory Overhead:** A DLL node requires an extra pointer slot (`Prev`). On a 64-bit system architecture, this adds 8 bytes of storage overhead per node.
    
- **Directionality:** Singly linked lists can only be traversed forward. DLLs allow seamless two-way exploration (`Next` and `Prev`), making algorithms like reverse-iteration effortless.
    
- **Deletion Mechanics:** Deleting a given mid-sequence node in a singly linked list forces an $O(n)$ scan from the start to find the element before it. A DLL can manipulate adjacent pointers instantly in $O(1)$ space and time given the node address.
    

#### 2. Doubly Linked List vs. Array

- Just like a singly linked list, a DLL provides dynamic structural resizing without requiring block re-allocations or block shifting copy loops. However, it still yields poor search lookup efficiency compared to an array's instantaneous direct index mathematical address translation.
    

---

### E. Time Complexity

|**Operation**|**Worst Case**|**Best Case**|**Description**|
|---|---|---|---|
|**Access by Index**|$O(n)$|$O(1)$|Can cut search time in half by checking if index is closer to Head or Tail, but remains linear scale.|
|**Search by Value**|$O(n)$|$O(1)$|Linearly scans nodes sequentially across reference chains.|
|**Insertion (Head/Tail)**|$O(1)$|$O(1)$|Instantaneous if explicit tracking references to Head and Tail exist.|
|**Insertion (Middle)**|$O(n)$|$O(1)$|$O(1)$ to rewire pointers, but takes $O(n)$ to navigate to the structural index spot first.|
|**Deletion (Head/Tail)**|$O(1)$|$O(1)$|Direct, immediate boundary pointer adjustment.|
|**Deletion (Given Node)**|$O(1)$|$O(1)$|**Unique Advantage:** Unlinks directly using internal `Prev` and `Next` properties without tracking from root.|
|**Space Complexity**|$O(n)$|$O(n)$|Higher structural overhead per item than arrays and singly linked lists.|
