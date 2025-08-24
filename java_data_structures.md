# Java Data Structures Mastery Guide for Interview Success

## Table of Contents
1. [Arrays](#arrays)
2. [ArrayList](#arraylist)
3. [Sets](#sets)
4. [Maps](#maps)
5. [Problem-Solving Patterns](#problem-solving-patterns)
6. [Interview Practice Problems](#interview-practice-problems)
7. [Performance Analysis](#performance-analysis)

---

## Arrays

### Core Concepts
Arrays are fixed-size collections of elements of the same type, stored in contiguous memory locations.

```java
// Declaration and initialization
int[] arr1 = new int[5];           // Creates array of size 5, all elements = 0
int[] arr2 = {1, 2, 3, 4, 5};      // Direct initialization
int[] arr3 = new int[]{1, 2, 3, 4, 5}; // Explicit initialization

// 2D Arrays
int[][] matrix = new int[3][4];    // 3x4 matrix
int[][] matrix2 = {{1, 2}, {3, 4}, {5, 6}}; // Direct initialization
```

### Common Array Patterns

#### 1. Two Pointers Technique
```java
public boolean isPalindrome(String s) {
    char[] chars = s.toLowerCase().toCharArray();
    int left = 0, right = chars.length - 1;
    
    while (left < right) {
        if (chars[left] != chars[right]) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```

#### 2. Sliding Window
```java
public int maxSubarraySum(int[] arr, int k) {
    int maxSum = 0;
    int windowSum = 0;
    
    // Calculate sum of first window
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    maxSum = windowSum;
    
    // Slide the window
    for (int i = k; i < arr.length; i++) {
        windowSum += arr[i] - arr[i - k];
        maxSum = Math.max(maxSum, windowSum);
    }
    return maxSum;
}
```

#### 3. Prefix Sum
```java
public class PrefixSum {
    private int[] prefixSum;
    
    public PrefixSum(int[] arr) {
        prefixSum = new int[arr.length + 1];
        for (int i = 0; i < arr.length; i++) {
            prefixSum[i + 1] = prefixSum[i] + arr[i];
        }
    }
    
    public int rangeSum(int left, int right) {
        return prefixSum[right + 1] - prefixSum[left];
    }
}
```

---

## ArrayList

### Core Concepts
ArrayList is a dynamic array that can grow and shrink during runtime.

```java
import java.util.*;

// Declaration and initialization
ArrayList<Integer> list1 = new ArrayList<>();
List<String> list2 = new ArrayList<>(); // Preferred - programming to interface
List<Integer> list3 = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> list4 = List.of(1, 2, 3, 4, 5); // Immutable (Java 9+)
```

### Complete Method Reference

#### Core ArrayList Methods
```java
// CONSTRUCTORS
ArrayList<T> list = new ArrayList<>();              // Empty list
ArrayList<T> list = new ArrayList<>(capacity);      // With initial capacity
ArrayList<T> list = new ArrayList<>(collection);    // From existing collection

// ADDING ELEMENTS
boolean add(E element)                    // Add to end, returns true
void add(int index, E element)           // Add at specific index
boolean addAll(Collection<? extends E> c) // Add all elements from collection
boolean addAll(int index, Collection<? extends E> c) // Add collection at index

// ACCESSING ELEMENTS
E get(int index)                         // Get element at index
int size()                               // Number of elements
boolean isEmpty()                        // Check if empty
boolean contains(Object o)               // Check if contains element
int indexOf(Object o)                    // First occurrence index (-1 if not found)
int lastIndexOf(Object o)               // Last occurrence index (-1 if not found)

// MODIFYING ELEMENTS
E set(int index, E element)             // Replace element at index, returns old element
void clear()                            // Remove all elements
void ensureCapacity(int minCapacity)    // Increase capacity if needed
void trimToSize()                       // Trim capacity to current size

// REMOVING ELEMENTS
E remove(int index)                     // Remove by index, returns removed element
boolean remove(Object o)                // Remove first occurrence of object
boolean removeAll(Collection<?> c)      // Remove all elements in collection
boolean retainAll(Collection<?> c)      // Keep only elements in collection
boolean removeIf(Predicate<? super E> filter) // Remove elements matching predicate

// CONVERSION AND COPYING
Object[] toArray()                      // Convert to Object array
<T> T[] toArray(T[] a)                 // Convert to typed array
ArrayList<E> clone()                    // Shallow copy
List<E> subList(int fromIndex, int toIndex) // View of portion of list

// ITERATION
Iterator<E> iterator()                  // Forward iterator
ListIterator<E> listIterator()         // Bidirectional iterator
ListIterator<E> listIterator(int index) // Iterator starting at index
void forEach(Consumer<? super E> action) // Apply action to each element

// SEARCHING AND SORTING (via Collections class)
Collections.sort(list)                  // Sort in natural order
Collections.sort(list, comparator)     // Sort with custom comparator
Collections.binarySearch(list, key)    // Binary search (list must be sorted)
Collections.reverse(list)               // Reverse the list
Collections.shuffle(list)               // Randomly shuffle
```

### Essential Operations Example
```java
public void arrayListOperations() {
    List<Integer> list = new ArrayList<>();
    
    // Adding elements
    list.add(10);           // [10]
    list.add(0, 5);         // [5, 10]
    list.addAll(Arrays.asList(15, 20)); // [5, 10, 15, 20]
    
    // Accessing elements
    int first = list.get(0);        // 5
    int size = list.size();         // 4
    boolean isEmpty = list.isEmpty(); // false
    
    // Modifying elements
    list.set(0, 7);         // [7, 10, 15, 20]
    
    // Removing elements
    list.remove(0);         // [10, 15, 20] - by index
    list.remove(Integer.valueOf(15)); // [10, 20] - by value
    
    // Searching
    boolean contains = list.contains(10);  // true
    int index = list.indexOf(20);          // 1
    
    // Iteration
    for (int num : list) {
        System.out.println(num);
    }
}
```

### ArrayList Problem-Solving Patterns

#### 1. Dynamic Programming with Lists
```java
public List<Integer> generatePascalRow(int rowIndex) {
    List<Integer> row = new ArrayList<>();
    row.add(1);
    
    for (int i = 1; i <= rowIndex; i++) {
        // Traverse backwards to avoid overwriting values we need
        for (int j = i - 1; j > 0; j--) {
            row.set(j, row.get(j) + row.get(j - 1));
        }
        row.add(1);
    }
    return row;
}
```

#### 2. List Manipulation
```java
public void moveZeroes(List<Integer> nums) {
    int writeIndex = 0;
    
    // Move all non-zero elements to the front
    for (int readIndex = 0; readIndex < nums.size(); readIndex++) {
        if (nums.get(readIndex) != 0) {
            nums.set(writeIndex++, nums.get(readIndex));
        }
    }
    
    // Fill the rest with zeros
    while (writeIndex < nums.size()) {
        nums.set(writeIndex++, 0);
    }
}
```

---

## Sets

### Core Concepts
Sets are collections that contain no duplicate elements.

```java
import java.util.*;

// Different Set implementations
Set<String> hashSet = new HashSet<>();     // O(1) average operations, no order
Set<String> linkedHashSet = new LinkedHashSet<>(); // Maintains insertion order
Set<String> treeSet = new TreeSet<>();     // Sorted order, O(log n) operations
```

### Essential Set Operations
```java
public void setOperations() {
    Set<Integer> set1 = new HashSet<>(Arrays.asList(1, 2, 3, 4));
    Set<Integer> set2 = new HashSet<>(Arrays.asList(3, 4, 5, 6));
    
    // Basic operations
    set1.add(5);
    boolean contains = set1.contains(3);
    set1.remove(1);
    
    // Set operations
    Set<Integer> union = new HashSet<>(set1);
    union.addAll(set2);  // Union: [2, 3, 4, 5, 6]
    
    Set<Integer> intersection = new HashSet<>(set1);
    intersection.retainAll(set2); // Intersection: [3, 4, 5]
    
    Set<Integer> difference = new HashSet<>(set1);
    difference.removeAll(set2); // Difference: [2]
}
```

### Set Problem-Solving Patterns

#### 1. Detecting Duplicates
```java
public boolean containsDuplicate(int[] nums) {
    Set<Integer> seen = new HashSet<>();
    for (int num : nums) {
        if (!seen.add(num)) {  // add() returns false if element already exists
            return true;
        }
    }
    return false;
}
```

#### 2. Finding Unique Elements
```java
public int singleNumber(int[] nums) {
    Set<Integer> set = new HashSet<>();
    for (int num : nums) {
        if (!set.add(num)) {
            set.remove(num);
        }
    }
    return set.iterator().next();
}
```

#### 3. Intersection of Arrays
```java
public int[] intersection(int[] nums1, int[] nums2) {
    Set<Integer> set1 = new HashSet<>();
    Set<Integer> result = new HashSet<>();
    
    for (int num : nums1) {
        set1.add(num);
    }
    
    for (int num : nums2) {
        if (set1.contains(num)) {
            result.add(num);
        }
    }
    
    return result.stream().mapToInt(Integer::intValue).toArray();
}
```

---

## Maps

### Core Concepts
Maps store key-value pairs with unique keys.

```java
import java.util.*;

// Different Map implementations
Map<String, Integer> hashMap = new HashMap<>();        // O(1) average, no order
Map<String, Integer> linkedHashMap = new LinkedHashMap<>(); // Maintains order
Map<String, Integer> treeMap = new TreeMap<>();        // Sorted by keys
```

### Essential Map Operations
```java
public void mapOperations() {
    Map<String, Integer> map = new HashMap<>();
    
    // Adding/Updating
    map.put("apple", 5);
    map.put("banana", 3);
    map.putIfAbsent("cherry", 7);  // Only adds if key doesn't exist
    
    // Accessing
    Integer apples = map.get("apple");              // 5
    Integer oranges = map.getOrDefault("orange", 0); // 0 (default)
    boolean hasApple = map.containsKey("apple");     // true
    boolean hasValue3 = map.containsValue(3);        // true
    
    // Removing
    map.remove("banana");
    
    // Iteration
    for (Map.Entry<String, Integer> entry : map.entrySet()) {
        System.out.println(entry.getKey() + ": " + entry.getValue());
    }
    
    // Java 8+ forEach
    map.forEach((key, value) -> System.out.println(key + ": " + value));
}
```

### Map Problem-Solving Patterns

#### 1. Frequency Counting
```java
public char firstUniqChar(String s) {
    Map<Character, Integer> freq = new HashMap<>();
    
    // Count frequencies
    for (char c : s.toCharArray()) {
        freq.put(c, freq.getOrDefault(c, 0) + 1);
    }
    
    // Find first character with frequency 1
    for (char c : s.toCharArray()) {
        if (freq.get(c) == 1) {
            return c;
        }
    }
    return '\0'; // No unique character found
}
```

#### 2. Two Sum Pattern
```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> numToIndex = new HashMap<>();
    
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (numToIndex.containsKey(complement)) {
            return new int[]{numToIndex.get(complement), i};
        }
        numToIndex.put(nums[i], i);
    }
    return new int[0];
}
```

#### 3. Grouping Elements
```java
public Map<String, List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> groups = new HashMap<>();
    
    for (String str : strs) {
        char[] chars = str.toCharArray();
        Arrays.sort(chars);
        String key = new String(chars);
        
        groups.computeIfAbsent(key, k -> new ArrayList<>()).add(str);
    }
    
    return groups;
}
```

---

## Problem-Solving Patterns

### 1. Sliding Window with Map
```java
public int lengthOfLongestSubstring(String s) {
    Map<Character, Integer> charIndex = new HashMap<>();
    int maxLength = 0;
    int left = 0;
    
    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        
        if (charIndex.containsKey(c) && charIndex.get(c) >= left) {
            left = charIndex.get(c) + 1;
        }
        
        charIndex.put(c, right);
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}
```

### 2. Multiple Data Structures
```java
public class ValidParentheses {
    private static final Map<Character, Character> PAIRS = Map.of(
        ')', '(',
        '}', '{',
        ']', '['
    );
    
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        Set<Character> openBrackets = new HashSet<>(PAIRS.values());
        
        for (char c : s.toCharArray()) {
            if (openBrackets.contains(c)) {
                stack.push(c);
            } else if (PAIRS.containsKey(c)) {
                if (stack.isEmpty() || stack.pop() != PAIRS.get(c)) {
                    return false;
                }
            }
        }
        
        return stack.isEmpty();
    }
}
```

---

## Interview Practice Problems

### Problem 1: Top K Frequent Elements
```java
public int[] topKFrequent(int[] nums, int k) {
    // Step 1: Count frequencies
    Map<Integer, Integer> freq = new HashMap<>();
    for (int num : nums) {
        freq.put(num, freq.getOrDefault(num, 0) + 1);
    }
    
    // Step 2: Use bucket sort for O(n) solution
    List<Integer>[] buckets = new List[nums.length + 1];
    for (int num : freq.keySet()) {
        int frequency = freq.get(num);
        if (buckets[frequency] == null) {
            buckets[frequency] = new ArrayList<>();
        }
        buckets[frequency].add(num);
    }
    
    // Step 3: Collect top k elements
    List<Integer> result = new ArrayList<>();
    for (int i = buckets.length - 1; i >= 0 && result.size() < k; i--) {
        if (buckets[i] != null) {
            result.addAll(buckets[i]);
        }
    }
    
    return result.stream().mapToInt(Integer::intValue).toArray();
}
```

### Problem 2: Design LRU Cache
```java
public class LRUCache {
    private final int capacity;
    private final Map<Integer, Node> cache;
    private final Node head, tail;
    
    class Node {
        int key, value;
        Node prev, next;
        
        Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }
    
    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.cache = new HashMap<>();
        this.head = new Node(0, 0);
        this.tail = new Node(0, 0);
        head.next = tail;
        tail.prev = head;
    }
    
    public int get(int key) {
        Node node = cache.get(key);
        if (node != null) {
            moveToHead(node);
            return node.value;
        }
        return -1;
    }
    
    public void put(int key, int value) {
        Node node = cache.get(key);
        
        if (node != null) {
            node.value = value;
            moveToHead(node);
        } else {
            Node newNode = new Node(key, value);
            
            if (cache.size() >= capacity) {
                Node tail = removeTail();
                cache.remove(tail.key);
            }
            
            cache.put(key, newNode);
            addToHead(newNode);
        }
    }
    
    private void addToHead(Node node) {
        node.prev = head;
        node.next = head.next;
        head.next.prev = node;
        head.next = node;
    }
    
    private void removeNode(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }
    
    private void moveToHead(Node node) {
        removeNode(node);
        addToHead(node);
    }
    
    private Node removeTail() {
        Node last = tail.prev;
        removeNode(last);
        return last;
    }
}
```

---

## Performance Analysis

### Time Complexity Comparison

| Operation | Array | ArrayList | HashSet | HashMap | TreeSet | TreeMap |
|-----------|--------|-----------|---------|---------|---------|---------|
| Access    | O(1)   | O(1)      | N/A     | N/A     | N/A     | N/A     |
| Search    | O(n)   | O(n)      | O(1)    | O(1)    | O(log n)| O(log n)|
| Insert    | N/A    | O(1)*     | O(1)    | O(1)    | O(log n)| O(log n)|
| Delete    | N/A    | O(n)      | O(1)    | O(1)    | O(log n)| O(log n)|

*ArrayList insertion is O(n) for middle insertions, O(1) amortized for end insertions.

### Space Complexity
- **Array**: O(n) - exactly n elements
- **ArrayList**: O(n) - capacity might be larger than size
- **HashSet/HashMap**: O(n) - additional overhead for buckets
- **TreeSet/TreeMap**: O(n) - additional overhead for tree structure

---

## Interview Tips

### 1. Choose the Right Data Structure
- **Need fast access by index?** → Array/ArrayList
- **Need to avoid duplicates?** → Set
- **Need key-value mapping?** → Map
- **Need sorted order?** → TreeSet/TreeMap
- **Need insertion order?** → LinkedHashSet/LinkedHashMap

### 2. Common Mistakes to Avoid
- Not handling edge cases (empty arrays, null values)
- Forgetting about integer overflow
- Mixing up `remove(index)` vs `remove(Object)` in ArrayList
- Not considering thread safety when needed

### 3. Optimization Techniques
- Use appropriate data structure for the problem
- Consider space-time tradeoffs
- Combine multiple data structures when beneficial
- Use Java 8+ streams for cleaner code

### 4. Practice Areas
- Implement basic operations from scratch
- Solve problems using each data structure
- Understand when to use which data structure
- Practice explaining time/space complexity

Remember: The key to standing out is not just knowing the data structures, but understanding when and why to use each one, and being able to combine them creatively to solve complex problems efficiently!
