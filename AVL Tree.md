
## 🌲 The AVL Tree Data Structure

### A. C# Example

An **AVL Tree** (named after inventors **A**delson-Velsky and **L**andis) is a self-balancing Binary Search Tree (BST). It introduces a strict structural constraint: for any given node, the heights of its left and right subtrees can differ by **at most 1**.

If at any point after an insertion or deletion the height difference (known as the **Balance Factor**) becomes greater than 1 or less than -1, a restructuring operation called a **Tree Rotation** is performed to restore structural balance. This completely neutralizes the worst-case degenerate ("linked list") flaw of a standard BST.

Here is a full C# implementation of an AVL Tree showcasing the core self-balancing `Insert` logic:

C#

```cs
using System;

public class AVLNode
{
    public int Key { get; set; }
    public int Height { get; set; }
    public AVLNode Left { get; set; }
    public AVLNode Right { get; set; }

    public AVLNode(int d)
    {
        Key = d;
        Height = 1; // New nodes are initially added at leaf level
    }
}

public class AVLTree
{
    public AVLNode Root { get; private set; }

    // Helper to get node height safely
    private int GetHeight(AVLNode node) => node == null ? 0 : node.Height;

    // Calculate balance factor
    private int GetBalance(AVLNode node) => node == null ? 0 : GetHeight(node.Left) - GetHeight(node.Right);

    // Right Rotate (Used for Left-Left Heavy scenarios)
    private AVLNode RightRotate(AVLNode y)
    {
        AVLNode x = y.Left;
        AVLNode T2 = x.Right;

        // Perform rotation
        x.Right = y;
        y.Left = T2;

        // Update heights
        y.Height = Math.Max(GetHeight(y.Left), GetHeight(y.Right)) + 1;
        x.Height = Math.Max(GetHeight(x.Left), GetHeight(x.Right)) + 1;

        return x; // New root of subtree
    }

    // Left Rotate (Used for Right-Right Heavy scenarios)
    private AVLNode LeftRotate(AVLNode x)
    {
        AVLNode y = x.Right;
        AVLNode T2 = y.Left;

        // Perform rotation
        y.Left = x;
        x.Right = T2;

        // Update heights
        x.Height = Math.Max(GetHeight(x.Left), GetHeight(x.Right)) + 1;
        y.Height = Math.Max(GetHeight(y.Left), GetHeight(y.Right)) + 1;

        return y; // New root of subtree
    }

    // Self-Balancing Insert Operation (O(log n))
    public void Insert(int key)
    {
        Root = InsertRec(Root, key);
    }

    private AVLNode InsertRec(AVLNode node, int key)
    {
        // 1. Perform standard BST insertion
        if (node == null) return new AVLNode(key);

        if (key < node.Key)
            node.Left = InsertRec(node.Left, key);
        else if (key > node.Key)
            node.Right = InsertRec(node.Right, key);
        else 
            return node; // Duplicate keys are not allowed in basic AVL

        // 2. Update height of this ancestor node
        node.Height = 1 + Math.Max(GetHeight(node.Left), GetHeight(node.Right));

        // 3. Check Balance Factor to see if it became unbalanced
        int balance = GetBalance(node);

        // Case 1: Left-Left Heavy (Requires a single Right Rotation)
        if (balance > 1 && key < node.Left.Key)
            return RightRotate(node);

        // Case 2: Right-Right Heavy (Requires a single Left Rotation)
        if (balance < -1 && key > node.Right.Key)
            return LeftRotate(node);

        // Case 3: Left-Right Heavy (Requires Left Rotate on child, then Right Rotate on parent)
        if (balance > 1 && key > node.Left.Key)
            {
                node.Left = LeftRotate(node.Left);
                return RightRotate(node);
            }

        // Case 4: Right-Left Heavy (Requires Right Rotate on child, then Left Rotate on parent)
        if (balance < -1 && key < node.Right.Key)
            {
                node.Right = RightRotate(node.Right);
                return LeftRotate(node);
            }

        return node; // Return the unchanged/balanced node pointer
    }
}
```

#### Visual Structural Drawing (Single Right Rotation)

Below maps how an unbalanced tree (Left-Left Heavy) shifts its center node to rebalance itself via a Right Rotation when the value `10` is added to `30 -> 20`.

Plaintext

```
      Unbalanced (Before Rotation)                Balanced (After Rotation)
             ┌───[30] (Height: 3)                       ┌───[20] (Height: 2)
             │   Balance: +2                            │    Balance: 0
             ▼                                          ▼          ▼
          ┌─[20] (Height: 2)                         ─[10]       ─[30]
          │   Balance: +1
          ▼
        [10] (Height: 1)
```

---

### B. Metaphor

