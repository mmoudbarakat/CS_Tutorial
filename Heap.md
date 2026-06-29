

## 📐 The Heap Data Structure

### A. C# Example

A **Heap** is a specialized, tree-based data structure that satisfies the **Heap Property**:

- In a **Max-Heap**, for any given node $i$, the value of $i$ is greater than or equal to the values of its children. The highest value is always at the root.
    
- In a **Min-Heap**, the value of $i$ is less than or equal to the values of its children. The lowest value is always at the root.
    

It is a _complete binary tree_, meaning all levels are completely filled except possibly the lowest, which is filled from left to right. This structural completeness allows a heap to be implemented efficiently using a flat, contiguous **Array** instead of complex node pointer objects.

Here is a full C# implementation of a Max-Heap:

C#

```cs
using System;
using System.Collections.Generic;

public class MaxHeap
{
    private List<int> heap = new List<int>();

    // 1. Insert Operation (O(log n))
    public void Insert(int value)
    {
        heap.Add(value);
        HeapifyUp(heap.Count - 1);
    }

    // 2. Extract Max Operation (O(log n)) - Removes and returns the root
    public int ExtractMax()
    {
        if (heap.Count == 0) throw new InvalidOperationException("Heap is empty.");

        int maxValue = heap[0];
        int lastIndex = heap.Count - 1;

        // Move the last element to the root and remove the last slot
        heap[0] = heap[lastIndex];
        heap.RemoveAt(lastIndex);

        // Bubble down to restore heap property
        if (heap.Count > 0)
        {
            HeapifyDown(0);
        }

        return maxValue;
    }

    // 3. Peek Operation (O(1)) - Returns the root without removing it
    public int PeekMax()
    {
        if (heap.Count == 0) throw new InvalidOperationException("Heap is empty.");
        return heap[0];
    }

    private void HeapifyUp(int index)
    {
        while (index > 0)
        {
            int parentIndex = (index - 1) / 2;
            if (heap[index] <= heap[parentIndex]) break;

            Swap(index, parentIndex);
            index = parentIndex;
        }
    }

    private void HeapifyDown(int index)
    {
        int lastIndex = heap.Count - 1;

        while (true)
        {
            int leftChild = 2 * index + 1;
            int rightChild = 2 * index + 2;
            int largest = index;

            if (leftChild <= lastIndex && heap[leftChild] > heap[largest])
                largest = leftChild;

            if (rightChild <= lastIndex && heap[rightChild] > heap[largest])
                largest = rightChild;

            if (largest == index) break;

            Swap(index, largest);
            index = largest;
        }
    }

    private void Swap(int i, int j)
    {
        int temp = heap[i];
        heap[i] = heap[j];
        heap[j] = temp;
    }
}
```

#### Visual Structural Drawing (Max-Heap Layout)

Notice how the logical tree maps directly to sequential indices in an array.

Plaintext

```
        Logical Tree View (Max-Heap)               Physical Sequential Array Mapping
               ┌───[90]───┐                        ┌────┬────┬────┬────┬────┐
               │  (Root)  │                        │ 90 │ 85 │ 70 │ 60 │ 55 │
               ▼          ▼                        └────┴────┴────┴────┴────┘
            ┌─[85]─┐     [70]                       0    1    2    3    4
            │      │                                
            ▼      ▼                               Mathematical Traversal Formulas:
          [60]    [55]                             • Parent(i)      = (i - 1) / 2
                                                   • LeftChild(i)  = 2 * i + 1
                                                   • RightChild(i) = 2 * i + 2
```

---

### B. Metaphor

> [!info] **The Corporate Ladder Pyramid** 👑
> 
> Imagine a strictly run corporate hierarchy. The Chief Executive Officer (CEO) is the highest-paid individual and sits at the absolute top of the pyramid (**Root**). Directly beneath them are Vice Presidents, and beneath them are regional managers.
> 
> A regional manager's salary can never surpass their immediate VP's salary, and a VP's salary can never surpass the CEO's. VPs on the exact same tier don't care who makes more than the other (meaning left and right siblings aren't sorted relative to each other). If the CEO suddenly quits (**ExtractMax**), the most junior intern at the very bottom corner of the company is temporarily dragged to the executive desk, and the board systematically compares them against the executives beneath them, demoting the intern down step-by-step (**Heapify-Down**) until they settle into an accurate position matching their true competence level.

