# Big-O Complexity Analysis - Simple Guide

Learn to analyze algorithm performance with practical Java examples.

## What is Big-O?

Big-O tells us **how slow your code gets** when you give it more data.

- **O(1)** = Always fast
- **O(n)** = Slower with more data  
- **O(n²)** = Much slower with more data

## Common Complexities

### O(1) - Constant
```java
int getFirst(int[] arr) {
    return arr[0]; // Always 1 step
}
```

### O(n) - Linear  
```java
int sum(int[] arr) {
    int total = 0;
    for (int num : arr) { // n steps
        total += num;
    }
    return total;
}
```

### O(n²) - Quadratic
```java
void printPairs(int[] arr) {
    for (int i = 0; i < arr.length; i++) {     // n times
        for (int j = 0; j < arr.length; j++) { // n times each
            System.out.println(arr[i] + ", " + arr[j]);
        }
    } // Total: n × n = n²
}
```

## Analysis Examples

### Example 1: Count Elements
```java
int countElements(int[] arr) {
    return arr.length; // O(1) - just return a property
}
```
**Analysis:** No loops, just one operation → **O(1)**

### Example 2: Find Maximum
```java
int findMax(int[] arr) {
    int max = arr[0];
    for (int i = 1; i < arr.length; i++) { // Loop n times
        if (arr[i] > max) {
            max = arr[i];
        }
    }
    return max;
}
```
**Analysis:** One loop through n elements → **O(n)**

### Example 3: Check for Duplicates
```java
boolean hasDuplicates(int[] arr) {
    for (int i = 0; i < arr.length; i++) {         // n times
        for (int j = i + 1; j < arr.length; j++) { // (n-1) times each
            if (arr[i] == arr[j]) {
                return true;
            }
        }
    }
    return false;
}
```
**Analysis:** Nested loops → **O(n²)**

### Example 4: Binary Search
```java
int binarySearch(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    
    while (left <= right) {           // Cut in half each time
        int mid = (left + right) / 2;
        if (arr[mid] == target) return mid;
        if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```
**Analysis:** Divide by 2 each step → **O(log n)**

### Example 5: Print Triangle
```java
void printTriangle(int n) {
    for (int i = 1; i <= n; i++) {        // n times
        for (int j = 1; j <= i; j++) {    // 1,2,3...n times
            System.out.print("* ");
        }
        System.out.println();
    }
}
```
**Analysis:** Total prints = 1+2+3+...+n = n(n+1)/2 → **O(n²)**

### Example 6: Multiple Loops (Same Level)
```java
void process(int[] arr) {
    // First loop
    for (int i = 0; i < arr.length; i++) {  // O(n)
        System.out.println(arr[i]);
    }
    
    // Second loop  
    for (int i = 0; i < arr.length; i++) {  // O(n)
        System.out.println(arr[i] * 2);
    }
}
```
**Analysis:** O(n) + O(n) = **O(n)** (not O(2n), we drop constants)

### Example 7: Multiple Loops (Nested)
```java
void nestedProcess(int[] arr) {
    for (int i = 0; i < arr.length; i++) {     // n times
        System.out.println("Outer: " + arr[i]);
        
        for (int j = 0; j < arr.length; j++) { // n times each
            System.out.println("Inner: " + arr[j]);
        }
    }
}
```
**Analysis:** Outer loop × Inner loop = n × n → **O(n²)**

### Example 8: Half Loop
```java
void printHalf(int[] arr) {
    for (int i = 0; i < arr.length / 2; i++) { // n/2 times
        System.out.println(arr[i]);
    }
}
```
**Analysis:** n/2 operations, drop constants → **O(n)**

### Example 9: Logarithmic Pattern
```java
void logPattern(int n) {
    for (int i = 1; i < n; i *= 2) { // 1,2,4,8,16... until n
        System.out.println(i);
    }
}
```
**Analysis:** How many times can we multiply by 2 to reach n? log₂(n) times → **O(log n)**

### Example 10: Fibonacci (Exponential)
```java
int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n-1) + fibonacci(n-2); // 2 recursive calls each time
}
```
**Analysis:** Each call makes 2 more calls, n levels deep → **O(2ⁿ)**

### Example 11: Complex Analysis
```java
void complexFunction(int[] arr) {
    // Step 1: O(1)
    int first = arr[0];
    
    // Step 2: O(n)
    for (int i = 0; i < arr.length; i++) {
        System.out.println(arr[i]);
    }
    
    // Step 3: O(n²)
    for (int i = 0; i < arr.length; i++) {
        for (int j = 0; j < arr.length; j++) {
            if (arr[i] > arr[j]) {
                System.out.println("Greater");
            }
        }
    }
    
    // Step 4: O(log n)
    int target = 5;
    binarySearch(arr, target);
}
```
**Analysis:** O(1) + O(n) + O(n²) + O(log n) = **O(n²)** (take the largest)

### Example 12: Space Complexity
```java
// O(1) Space - only using a few variables
int sumArray(int[] arr) {
    int sum = 0; // One variable
    for (int num : arr) {
        sum += num;
    }
    return sum;
}

// O(n) Space - creating new array
int[] doubleArray(int[] arr) {
    int[] doubled = new int[arr.length]; // n space
    for (int i = 0; i < arr.length; i++) {
        doubled[i] = arr[i] * 2;
    }
    return doubled;
}

// O(n) Space - recursive calls use stack
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n-1); // n function calls on stack
}
```

## Quick Rules

1. **No loops** = O(1)
2. **One loop** = O(n)  
3. **Nested loops** = O(n²)
4. **Dividing by 2** = O(log n)
5. **Recursive with 2 calls** = O(2ⁿ)

## Performance Comparison

For 1000 items:
- O(1): 1 operation ⚡
- O(log n): 10 operations ⚡  
- O(n): 1,000 operations ✅
- O(n²): 1,000,000 operations ⚠️
- O(2ⁿ): Way too many! 💥

**Remember:** Big-O helps you choose the right algorithm before your code gets slow!