> [!info] **The Precise Architectural Scale** ⚖️
> 
> Imagine a balance scale where you are stacking fragile blocks. In a standard BST, if you keep adding blocks exclusively to the left side, the scale tips completely over, crashes into the ground, and forms a straight line on the floor.
> 
> An **AVL Tree** acts like a smart, automated mechanical scale. The moment the scale tilts even a fraction too far to one side (Balance Factor out of $[-1, 1]$ bounds), a built-in internal gear immediately clicks. It mechanically shifts the center pivot point over to the heavier side, pulling a lower block up to become the new center hanger. The balance is dynamically, instantly restored, keeping the structure short, round, and perfectly upright at all times.

---

### C. Pseudo Code Operations

#### 1. Search / Lookup

Because an AVL tree is structurally a binary search tree, the search lookup process is completely identical to a standard BST.

Plaintext

```c
procedure search(root : AVLNode, target : int)
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

#### 2. Insert (With Rotation Balancing)

Plaintext

```c
procedure insert(node : AVLNode, key : int)
    if node == null then return create AVLNode(key) end if
    
    if key < node.Key then
        node.Left := insert(node.Left, key)
    else if key > node.Key then
        node.Right := insert(node.Right, key)
    else
        return node // Duplicate ignored
    end if
    
    node.Height := 1 + max(getHeight(node.Left), getHeight(node.Right))
    balance := getBalance(node)
    
    // Balance restorations (Left-Left, Right-Right, Left-Right, Right-Left)
    if balance > 1 and key < node.Left.Key then return rightRotate(node) end if
    if balance < -1 and key > node.Right.Key then return leftRotate(node) end if
    if balance > 1 and key > node.Left.Key then
        node.Left := leftRotate(node.Left)
        return rightRotate(node)
    end if
    if balance < -1 and key < node.Right.Key then
        node.Right := rightRotate(node.Right)
        return leftRotate(node)
    end if
    
    return node
end procedure
```

#### 3. Delete

Deletion mirrors BST logic but requires unwinding back up the call stack to check heights and trigger rotations all the way up to the root.

Plaintext

```c
procedure deleteNode(root : AVLNode, key : int)
    // Step 1: Standard BST Node Unlinking
    if root == null then return root end if
    if key < root.Key then
        root.Left := deleteNode(root.Left, key)
    else if key > root.Key then
        root.Right := deleteNode(root.Right, key)
    else
        if root.Left == null then return root.Right end if
        if root.Right == null then return root.Left end if
        
        successor := minValueNode(root.Right)
        root.Key := successor.Key
        root.Right := deleteNode(root.Right, successor.Key)
    end if
    
    if root == null then return root end if
    
    // Step 2: Update Height and Balance precisely like insertion
    root.Height := 1 + max(getHeight(root.Left), getHeight(root.Right))
    balance := getBalance(root)
    
    // Step 3: Handle structural rotations using child node balances
    if balance > 1 and getBalance(root.Left) >= 0 then return rightRotate(root) end if
    if balance > 1 and getBalance(root.Left) < 0 then
        root.Left := leftRotate(root.Left)
        return rightRotate(root)
    end if
    if balance < -1 and getBalance(root.Right) <= 0 then return leftRotate(root) end if
    if balance < -1 and getBalance(root.Right) > 0 then
        root.Right := rightRotate(root.Right)
        return leftRotate(root)
    end if
    
    return root
end procedure
```

---

## D. Differences From Similar Data Structures

#### 1. AVL Tree vs. Standard Binary Search Tree (BST)

- **Worst-Case Guarantee:** A standard BST can degrade into an $O(n)$ linear chain if sorted data is sequentially input. An AVL tree aggressively forces balance checks at every modification step, ensuring the tree height always remains bounded tightly to $O(\log n)$ constraints.
    
- **Overhead Cost:** AVL nodes carry an explicit memory overhead allocation to track their local node integer heights or balance factors.
    

#### 2. AVL Tree vs. Red-Black Tree (RBT)

- **Strictness of Balance:** AVL trees enforce a stricter balancing threshold (height difference $\le 1$) than Red-Black trees. As a result, AVL trees are more tightly compressed and offer **faster lookups/searches** because path lengths are minimized.
    
- **Insertion/Deletion Performance:** Because an AVL tree is more rigidly balanced, it requires more frequent complex structural rotations during writes. A Red-Black tree has looser balance thresholds, meaning it undergoes fewer rebalancing rotations and generally yields **faster insertion and deletion** modifications.
    

---

## E. Time Complexity

Because an AVL tree guarantees it will never lose structural balance symmetry, its worst-case scenario scales identically to its average-case limits.

|**Operation**|**Average Case**|**Worst Case**|**Description**|
|---|---|---|---|
|**Search**|$O(\log n)$|$O(\log n)$|Guaranteed narrow tree depth execution path profiles.|
|**Insert**|$O(\log n)$|$O(\log n)$|Traverses down branches and executes up to two structural rotations.|
|**Delete**|$O(\log n)$|$O(\log n)$|Traverses down branches and can execute up to $O(\log n)$ rotations back to root.|
|**Space Complexity**|$O(n)$|$O(n)$|Requires storage for node instances plus height structural state metrics.|

## Balancing:

In an AVL tree, unbalance occurs when the **Balance Factor** (Height of Left Subtree − Height of Right Subtree) of a node becomes **+2** (left-heavy) or **-2** (right-heavy).

The path from the unbalanced node to the newly inserted grandchild node determines which of the four cases you are dealing with. Each case requires a specific rotational remedy to restore the tree's height balance.

---

## 1. Left-Left (LL) Case

- **The Problem:** A node becomes unbalanced (+2) because a child was inserted into its **left** child's **left** subtree. The tree leans heavily in a straight line to the left.
    
- **The Remedy:** A single **Right Rotation** around the unbalanced node.
    

Plaintext

```c
       Unbalanced (LL)                       Balanced
         ┌───[30] (+2)                       ┌───[20] (0)
         │                                   │    │
         ▼                                   ▼    ▼
      ┌─[20] (+1)                          [10]  [30]
      │
      ▼
    [10] (0)