---

### C. Pseudo Code Operations

Below is the standard procedural logic for a flat array implementation of a Max-Heap.

#### 1. Insert (Add to Heap)

Plaintext

```c
procedure insert(A : Array, value : Element)
    append value to the end of A
    currentIndex := length(A) - 1
    
    // Heapify-Up
    while currentIndex > 0 do
        parentIndex := (currentIndex - 1) / 2
        if A[currentIndex] <= A[parentIndex] then
            break // Heap property satisfied
        end if
        swap(A[currentIndex], A[parentIndex])
        currentIndex := parentIndex
    end while
end procedure
```

#### 2. ExtractMax (Remove and Return Root)

Plaintext

```c
procedure extractMax(A : Array)
    if length(A) == 0 then return "Underflow Error"
    
    maxValue := A[0]
    lastValue := A[length(A) - 1]
    A[0] := lastValue
    remove last slot from A
    
    // Heapify-Down
    currentIndex := 0
    while (2 * currentIndex + 1) < length(A) do
        leftChild := 2 * currentIndex + 1
        rightChild := 2 * currentIndex + 2
        largest := currentIndex
        
        if A[leftChild] > A[largest] then
            largest := leftChild
        end if
        if rightChild < length(A) and A[rightChild] > A[largest] then
            largest := rightChild
        end if
        
        if largest == currentIndex then
            break // Structural property restored
        end if
        
        swap(A[currentIndex], A[largest])
        currentIndex := largest
    end while
    
    return maxValue
end procedure
```

#### 3. Search & Random Access (Non-Standard Operations)

> [!warning] **Design Constraint Violation**
> 
> Heaps do not maintain ordering guarantees between sibling nodes or across different tree sub-branches. To find an arbitrary element, you cannot utilize binary search logic; you are forced to perform a complete linear search across the entire underlying array layout.

Plaintext

```c
procedure search(A : Array, target : Element)
    for i := 0 to length(A) - 1 do
        if A[i] == target then
            return true // Element exists within array
        end if
    end for
    return false
end procedure
```

---

### D. Differences From Similar Data Structures

#### 1. Binary Heap vs. Binary Search Tree (BST)

- **Ordering Strictness:** A BST enforces a strict left-to-right ordering (all left descendants $<$ parent $<$ all right descendants), allowing for fast $O(\log n)$ search operations. A Heap enforces a top-down vertical ordering restriction (parent $\ge$ children). Siblings have no structural ordering constraints.
    
- **Search Capability:** Searching for a generic element takes $O(\log n)$ in a balanced BST but requires an $O(n)$ linear scan in a Heap. Heaps are optimized strictly to track the extreme maximum or minimum value.
    

#### 2. Binary Heap vs. Standard Array / List

- While a heap uses an array as its internal storage structure, it restricts direct element overrides. Overwriting an arbitrary index inside a heap would break the vertical heap relationship structure, requiring explicit manual adjustments (`HeapifyUp` or `HeapifyDown`) to repair.
    

---

### E. Time Complexity

|**Operation**|**Worst Case**|**Best Case**|**Description**|
|---|---|---|---|
|**Insert**|$O(\log n)$|$O(1)$|Swaps elements upward along a single tree path length.|
|**Extract Max/Min**|$O(\log n)$|$O(\log n)$|Truncating the end and rebuilding down takes logarithmic time.|
|**Peek Max/Min**|$O(1)$|$O(1)$|The extreme value is always positioned at array index `0`.|
|**Search by Value**|$O(n)$|$O(1)$|Requires linearly scanning through unsorted sibling paths.|
|**Build Heap**|$O(n)$|$O(n)$|Converting an unsorted array into a valid heap using Floyd's algorithm.|
|**Space Complexity**|$O(n)$|$O(n)$|Zero extra structural pointer overhead; memory matches the raw data array scale.|
![[memory_allocation.webp]]