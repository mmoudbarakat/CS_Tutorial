
## 🌲 The Binary Search Tree (BST) Data Structure

### A. C# Example

A **Binary Search Tree** is a node-based binary tree data structure that maintains a strict internal ordering rule:

- The **left subtree** of a node contains only nodes with keys **less than** the node's key.
    
- The **right subtree** of a node contains only nodes with keys **greater than** the node's key.
    
- Both the left and right subtrees must also be binary search trees.
    

Unlike a Heap, a BST allows for highly efficient searching because it eliminates half of the remaining tree branches with every single comparison step.

Here is a full C# implementation of a Binary Search Tree:

C#

```cs
using System;

public class BSTNode
{
    public int Key { get; set; }
    public BSTNode Left { get; set; }
    public BSTNode Right { get; set; }

    public BSTNode(int item)
    {
        Key = item;
        Left = Right = null;
    }
}

public class BinarySearchTree
{
    public BSTNode Root { get; private set; }

    // 1. Insert Operation (O(log n) average)
    public void Insert(int key)
    {
        Root = InsertRec(Root, key);
    }

    private BSTNode InsertRec(BSTNode root, int key)
    {
        if (root == null)
        {
            return new BSTNode(key);
        }

        if (key < root.Key)
            root.Left = InsertRec(root.Left, key);
        else if (key > root.Key)
            root.Right = InsertRec(root.Right, key);

        return root;
    }

    // 2. Search Operation (O(log n) average)
    public bool Search(int key)
    {
        return SearchRec(Root, key) != null;
    }

    private BSTNode SearchRec(BSTNode root, int key)
    {
        if (root == null || root.Key == key)
            return root;

        if (key < root.Key)
            return SearchRec(root.Left, key);

        return SearchRec(root.Right, key);
    }

    // 3. In-Order Traversal (Prints elements in sorted ascending order)
    public void InOrder()
    {
        InOrderRec(Root);
        Console.WriteLine();
    }

    private void InOrderRec(BSTNode root)
    {
        if (root != null)
        {
            InOrderRec(root.Left);
            Console.Write($"{root.Key} ");
            InOrderRec(root.Right);
        }
    }
}
```

#### Visual Structural Drawing (BST Node Layout)

Notice how every single node strictly obeys the boundary constraint: left is smaller, right is larger.

Plaintext

```
               ┌───[50]───┐                  <-- Root Node
               │          │
               ▼          ▼
            ┌─[30]─┐     [70]─┐              <-- All nodes left of [50] are < 50
            │      │          │              <-- All nodes right of [50] are > 50
            ▼      ▼          ▼
          [20]    [40]       [80]

  Structural Storage Linkages:
  Node Object -> { int Key; BSTNode LeftPointer; BSTNode RightPointer; }
```

---

### B. Metaphor

> [!info] **The High-Low Guessing Game** 🔢
> 
> Imagine someone asks you to guess a secret number between 1 and 100. You guess **50**. They reply, _"Higher!"_
> 
> Because you know how numbers are ordered, you instantly throw away the entire block of numbers from 1 to 50; you don't waste time checking 12, 34, or 45. Your next strategic guess is **75**. They say, _"Lower!"_ Now you throw away everything from 76 to 100.
> 
> A Binary Search Tree is that exact guessing game frozen into a physical physical data structure. Every single node acts as a fork in the road where you ask a single question: _"Is my target higher or lower than this spot?"_ You make one turn and completely bypass an entire forest of data without ever looking at it.

---

### C. Pseudo Code Operations

#### 1. Search (Lookup)

Plaintext

```c
procedure search(root : BSTNode, target : int)
    if root == null or root.Key == target then
        return root
    end if
    
    if target < root.Key then
        return search(root.Left, target)
    else
        return search(root.Right, target)
    end if
end procedure
```

#### 2. Insert (Add Node)

Plaintext

```c
procedure insert(root : BSTNode, value : int)
    if root == null then
        return create BSTNode(value)
    end if
    
    if value < root.Key then
        root.Left := insert(root.Left, value)
    else if value > root.Key then
        root.Right := insert(root.Right, value)
    end if
    
    return root
end procedure
```

#### 3. Delete (Remove Node)

Deletion has three unique topological conditions: deleting a leaf (0 children), deleting a node with 1 child, or deleting a node with 2 children.

Plaintext

```c
procedure deleteNode(root : BSTNode, key : int)
    if root == null then return root end if
    
    if key < root.Key then
        root.Left := deleteNode(root.Left, key)
    else if key > root.Key then
        root.Right := deleteNode(root.Right, key)
    else
        // Node found! Handle cases:
        // Case 1 & 2: 0 or 1 child
        if root.Left == null then return root.Right end if
        if root.Right == null then return root.Left end if
        
        // Case 3: 2 children. Find the In-Order Successor (smallest in right subtree)
        successor := minValueNode(root.Right)
        root.Key := successor.Key
        root.Right := deleteNode(root.Right, successor.Key)
    end if
    return root
end procedure

procedure minValueNode(node : BSTNode)
    current := node
    while current.Left != null do
        current := current.Left
    end while
    return current
end procedure
```

---

### D. Differences From Similar Data Structures

#### 1. Binary Search Tree (BST) vs. Binary Heap

- **Ordering Trait:** A BST separates data horizontally (left items $<$ parent $<$ right items), facilitating precise, specific lookups anywhere in the collection. A Heap sorts vertically (parent $\ge$ children), with zero horizontal guarantees, optimizing it strictly to expose the single maximum or minimum value.
    
- **Search Traversal:** Searching for an arbitrary item takes $O(\log n)$ average time in a BST, but demands an unoptimized $O(n)$ linear sweep across a Heap array.
    

#### 2. Binary Search Tree (BST) vs. Array / List

- To keep a sequential array perfectly ordered during insertions, elements must manually slide, creating an $O(n)$ operational drag. A BST handles dynamic structural insertions and lookups inside rapid $O(\log n)$ operational loops without any block movement pointer math.
    

---

### E. Time Complexity

> [!warning] **The Degenerate Tree Flaw**
> 
> If you insert items into a standard BST in strict sorted order (e.g., `10, 20, 30, 40`), the tree fails to branch and instead forms a single straight chain. It behaves exactly like a Singly Linked List, collapsing performance metrics down to $O(n)$. Advanced self-balancing variations (like AVL Trees or Red-Black Trees) prevent this structural flaw.

|**Operation**|**Average Case**|**Worst Case (Unbalanced)**|**Description**|
|---|---|---|---|
|**Search**|$O(\log n)$|$O(n)$|Trims half of the subtree paths per step when balanced.|
|**Insert**|$O(\log n)$|$O(n)$|Navigates down tree branches to discover a blank leaf target.|
|**Delete**|$O(\log n)$|$O(n)$|Locates target node and links sub-branches cleanly.|
|**Space Complexity**|$O(n)$|$O(n)$|Scaled to the memory required to host active node links.|


![[bst1.webp]]![[bst2.webp]]