```

**Rotation Mechanics:**

The middle node `[20]` is pulled up to become the new root of this subtree. The old root `[30]` kicks down to become its right child.

---

## 2. Right-Right (RR) Case

- **The Problem:** A node becomes unbalanced (-2) because a child was inserted into its **right** child's **right** subtree. The tree leans heavily in a straight line to the right.
    
- **The Remedy:** A single **Left Rotation** around the unbalanced node.
    

Plaintext

```c
       Unbalanced (RR)                       Balanced
         [10]───┐ (-2)                       ┌───[20] (0)
                │                            │    │
                ▼                            ▼    ▼
             [20]─┐ (-1)                   [10]  [30]
                  │
                  ▼
                [30] (0)
```

**Rotation Mechanics:**

The middle node `[20]` is pulled up to become the new root. The old root `[10]` drops down to become its left child.

---

## 3. Left-Right (LR) Case

- **The Problem:** A node becomes unbalanced (+2) because a child was inserted into its **left** child's **right** subtree. This creates a "zigzag" shape pointing left then right.
    
- **The Remedy:** A **Double Rotation**: First a **Left Rotation** on the left child, transforming it into a straightforward LL case, followed by a **Right Rotation** on the unbalanced parent.
    

Plaintext

```c
       Unbalanced (LR)                Step 1: Left Rotate Child              Step 2: Right Rotate Parent
         ┌───[30] (+2)                      ┌───[30] (+2)                         ┌───[25] (0)
         │                                  │                                     │    │
         ▼                                  ▼                                     ▼    ▼
      ┌─[20] (-1)                        ┌─[25] (+1)                            [20]  [30]
      │                                  │
      ▼                                  ▼
    [25]─┐                             [20]
         │
        null
```

**Rotation Mechanics:**

1. We target the child `[20]` and perform a left rotation. This straightens the zigzag out, dropping `[20]` lower down and making `[25]` the new left child.
    
2. Now that it is a standard Left-Left tree layout, we perform a single right rotation around `[30]` to achieve perfect symmetry.
    

---

## 4. Right-Left (RL) Case

- **The Problem:** A node becomes unbalanced (-2) because a child was inserted into its **right** child's **left** subtree. This creates a "zigzag" shape pointing right then left.
    
- **The Remedy:** A **Double Rotation**: First a **Right Rotation** on the right child, transforming it into a straightforward RR case, followed by a **Left Rotation** on the unbalanced parent.
    

Plaintext

```c
       Unbalanced (RL)                Step 1: Right Rotate Child             Step 2: Left Rotate Parent
         [10]───┐ (-2)                      [10]───┐ (-2)                         ┌───[15] (0)
                │                                  │                              │    │
                ▼                                  ▼                              ▼    ▼
             [20] (+1)                          [15]───┐ (-1)                   [10]  [20]
             │                                         │
             ▼                                         ▼
           [15]                                      [20]
```

**Rotation Mechanics:**

1. We target the right child `[20]` and perform a right rotation. This straightens out the rightward zigzag, pushing `[20]` lower and raising `[15]`.
    
2. With the tree now matching a textbook Right-Right template, we execute a final single left rotation around `[10]` to restore full balance.
    

---

## Summary Matrix

| **Case Type**        | **Geometry**                      | **Primary Node Balance** | **Child Node Balance** | **Rotational Cure**                                 |
| -------------------- | --------------------------------- | ------------------------ | ---------------------- | --------------------------------------------------- |
| **LL (Left-Left)**   | Straight line left                | $+2$                     | $+1$ or $0$            | **Single Right Rotate** on parent                   |
| **RR (Right-Right)** | Straight line right               | $-2$                     | $-1$ or $0$            | **Single Left Rotate** on parent                    |
| **LR (Left-Right)**  | Zigzag (Left $\rightarrow$ Right) | $+2$                     | $-1$                   | **Left Rotate child**, then **Right Rotate parent** |
| **RL (Right-Left)**  | Zigzag (Right $\rightarrow$ Left) | $-2$                     | $+1$                   | **Right Rotate child**, then **Left Rotate parent** |

