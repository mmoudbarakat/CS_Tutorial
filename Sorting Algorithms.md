
## 🟦 Bubble Sort

### A. C# Example

C#

```csharp
public void BubbleSort(int[] array)
{
    int n = array.Length;
    for (int i = 0; i < n - 1; i++)
    {
        for (int j = 0; j < n - i - 1; j++)
        {
            if (array[j] > array[j + 1])
            {
                // Swap elements
                int temp = array[j];
                array[j] = array[j + 1];
                array[j + 1] = temp;
            }
        }
    }
}
```

### B. Metaphor

> [!info] **The Fizzy Drink** 🫧
> 
> Imagine bubbles rising to the top of a soda glass. The largest bubbles (largest numbers) are the heaviest/most buoyant and quickly float to the very top (the end of the array), while the smaller ones slowly sink to the bottom.

### C. Pseudo Code

Plaintext

```csharp
procedure bubbleSort(A : list of sortable items)
    n := length(A)
    repeat
        swapped := false
        for i := 1 to n-1 inclusive do
            if A[i-1] > A[i] then
                swap(A[i-1], A[i])
                swapped := true
            end if
        end for
        n := n - 1
    until not swapped
end procedure
```

---

## 🟨 Selection Sort

### A. C# Example

C#

```csharp
public void SelectionSort(int[] array)
{
    int n = array.Length;
    for (int i = 0; i < n - 1; i++)
    {
        int minIndex = i;
        for (int j = i + 1; j < n; j++)
        {
            if (array[j] < array[minIndex])
            {
                minIndex = j;
            }
        }
        // Swap the found minimum element with the first element
        int temp = array[minIndex];
        array[minIndex] = array[i];
        array[i] = temp;
    }
}
```

### B. Metaphor

> [!info] **The Scanning Eye** 🕵️
> 
> You are scanning a messy shelf of books from left to right. You look for the absolute shortest book on the entire shelf, pick it up, and swap its position with the very first book. Then you move to the second spot and scan the remaining books for the next shortest one.

### C. Pseudo Code

Plaintext

```csharp
procedure selectionSort(A : list of sortable items)
    n := length(A)
    for i := 0 to n-2 do
        minIndex := i
        for j := i+1 to n-1 do
            if A[j] < A[minIndex] then
                minIndex := j
            end if
        end for
        if minIndex != i then
            swap(A[i], A[minIndex])
        end if
    end for
end procedure
```

---

## 🟩 Insertion Sort

### A. C# Example

C#

```csharp
public void InsertionSort(int[] array)
{
    int n = array.Length;
    for (int i = 1; i < n; ++i)
    {
        int key = array[i];
        int j = i - 1;

        // Move elements of array[0..i-1] that are greater than key
        // to one position ahead of their current position
        while (j >= 0 && array[j] > key)
        {
            array[j + 1] = array[j];
            j = j - 1;
        }
        array[j + 1] = key;
    }
}
```

### B. Metaphor

> [!info] **Sorting a Hand of Cards** 🃏
> 
> Imagine you are dealt a hand of playing cards one by one. You pick up a new card, look at your already sorted hand, and slide/insert it into its exact correct position, shifting the larger cards over to make room.

### C. Pseudo Code

Plaintext

```csharp
procedure insertionSort(A : list of sortable items)
    for i := 1 to length(A) - 1 do
        key := A[i]
        j := i - 1
        while j >= 0 and A[j] > key do
            A[j + 1] := A[j]
            j := j - 1
        end while
        A[j + 1] := key
    end for
end procedure
```

---

## 🟥 Merge Sort

### A. C# Example

C#

```csharp
public void MergeSort(int[] array, int left, int right)
{
    if (left < right)
    {
        int mid = left + (right - left) / 2;

        MergeSort(array, left, mid);
        MergeSort(array, mid + 1, right);

        Merge(array, left, mid, right);
    }
}

private void Merge(int[] array, int left, int mid, int right)
{
    int n1 = mid - left + 1;
    int n2 = right - mid;

    int[] leftArray = new int[n1];
    int[] rightArray = new int[n2];

    Array.Copy(array, left, leftArray, 0, n1);
    Array.Copy(array, mid + 1, rightArray, 0, n2);

    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2)
    {
        if (leftArray[i] <= rightArray[j])
        {
            array[k] = leftArray[i];
            i++;
        }
        else
        {
            array[k] = rightArray[j];
            j++;
        }
        k++;
    }

    while (i < n1)
    {
        array[k] = leftArray[i];
        i++; k++;
    }

    while (j < n2)
    {
        array[k] = rightArray[j];
        j++; k++;
    }
}
```

