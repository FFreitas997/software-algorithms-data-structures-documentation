# Algorithms and Data Structures Documentation

Algorithms and data structures are fundamental to computer science and programming. They are the building blocks of software development and are essential to creating efficient and scalable programs. Algorithms are step-by-step procedures for solving problems, while data structures are ways of organizing and storing data in a computer so that it can be accessed and modified efficiently.

---

## Table of Contents

1. [Introduction](#introduction)
2. [Big O Notation](#big-o-notation)
3. [Time Complexity](#time-complexity)
4. [Space Complexity](#space-complexity)
5. [Search Algorithms](#search-algorithms)
   - [Linear (Sequential) Search](#linear-sequential-search)
   - [Binary Search](#binary-search)
6. [Sorting Algorithms](#sorting-algorithms)
   - [Bubble Sort](#bubble-sort)
   - [Selection Sort](#selection-sort)
   - [Insertion Sort](#insertion-sort)
   - [Merge Sort](#merge-sort)
   - [Quick Sort](#quick-sort)
7. [Recursion](#recursion)
8. [Dynamic Programming](#dynamic-programming)
9. [Common Data Structures](#common-data-structures)
   - [Array](#array)
   - [Linked List](#linked-list)
   - [Stack](#stack)
   - [Queue](#queue)
   - [Hash Table](#hash-table)
   - [Tree](#tree)
   - [Graph](#graph)
10. [Algorithm Design Paradigms](#algorithm-design-paradigms)

---

## Introduction

An **algorithm** is a finite sequence of well-defined instructions used to solve a problem or perform a computation. A **data structure** is a specific way of organizing, managing, and storing data to enable efficient access and modification.

Choosing the right algorithm and data structure can mean the difference between a program that finishes in milliseconds and one that takes hours. Understanding their performance characteristics allows engineers to make informed design decisions.

---

## Big O Notation

**Big O Notation** is a mathematical notation used to describe the **upper bound** of an algorithm's running time or space requirements as the input size grows. It expresses the worst-case scenario and helps us compare algorithms independent of hardware.

### Common Big O Complexities (from fastest to slowest)

| Notation      | Name          | Example                                |
|---------------|---------------|----------------------------------------|
| O(1)          | Constant      | Array index access                     |
| O(log n)      | Logarithmic   | Binary search                          |
| O(n)          | Linear        | Linear search                          |
| O(n log n)    | Linearithmic  | Merge sort, Quick sort (average)       |
| O(n²)         | Quadratic     | Bubble sort, Selection sort            |
| O(2ⁿ)         | Exponential   | Recursive Fibonacci (naïve)            |
| O(n!)         | Factorial     | Permutation generation                 |

### Big O Rules

1. **Drop constants**: O(2n) → O(n)
2. **Drop non-dominant terms**: O(n² + n) → O(n²)
3. **Different inputs, different variables**: An algorithm over two separate arrays of sizes `a` and `b` is O(a + b), not O(n²)

### Java Example — Demonstrating O(1), O(n), O(n²)

```java
public class BigOExamples {

    // O(1) — constant time: always one operation regardless of input size
    public static int getFirst(int[] arr) {
        return arr[0];
    }

    // O(n) — linear time: one operation per element
    public static void printAll(int[] arr) {
        for (int value : arr) {
            System.out.println(value);
        }
    }

    // O(n²) — quadratic time: nested loops over same input
    public static void printAllPairs(int[] arr) {
        for (int i = 0; i < arr.length; i++) {
            for (int j = 0; j < arr.length; j++) {
                System.out.println(arr[i] + ", " + arr[j]);
            }
        }
    }
}
```

---

## Time Complexity

**Time complexity** describes how the runtime of an algorithm scales with the size of its input `n`. It is typically expressed using Big O Notation.

### Cases to Consider

| Case         | Description                                                     |
|--------------|-----------------------------------------------------------------|
| Best case    | The minimum time required (e.g., target is the first element)   |
| Average case | The expected time over all possible inputs                      |
| Worst case   | The maximum time required (e.g., target is not in the list)     |

### Common Time Complexities

- **O(1)** — Accessing an element in an array by index.
- **O(log n)** — Halving the search space each step (e.g., Binary Search).
- **O(n)** — Visiting every element once (e.g., Linear Search).
- **O(n log n)** — Efficient sorting algorithms (e.g., Merge Sort).
- **O(n²)** — Comparing every element with every other element (e.g., Bubble Sort).
- **O(2ⁿ)** — Solving every sub-problem twice (e.g., naïve recursive Fibonacci).

### Java Example — Fibonacci: O(2ⁿ) vs O(n)

```java
public class Fibonacci {

    // O(2^n) — exponential: recalculates sub-problems repeatedly
    public static long fibRecursive(int n) {
        if (n <= 1) return n;
        return fibRecursive(n - 1) + fibRecursive(n - 2);
    }

    // O(n) — linear: calculates each value only once
    public static long fibIterative(int n) {
        if (n <= 1) return n;
        long prev = 0, curr = 1;
        for (int i = 2; i <= n; i++) {
            long next = prev + curr;
            prev = curr;
            curr = next;
        }
        return curr;
    }
}
```

---

## Space Complexity

**Space complexity** describes how much memory an algorithm uses relative to its input size `n`. Like time complexity, it is expressed with Big O Notation and includes both:

- **Auxiliary space**: Extra memory used by the algorithm (e.g., variables, call stack, temporary arrays).
- **Input space**: Memory taken by the input itself (sometimes excluded when reporting space complexity).

### Common Space Complexities

| Notation   | Example                                               |
|------------|-------------------------------------------------------|
| O(1)       | In-place sorting (e.g., Bubble Sort, Insertion Sort)  |
| O(log n)   | Recursive Binary Search (call stack depth)            |
| O(n)       | Storing a copy of the input array                     |
| O(n log n) | Some parallel merge sort implementations              |
| O(n²)      | Storing a 2D matrix of size n × n                    |

### Trade-off: Time vs Space

Algorithms often trade time for space or vice versa. For example:
- **Memoization** in dynamic programming uses extra space (O(n)) to avoid redundant computation and reduce time from O(2ⁿ) to O(n).
- **In-place sorting** saves memory but may involve more element swaps.

### Java Example — Space Complexity

```java
public class SpaceComplexityExample {

    // O(1) space — only a constant number of extra variables
    public static int sum(int[] arr) {
        int total = 0;                      // one extra variable
        for (int value : arr) total += value;
        return total;
    }

    // O(n) space — creates a new array proportional to input size
    public static int[] doubled(int[] arr) {
        int[] result = new int[arr.length]; // extra array of size n
        for (int i = 0; i < arr.length; i++) {
            result[i] = arr[i] * 2;
        }
        return result;
    }
}
```

---

## Search Algorithms

Search algorithms are designed to retrieve information stored within data structures. The two most fundamental are Linear Search and Binary Search.

---

### Linear (Sequential) Search

**Linear Search** examines each element of the list one by one until the target is found or the list is exhausted.

| Property        | Value        |
|-----------------|--------------|
| Best case       | O(1)         |
| Worst case      | O(n)         |
| Average case    | O(n)         |
| Space           | O(1)         |
| Requires sorted | No           |

**When to use**: Small or unsorted datasets where simplicity matters more than performance.

#### Java Example

```java
public class LinearSearch {

    /**
     * Searches for a target value in an array sequentially.
     *
     * @param arr    the array to search
     * @param target the value to find
     * @return the index of target if found, otherwise -1
     */
    public static int linearSearch(int[] arr, int target) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == target) {
                return i;
            }
        }
        return -1;
    }

    public static void main(String[] args) {
        int[] data = {5, 3, 8, 1, 9, 2};
        int index = linearSearch(data, 9);
        System.out.println("Found at index: " + index); // Found at index: 4
    }
}
```

---

### Binary Search

**Binary Search** works on **sorted** arrays. It repeatedly divides the search interval in half. If the middle element matches the target, the search ends; otherwise the search continues in the left or right half.

| Property        | Value        |
|-----------------|--------------|
| Best case       | O(1)         |
| Worst case      | O(log n)     |
| Average case    | O(log n)     |
| Space (iter.)   | O(1)         |
| Space (recur.)  | O(log n)     |
| Requires sorted | **Yes**      |

**When to use**: Large, sorted datasets where fast lookup is critical (e.g., dictionary lookup, database indexes).

#### Java Example — Iterative

```java
public class BinarySearch {

    /**
     * Iterative binary search on a sorted array.
     *
     * @param arr    sorted array to search
     * @param target the value to find
     * @return the index of target if found, otherwise -1
     */
    public static int binarySearch(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2; // avoids integer overflow

            if (arr[mid] == target) {
                return mid;
            } else if (arr[mid] < target) {
                left = mid + 1;  // search right half
            } else {
                right = mid - 1; // search left half
            }
        }
        return -1; // not found
    }

    public static void main(String[] args) {
        int[] sorted = {1, 3, 5, 7, 9, 11, 15};
        System.out.println(binarySearch(sorted, 7));  // 3
        System.out.println(binarySearch(sorted, 6));  // -1
    }
}
```

#### Java Example — Recursive

```java
public class BinarySearchRecursive {

    public static int binarySearch(int[] arr, int target, int left, int right) {
        if (left > right) return -1;

        int mid = left + (right - left) / 2;

        if (arr[mid] == target) return mid;
        if (arr[mid] < target)  return binarySearch(arr, target, mid + 1, right);
        return binarySearch(arr, target, left, mid - 1);
    }

    public static void main(String[] args) {
        int[] sorted = {1, 3, 5, 7, 9, 11, 15};
        System.out.println(binarySearch(sorted, 11, 0, sorted.length - 1)); // 5
    }
}
```

---

## Sorting Algorithms

Sorting arranges elements of a list in a specific order (ascending or descending). Sorting is a prerequisite for many other algorithms (e.g., Binary Search) and is used extensively in databases, file systems, and search engines.

---

### Bubble Sort

**Bubble Sort** repeatedly steps through the list, compares adjacent elements and swaps them if they are in the wrong order. The largest unsorted element "bubbles" to its correct position with each pass.

| Property     | Value    |
|--------------|----------|
| Best case    | O(n)     |
| Worst case   | O(n²)    |
| Average case | O(n²)    |
| Space        | O(1)     |
| Stable       | Yes      |
| In-place     | Yes      |

**When to use**: Only for educational purposes or very small datasets. It is generally the slowest practical sorting algorithm.

#### Java Example

```java
public class BubbleSort {

    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n - 1; i++) {
            boolean swapped = false;
            for (int j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    // swap
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                }
            }
            // if no swap occurred, array is already sorted
            if (!swapped) break;
        }
    }

    public static void main(String[] args) {
        int[] arr = {64, 34, 25, 12, 22, 11, 90};
        bubbleSort(arr);
        System.out.println(java.util.Arrays.toString(arr));
        // [11, 12, 22, 25, 34, 64, 90]
    }
}
```

---

### Selection Sort

**Selection Sort** divides the array into a sorted and an unsorted region. On each pass, it finds the minimum (or maximum) element in the unsorted region and moves it to the end of the sorted region.

| Property     | Value    |
|--------------|----------|
| Best case    | O(n²)    |
| Worst case   | O(n²)    |
| Average case | O(n²)    |
| Space        | O(1)     |
| Stable       | No       |
| In-place     | Yes      |

**When to use**: When memory writes are costly (it performs at most O(n) swaps), on small datasets.

#### Java Example

```java
public class SelectionSort {

    public static void selectionSort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n - 1; i++) {
            int minIndex = i;
            for (int j = i + 1; j < n; j++) {
                if (arr[j] < arr[minIndex]) {
                    minIndex = j;
                }
            }
            // swap the found minimum with arr[i]
            int temp = arr[minIndex];
            arr[minIndex] = arr[i];
            arr[i] = temp;
        }
    }

    public static void main(String[] args) {
        int[] arr = {64, 25, 12, 22, 11};
        selectionSort(arr);
        System.out.println(java.util.Arrays.toString(arr));
        // [11, 12, 22, 25, 64]
    }
}
```

---

### Insertion Sort

**Insertion Sort** builds the sorted array one element at a time. It picks the next element and inserts it into its correct position within the already-sorted portion.

| Property     | Value    |
|--------------|----------|
| Best case    | O(n)     |
| Worst case   | O(n²)    |
| Average case | O(n²)    |
| Space        | O(1)     |
| Stable       | Yes      |
| In-place     | Yes      |

**When to use**: Efficient for small datasets or nearly-sorted arrays. Often used as the base case in hybrid sorting algorithms like Tim Sort.

#### Java Example

```java
public class InsertionSort {

    public static void insertionSort(int[] arr) {
        int n = arr.length;
        for (int i = 1; i < n; i++) {
            int key = arr[i];
            int j = i - 1;
            // shift elements of arr[0..i-1] that are greater than key
            while (j >= 0 && arr[j] > key) {
                arr[j + 1] = arr[j];
                j--;
            }
            arr[j + 1] = key;
        }
    }

    public static void main(String[] args) {
        int[] arr = {12, 11, 13, 5, 6};
        insertionSort(arr);
        System.out.println(java.util.Arrays.toString(arr));
        // [5, 6, 11, 12, 13]
    }
}
```

---

### Merge Sort

**Merge Sort** is a divide-and-conquer algorithm. It recursively divides the array into two halves, sorts each half, and then merges the two sorted halves back together.

| Property     | Value      |
|--------------|------------|
| Best case    | O(n log n) |
| Worst case   | O(n log n) |
| Average case | O(n log n) |
| Space        | O(n)       |
| Stable       | Yes        |
| In-place     | No         |

**When to use**: When stable sorting with guaranteed O(n log n) performance is needed. Preferred for sorting linked lists and external sorting (data too large to fit in memory).

#### Java Example

```java
public class MergeSort {

    public static void mergeSort(int[] arr, int left, int right) {
        if (left < right) {
            int mid = left + (right - left) / 2;
            mergeSort(arr, left, mid);
            mergeSort(arr, mid + 1, right);
            merge(arr, left, mid, right);
        }
    }

    private static void merge(int[] arr, int left, int mid, int right) {
        int n1 = mid - left + 1;
        int n2 = right - mid;

        int[] leftArr  = new int[n1];
        int[] rightArr = new int[n2];

        System.arraycopy(arr, left,      leftArr,  0, n1);
        System.arraycopy(arr, mid + 1,   rightArr, 0, n2);

        int i = 0, j = 0, k = left;
        while (i < n1 && j < n2) {
            if (leftArr[i] <= rightArr[j]) {
                arr[k++] = leftArr[i++];
            } else {
                arr[k++] = rightArr[j++];
            }
        }
        while (i < n1) arr[k++] = leftArr[i++];
        while (j < n2) arr[k++] = rightArr[j++];
    }

    public static void main(String[] args) {
        int[] arr = {38, 27, 43, 3, 9, 82, 10};
        mergeSort(arr, 0, arr.length - 1);
        System.out.println(java.util.Arrays.toString(arr));
        // [3, 9, 10, 27, 38, 43, 82]
    }
}
```

---

### Quick Sort

**Quick Sort** is a divide-and-conquer algorithm. It picks a **pivot** element and partitions the array into two sub-arrays: elements less than the pivot and elements greater than the pivot. It then recursively sorts each sub-array.

| Property        | Value      |
|-----------------|------------|
| Best case       | O(n log n) |
| Worst case      | O(n²)      |
| Average case    | O(n log n) |
| Space (avg)     | O(log n)   |
| Space (worst)   | O(n)       |
| Stable          | No         |
| In-place        | Yes        |

**When to use**: Generally the fastest in practice for in-memory sorting. Preferred when average-case performance matters and stability is not required. The worst case O(n²) occurs on already-sorted arrays with a poor pivot choice, which can be mitigated with randomized pivot selection.

#### Java Example

```java
public class QuickSort {

    public static void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            int pivotIndex = partition(arr, low, high);
            quickSort(arr, low, pivotIndex - 1);
            quickSort(arr, pivotIndex + 1, high);
        }
    }

    private static int partition(int[] arr, int low, int high) {
        int pivot = arr[high]; // last element as pivot
        int i = low - 1;       // index of smaller element

        for (int j = low; j < high; j++) {
            if (arr[j] <= pivot) {
                i++;
                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
        // place pivot in correct position
        int temp = arr[i + 1];
        arr[i + 1] = arr[high];
        arr[high] = temp;
        return i + 1;
    }

    public static void main(String[] args) {
        int[] arr = {10, 7, 8, 9, 1, 5};
        quickSort(arr, 0, arr.length - 1);
        System.out.println(java.util.Arrays.toString(arr));
        // [1, 5, 7, 8, 9, 10]
    }
}
```

---

### Sorting Algorithms Comparison

| Algorithm      | Best       | Average    | Worst      | Space   | Stable |
|----------------|------------|------------|------------|---------|--------|
| Bubble Sort    | O(n)       | O(n²)      | O(n²)      | O(1)    | Yes    |
| Selection Sort | O(n²)      | O(n²)      | O(n²)      | O(1)    | No     |
| Insertion Sort | O(n)       | O(n²)      | O(n²)      | O(1)    | Yes    |
| Merge Sort     | O(n log n) | O(n log n) | O(n log n) | O(n)    | Yes    |
| Quick Sort     | O(n log n) | O(n log n) | O(n²)      | O(log n)| No     |

---

## Recursion

**Recursion** is a technique where a function calls itself to solve smaller instances of the same problem. Every recursive function needs:

1. **Base case**: The condition that stops the recursion.
2. **Recursive case**: The part that calls the function with a smaller/simpler input.

Recursion uses the **call stack** to keep track of each function invocation, contributing O(n) or O(log n) space depending on the depth.

### Java Example — Factorial

```java
public class Recursion {

    // O(n) time, O(n) space (call stack)
    public static long factorial(int n) {
        if (n <= 1) return 1;           // base case
        return n * factorial(n - 1);    // recursive case
    }

    public static void main(String[] args) {
        System.out.println(factorial(5)); // 120
        System.out.println(factorial(10)); // 3628800
    }
}
```

---

## Dynamic Programming

**Dynamic Programming (DP)** is an optimization technique that solves complex problems by breaking them down into overlapping sub-problems and storing their results to avoid redundant computation. It is applicable when a problem exhibits:

1. **Overlapping sub-problems**: The same sub-problems are solved multiple times.
2. **Optimal substructure**: The optimal solution of the problem is built from the optimal solutions of its sub-problems.

### Approaches

| Approach        | Description                                                      |
|-----------------|------------------------------------------------------------------|
| **Top-down**    | Recursive + memoization (cache results as you recurse)           |
| **Bottom-up**   | Iterative + tabulation (build the table from smallest sub-problem)|

### Java Example — Fibonacci with Memoization (Top-down DP)

```java
import java.util.HashMap;
import java.util.Map;

public class DynamicProgramming {

    private static final Map<Integer, Long> memo = new HashMap<>();

    // O(n) time, O(n) space
    public static long fib(int n) {
        if (n <= 1) return n;
        if (memo.containsKey(n)) return memo.get(n);
        long result = fib(n - 1) + fib(n - 2);
        memo.put(n, result);
        return result;
    }

    // Bottom-up tabulation, O(n) time, O(n) space
    public static long fibTable(int n) {
        if (n <= 1) return n;
        long[] dp = new long[n + 1];
        dp[0] = 0;
        dp[1] = 1;
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }

    public static void main(String[] args) {
        System.out.println(fib(40));      // 102334155
        System.out.println(fibTable(40)); // 102334155
    }
}
```

---

## Common Data Structures

Data structures determine how data is stored and accessed. Choosing the right data structure significantly impacts algorithm performance.

---

### Array

An **Array** is a contiguous block of memory storing elements of the same type. Elements are accessed via an integer index.

| Operation   | Time Complexity |
|-------------|-----------------|
| Access      | O(1)            |
| Search      | O(n)            |
| Insert (end)| O(1) amortized  |
| Insert (mid)| O(n)            |
| Delete      | O(n)            |

```java
int[] arr = new int[5];
arr[0] = 10;
arr[1] = 20;
System.out.println(arr[0]); // 10
```

---

### Linked List

A **Linked List** is a collection of nodes where each node holds a value and a reference to the next node. Insertions and deletions are efficient at any known position but random access requires traversal.

| Operation     | Time Complexity |
|---------------|-----------------|
| Access        | O(n)            |
| Search        | O(n)            |
| Insert (head) | O(1)            |
| Insert (tail) | O(n) or O(1)*   |
| Delete        | O(n)            |

*O(1) when a tail reference is maintained.

```java
import java.util.LinkedList;

LinkedList<Integer> list = new LinkedList<>();
list.add(1);
list.add(2);
list.addFirst(0);
System.out.println(list); // [0, 1, 2]
```

---

### Stack

A **Stack** is a Last-In-First-Out (LIFO) data structure. Elements are pushed onto and popped from the top.

| Operation | Time Complexity |
|-----------|-----------------|
| Push      | O(1)            |
| Pop       | O(1)            |
| Peek      | O(1)            |
| Search    | O(n)            |

**Use cases**: Function call stack, undo/redo, expression evaluation, DFS traversal.

```java
import java.util.Deque;
import java.util.ArrayDeque;

Deque<Integer> stack = new ArrayDeque<>();
stack.push(1);
stack.push(2);
stack.push(3);
System.out.println(stack.pop());  // 3 (LIFO)
System.out.println(stack.peek()); // 2
```

---

### Queue

A **Queue** is a First-In-First-Out (FIFO) data structure. Elements are added at the rear and removed from the front.

| Operation | Time Complexity |
|-----------|-----------------|
| Enqueue   | O(1)            |
| Dequeue   | O(1)            |
| Peek      | O(1)            |
| Search    | O(n)            |

**Use cases**: BFS traversal, task scheduling, printer queues.

```java
import java.util.Queue;
import java.util.LinkedList;

Queue<Integer> queue = new LinkedList<>();
queue.offer(1);
queue.offer(2);
queue.offer(3);
System.out.println(queue.poll()); // 1 (FIFO)
System.out.println(queue.peek()); // 2
```

---

### Hash Table

A **Hash Table** (or HashMap) stores key-value pairs and uses a hash function to compute the index of a bucket where the value is stored. Collisions are handled via chaining or open addressing.

| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| Insert    | O(1)         | O(n)       |
| Delete    | O(1)         | O(n)       |
| Search    | O(1)         | O(n)       |

**Use cases**: Caching, frequency counting, lookup tables, sets.

```java
import java.util.HashMap;
import java.util.Map;

Map<String, Integer> wordCount = new HashMap<>();
wordCount.put("apple",  3);
wordCount.put("banana", 5);
wordCount.put("apple",  wordCount.getOrDefault("apple", 0) + 1);

System.out.println(wordCount.get("apple"));  // 4
System.out.println(wordCount.containsKey("banana")); // true
```

---

### Tree

A **Tree** is a hierarchical data structure with a root node and subtrees of children nodes. A **Binary Search Tree (BST)** stores values so that for each node, all values in the left subtree are smaller and all values in the right subtree are larger.

| Operation (BST) | Average Case | Worst Case |
|-----------------|--------------|------------|
| Insert          | O(log n)     | O(n)       |
| Delete          | O(log n)     | O(n)       |
| Search          | O(log n)     | O(n)       |

*Worst case O(n) occurs with a degenerate (unbalanced) tree. Self-balancing trees like AVL and Red-Black Trees guarantee O(log n).*

```java
// Java's TreeMap is backed by a Red-Black Tree
import java.util.TreeMap;
import java.util.Map;

Map<Integer, String> bst = new TreeMap<>();
bst.put(5, "five");
bst.put(3, "three");
bst.put(7, "seven");

// TreeMap iterates in sorted key order
bst.forEach((k, v) -> System.out.println(k + " -> " + v));
// 3 -> three
// 5 -> five
// 7 -> seven
```

---

### Graph

A **Graph** is a collection of **vertices** (nodes) connected by **edges**. Graphs can be directed or undirected, weighted or unweighted. Common representations are:

- **Adjacency Matrix**: O(V²) space, O(1) edge lookup.
- **Adjacency List**: O(V + E) space, O(degree) edge lookup.

**Common graph algorithms**: BFS (shortest path in unweighted graphs), DFS (connectivity, cycle detection), Dijkstra's algorithm (shortest path in weighted graphs).

```java
import java.util.*;

public class Graph {

    private final Map<Integer, List<Integer>> adjacencyList = new HashMap<>();

    public void addEdge(int src, int dest) {
        adjacencyList.computeIfAbsent(src,  k -> new ArrayList<>()).add(dest);
        adjacencyList.computeIfAbsent(dest, k -> new ArrayList<>()).add(src);
    }

    // BFS traversal
    public void bfs(int start) {
        Set<Integer> visited = new HashSet<>();
        Queue<Integer> queue = new LinkedList<>();
        visited.add(start);
        queue.offer(start);

        while (!queue.isEmpty()) {
            int node = queue.poll();
            System.out.print(node + " ");
            for (int neighbor : adjacencyList.getOrDefault(node, Collections.emptyList())) {
                if (!visited.contains(neighbor)) {
                    visited.add(neighbor);
                    queue.offer(neighbor);
                }
            }
        }
    }

    public static void main(String[] args) {
        Graph g = new Graph();
        g.addEdge(0, 1);
        g.addEdge(0, 2);
        g.addEdge(1, 3);
        g.addEdge(2, 4);
        g.bfs(0); // 0 1 2 3 4
    }
}
```

---

## Algorithm Design Paradigms

Several general strategies guide algorithm design:

| Paradigm              | Description                                                                 | Examples                              |
|-----------------------|-----------------------------------------------------------------------------|---------------------------------------|
| **Brute Force**       | Try all possibilities                                                        | Linear search, naïve string matching  |
| **Divide and Conquer**| Split problem into sub-problems, solve independently, combine results        | Merge Sort, Quick Sort, Binary Search |
| **Dynamic Programming**| Store results of overlapping sub-problems to avoid recomputation            | Fibonacci, Knapsack, Longest Common Subsequence |
| **Greedy**            | Make the locally optimal choice at each step                                 | Dijkstra's, Prim's, Huffman encoding  |
| **Backtracking**      | Try options recursively and backtrack when a dead end is reached             | N-Queens, Sudoku solver, Maze solving |

### Java Example — Greedy: Coin Change (minimum coins)

```java
import java.util.Arrays;

public class GreedyCoinChange {

    /**
     * Greedy approach: always pick the largest coin that fits.
     * Note: Only works for canonical coin systems (e.g., US coins).
     * For arbitrary coin systems, use Dynamic Programming instead.
     */
    public static int minCoinsGreedy(int[] coins, int amount) {
        Arrays.sort(coins);
        int count = 0;
        for (int i = coins.length - 1; i >= 0 && amount > 0; i--) {
            count += amount / coins[i];
            amount %= coins[i];
        }
        return amount == 0 ? count : -1;
    }

    public static void main(String[] args) {
        int[] coins = {1, 5, 10, 25};
        System.out.println(minCoinsGreedy(coins, 41)); // 4 (25+10+5+1)
    }
}
```

---

*This documentation covers the foundational concepts of algorithms and data structures. Mastering these topics is essential for writing efficient software, passing technical interviews, and making sound engineering decisions.*
