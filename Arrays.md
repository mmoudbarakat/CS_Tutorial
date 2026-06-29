
## 🟥 The Array Data Structure

### A. C# Example

An array is a contiguous block of memory holding a fixed number of values of a single type. Elements are accessed via a zero-based index.

C#

```cs
using System;

class ArrayExample
{
    static void Main()
    {
        // 1. Declaration and Initialization
        int[] scores = new int[5] { 90, 85, 93, 77, 88 };

        // 2. Accessing elements via index (O(1) Time Complexity)
        int firstScore = scores[0]; 
        Console.WriteLine($"First Score: {firstScore}"); // Output: 90

        // 3. Modifying an element
        scores[3] = 95; // Changes 77 to 95

        // 4. Iterating through the array
        Console.WriteLine("All scores:");
        for (int i = 0; i < scores.Length; i++)
        {
            Console.WriteLine($"Index {i}: {scores[i]}");
        }
    }
}
```

#### Visual Structural Drawing

Below is a architectural map of how this `scores` array is laid out in system memory. Notice how the indices directly map to sequential, neighboring slots.

Plaintext

```c
 Array Block in Memory (Contiguous Slots)
 ┌───────────┬───────────┬───────────┬───────────┬───────────┐
 │    90     │    85     │    93     │    95     │    88     │  <-- Values
 └───────────┴───────────┴───────────┴───────────┴───────────┘
   Index [0]   Index [1]   Index [2]   Index [3]   Index [4]   <-- Offsets
 
 Pointer Math: Base Address + (Index * Element Size in Bytes)
```

### B. Metaphor

> [!info] **The Assigned Post Office Boxes** 📭
> 
> Imagine a massive, unmovable wall of post office boxes built into a wall. The entire unit is bolted to the ground, meaning you cannot dynamically add a new box in the middle of it (Fixed Size). Each box is identical in size and holds the exact same type of item. Crucially, every box has a sequential number painted on it starting from `0`. If you have the box number, you can walk directly to it instantly without searching through any other boxes.

### C. Pseudo Code

Because arrays are built-in hardware-level structures, operations focus on manual manipulation like insertion, deletion, and searching.

#### Linear Search (Finding an item)

Plaintext

```c
procedure linearSearch(A : Array, target : Element)
    n := length(A)
    for i := 0 to n - 1 do
        if A[i] == target then
            return i // Return index where target is found
        end if
    end for
    return -1 // Target not found
end procedure
```

#### Inserting into a Fixed-Size Array (Shifting elements)

Plaintext

```c
procedure insertAt(A : Array, index : int, value : Element)
    n := length(A)
    // Shift elements to the right to make room from the end down to index
    for i := n - 1 down to index + 1 do
        A[i] := A[i - 1]
    end for
    A[index] := value
end procedure
```

---

## 📈 Array Operations Performance Cheatsheet

|**Operation**|**Time Complexity**|**Description**|
|---|---|---|
|**Access by Index**|$O(1)$|Instant lookup via direct memory address pointer math.|
|**Search by Value**|$O(n)$|Must scan sequentially through elements in the worst case.|
|**Insertion (Start/Middle)**|$O(n)$|Requires shifting subsequent elements rightward in memory.|
|**Deletion (Start/Middle)**|$O(n)$|Requires shifting subsequent elements leftward to fill the gap.|
|**Space Complexity**|$O(n)$|Memory scale matches the allocated fixed size of the array.|

