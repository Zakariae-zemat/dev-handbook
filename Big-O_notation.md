# Time & Space Complexity Analysis (Big-O Notation)

A comprehensive guide to understanding algorithmic complexity with Java examples.

## 📚 Table of Contents
- [What is Big-O Notation?](#what-is-big-o-notation)
- [Common Time Complexities](#common-time-complexities)
- [Space Complexity](#space-complexity)
- [Analysis Examples](#analysis-examples)
- [Quick Reference](#quick-reference)

## What is Big-O Notation?

Big-O notation describes the **worst-case** performance of an algorithm as the input size grows. It helps us compare algorithms and predict their behavior with large datasets.

**Key Points:**
- Focuses on the dominant term (highest order)
- Ignores constants and lower-order terms
- Represents upper bound of growth rate

## Common Time Complexities

### O(1) - Constant Time
Operations that take the same time regardless of input size.

```java
public int getFirstElement(int[] arr) {
    return arr[0]; // Always one operation
}

public void printHello() {
    System.out.println("Hello"); // Constant time
}
```

### O(log n) - Logarithmic Time
Divides the problem in half with each step.

```java
// Binary Search
public int binarySearch(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) return mid;
        if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

### O(n) - Linear Time
Processing each element once.

```java
public int findMax(int[] arr) {
    int max = arr[0];
    for (int i = 1; i < arr.length; i++) { // n iterations
        if (arr[i] > max) max = arr[i];
    }
    return max;
}
```

### O(n log n) - Linearithmic Time
Efficient sorting algorithms.

```java
// Merge Sort
public void mergeSort(int[] arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);      // T(n/2)
        mergeSort(arr, mid + 1, right); // T(n/2)
        merge(arr, left, mid, right);   // O(n)
    }
} // Overall: O(n log n)
```

### O(n²) - Quadratic Time
Nested loops over the same data.

```java
// Bubble Sort
public void bubbleSort(int[] arr) {
    for (int i = 0; i < arr.length - 1; i++) {        // n iterations
        for (int j = 0; j < arr.length - i - 1; j++) { // n iterations
            if (arr[j] > arr[j + 1]) {
                // Swap elements
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}
```

### O(2ⁿ) - Exponential Time
Algorithms that explore all possible combinations.

```java
// Naive Fibonacci (inefficient)
public int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2); // 2^n recursive calls
}
```

## Space Complexity

Space complexity measures memory usage relative to input size.

### O(1) - Constant Space
```java
public int sum(int[] arr) {
    int total = 0; // Only one variable
    for (int num : arr) {
        total += num;
    }
    return total;
} // Space: O(1)
```

### O(n) - Linear Space
```java
public int[] createCopy(int[] arr) {
    int[] copy = new int[arr.length]; // n space
    for (int i = 0; i < arr.length; i++) {
        copy[i] = arr[i];
    }
    return copy;
} // Space: O(n)
```

### Recursive Space
```java
public int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1); // n recursive calls on stack
} // Space: O(n) due to call stack
```

## Analysis Examples

### Example 1: Multiple Operations
```java
public void complexFunction(int[] arr) {
    // O(1)
    int first = arr[0];
    
    // O(n)
    for (int i = 0; i < arr.length; i++) {
        System.out.println(arr[i]);
    }
    
    // O(n²)
    for (int i = 0; i < arr.length; i++) {
        for (int j = 0; j < arr.length; j++) {
            System.out.println(arr[i] + arr[j]);
        }
    }
} // Overall: O(1) + O(n) + O(n²) = O(n²)
```

### Example 2: Best vs Worst Case
```java
// Linear Search
public int linearSearch(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) return i; // Best case: O(1)
    }
    return -1; // Worst case: O(n)
} // Big-O focuses on worst case: O(n)
```

## Quick Reference

| Complexity | Name | Example Operations |
|------------|------|-------------------|
| O(1) | Constant | Array access, HashMap get |
| O(log n) | Logarithmic | Binary search, balanced BST |
| O(n) | Linear | Linear search, array traversal |
| O(n log n) | Linearithmic | Merge sort, heap sort |
| O(n²) | Quadratic | Bubble sort, nested loops |
| O(2ⁿ) | Exponential | Recursive fibonacci, subset generation |

### Performance Comparison (n = 1000)
- O(1): 1 operation
- O(log n): ~10 operations  
- O(n): 1,000 operations
- O(n log n): ~10,000 operations
- O(n²): 1,000,000 operations
- O(2ⁿ): More than atoms in universe!

## 🎯 Tips for Analysis

1. **Identify loops**: Each nested loop typically adds a factor of n
2. **Look for divide-and-conquer**: Often results in O(log n) factor  
3. **Consider recursive calls**: Count the depth and breadth of recursion
4. **Drop constants**: O(2n) = O(n), O(n² + 100n) = O(n²)
5. **Focus on worst case**: Big-O represents the upper bound

---

*Understanding complexity helps you write efficient code and choose the right algorithms for your specific use case.*