### B. Metaphor

> [!info] **The Organized Classrooms** 🤝
> 
> A teacher has a massive stack of tests to sort alphabetically. They split the stack in half and hand them to two assistants. Those assistants split them again, down to individual papers. Then, everyone pairs back up, merging their smaller, already-sorted stacks together by comparing just the top cards of each pile.

### C. Pseudo Code

Plaintext

```csharp
procedure mergeSort(A : list, left : int, right : int)
    if left < right then
        mid := left + (right - left) / 2
        mergeSort(A, left, mid)
        mergeSort(A, mid + 1, right)
        merge(A, left, mid, right)
    end if
end procedure
```

---

## 🟪 Quick Sort

### A. C# Example

C#

```csharp
public void QuickSort(int[] array, int low, int high)
{
    if (low < high)
    {
        int pivotIndex = Partition(array, low, high);

        QuickSort(array, low, pivotIndex - 1);
        QuickSort(array, pivotIndex + 1, high);
    }
}

private int Partition(int[] array, int low, int high)
{
    int pivot = array[high];
    int i = (low - 1);

    for (int j = low; j < high; j++)
    {
        if (array[j] < pivot)
        {
            i++;
            int temp = array[i];
            array[i] = array[j];
            array[j] = temp;
        }
    }

    int tempPivot = array[i + 1];
    array[i + 1] = array[high];
    array[high] = tempPivot;

    return i + 1;
}
```

### B. Metaphor

> [!info] **The Line Leader** 🎯
> 
> A gym teacher picks a random student (the pivot). The teacher tells everyone shorter than that student to stand on their left, and everyone taller to stand on their right. Now, that pivot student is perfectly in their final sorted spot. The teacher then repeats this rule for the left group and the right group.

### C. Pseudo Code

Plaintext

```csharp
procedure quickSort(A : list, low : int, high : int)
    if low < high then
        pivotIndex := partition(A, low, high)
        quickSort(A, low, pivotIndex - 1)
        quickSort(A, pivotIndex + 1, high)
    end if
end procedure
```

---

## 🟫 Heap Sort

### A. C# Example

C#

```csharp
public void HeapSort(int[] array)
{
    int n = array.Length;

    for (int i = n / 2 - 1; i >= 0; i--)
        Heapify(array, n, i);

    for (int i = n - 1; i > 0; i--)
    {
        int temp = array[0];
        array[0] = array[i];
        array[i] = temp;

        Heapify(array, i, 0);
    }
}

private void Heapify(int[] array, int n, int i)
{
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;

    if (left < n && array[left] > array[largest])
        largest = left;

    if (right < n && array[right] > array[largest])
        largest = right;

    if (largest != i)
    {
        int swap = array[i];
        array[i] = array[largest];
        array[largest] = swap;

        Heapify(array, n, largest);
    }
}
```

### B. Metaphor

> [!info] **The Corporate Pyramid** 👑
> 
> Think of a tournament bracket or a strict corporate pyramid where the absolute biggest boss always sits at the top (a Max-Heap). You take the big boss out of the building (put them at the end of your sorted list), promote the next most qualified person to the top, fix the corporate ladder, and repeat.

### C. Pseudo Code

Plaintext

```csharp
procedure heapSort(A : list)
    n := length(A)
    for i := (n / 2) - 1 down to 0 do
        heapify(A, n, i)
    end for
    for i := n - 1 down to 1 do
        swap(A[0], A[i])
        heapify(A, i, 0)
    end for
end procedure
```

---

## 📈 Summary Cheatsheet (For your MOC)

| **Algorithm** | **Best Case** | **Average Case** | **Worst Case** | **Space Complexity** | **Stable?** |
| ------------- | ------------- | ---------------- | -------------- | -------------------- | ----------- |
| **Bubble**    | $O(n)$        | $O(n^2)$         | $O(n^2)$       | $O(1)$               | Yes         |
| **Selection** | $O(n^2)$      | $O(n^2)$         | $O(n^2)$       | $O(1)$               | No          |
| **Insertion** | $O(n)$        | $O(n^2)$         | $O(n^2)$       | $O(1)$               | Yes         |
| **Merge**     | $O(n \log n)$ | $O(n \log n)$    | $O(n \log n)$  | $O(n)$               | Yes         |
| **Quick**     | $O(n \log n)$ | $O(n \log n)$    | $O(n^2)$       | $O(\log n)$          | No          |
| **Heap**      | $O(n \log n)$ | $O(n \log n)$    | $O(n \log n)$  | $O(1)$               | No          |


![[15 Sorting Algorithms in 6 Minutes.mp4]]

