### Advanced Problem: Design LRU Cache
**Problem**: Design a data structure that follows LRU (Least Recently Used) cache constraints.

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
                Node lastNode = removeTail();
                cache.remove(lastNode.key);
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
// Time: O(1) for both get and put operations
// Space: O(capacity)
```

## Additional Interview Questions by Data Structure

### Array Problems
1. **Maximum Subarray Sum** - Kadane's Algorithm
2. **Rotate Array** - Three reversals technique
3. **Merge Sorted Array** - Two pointers from end
4. **Remove Duplicates from Sorted Array** - Two pointers
5. **Find Peak Element** - Binary search variant
6. **3Sum** - Sort + two pointers
7. **Container With Most Water** - Two pointers
8. **Trapping Rain Water** - Two pointers or stack

### ArrayList Problems
1. **Insert Delete GetRandom O(1)** - ArrayList + HashMap
2. **Design Dynamic Array** - Implement resizing logic
3. **Spiral Matrix** - Boundary manipulation
4. **Pascal's Triangle** - Dynamic programming with lists
5. **Summary Ranges** - Consecutive number grouping

### Set Problems
1. **Intersection of Two Arrays II** - HashMap for counts
2. **Valid Sudoku** - Multiple sets for validation
3. **Longest Substring Without Repeating Characters** - Sliding window + set
4. **Word Break** - Set + dynamic programming
5. **Happy Number** - Cycle detection with set

### Map Problems
1. **Word Pattern** - Bijection mapping
2. **Isomorphic Strings** - Character mapping
3. **Logger Rate Limiter** - Timestamp mapping
4. **Design Twitter** - Complex data structure design
5. **Subarray Sum Equals K** - Prefix sum + map
6. **Copy List with Random Pointer** - Node mapping
7. **Clone Graph** - Node mapping with DFS/BFS

## Common Interview Scenarios

### When Interviewer Asks "Can you optimize this?"
1. **Time Complexity**: Can you reduce from O(n²) to O(n log n) or O(n)?
2. **Space Complexity**: Can you reduce space usage?
3. **Different Data Structure**: Would a different structure be more efficient?

### Red Flags to Avoid
1. **Not asking clarifying questions** about input constraints
2. **Not discussing time/space complexity** upfront
3. **Not testing with examples** before coding
4. **Writing overly complex solutions** when simple ones exist
5. **Not handling edge cases** (empty input, single element, etc.)

### Interview Communication Tips
1. **Think out loud** - Explain your approach
2. **Start with brute force** - Then optimize
3. **Draw examples** - Visual representation helps
4. **Trace through code** - Walk through with sample input
5. **Discuss tradeoffs** - Why choose one approach over another

---

## Final Mastery Checklist

### Arrays ✓
- [ ] Can implement all common patterns (two pointers, sliding window, etc.)
- [ ] Know all Arrays utility methods
- [ ] Understand time/space complexity of operations
- [ ] Can solve problems like 3Sum, Trapping Rain Water

### ArrayList ✓
- [ ] Know when to use ArrayList vs Array
- [ ] Understand dynamic resizing and amortized complexity  
- [ ] Can implement custom dynamic array
- [ ] Master list manipulation problems

### Sets ✓
- [ ] Know differences between HashSet, LinkedHashSet, TreeSet
- [ ] Can choose appropriate set based on requirements
- [ ] Understand set operations and their complexities
- [ ] Master duplicate detection and uniqueness problems

### Maps ✓
- [ ] Know differences between HashMap, LinkedHashMap, TreeMap
- [ ] Master frequency counting and grouping patterns
- [ ] Understand hash collisions and load factors
- [ ] Can implement complex designs using maps

### Problem Solving ✓
- [ ] Can identify which data structure to use for given problem
- [ ] Know common algorithmic patterns for each structure
- [ ] Can analyze and optimize time/space complexity
- [ ] Practice explaining solutions clearly

**Remember**: Consistent practice with these patterns will make you stand out in interviews. Focus on understanding WHY each data structure is optimal for specific scenarios, not just HOW to use them!# Java Data Structures Mastery Guide for Interview Success

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

### Complete Method Reference

#### Array Utility Methods (via Arrays class)
```java
// SORTING
Arrays.sort(array)                      // Sort in natural order
Arrays.sort(array, fromIndex, toIndex)  // Sort portion of array
Arrays.sort(array, comparator)          // Sort with custom comparator
Arrays.parallelSort(array)              // Parallel sort (for large arrays)

// SEARCHING
Arrays.binarySearch(array, key)         // Binary search (array must be sorted)
Arrays.binarySearch(array, fromIndex, toIndex, key) // Binary search in range
Arrays.binarySearch(array, key, comparator) // Binary search with comparator

// COMPARISON
Arrays.equals(array1, array2)           // Element-wise equality
Arrays.deepEquals(array1, array2)       // Deep equality for multi-dimensional arrays
Arrays.compare(array1, array2)          // Lexicographic comparison
Arrays.mismatch(array1, array2)         // Index of first mismatch

// COPYING
Arrays.copyOf(array, newLength)         // Copy array with new length
Arrays.copyOfRange(array, from, to)     // Copy specific range
array.clone()                           // Create shallow copy

// FILLING
Arrays.fill(array, value)               // Fill entire array with value
Arrays.fill(array, fromIndex, toIndex, value) // Fill range with value

// CONVERSION
Arrays.asList(array)                    // Convert to List (fixed-size)
Arrays.toString(array)                  // String representation
Arrays.deepToString(array)              // Deep string representation for multi-dimensional

// STREAMING (Java 8+)
Arrays.stream(array)                    // Create stream from array
Arrays.stream(array, startInclusive, endExclusive) // Stream from range

// PARALLEL OPERATIONS (Java 8+)
Arrays.parallelSetAll(array, generator) // Set all elements using generator function
Arrays.setAll(array, generator)         // Set all elements using generator function
Arrays.parallelPrefix(array, operator)  // Cumulative operation in parallel
```

#### Array Examples
```java
public void arrayUtilities() {
    int[] arr = {5, 2, 8, 1, 9, 3};
    
    // Sorting
    Arrays.sort(arr);                    // [1, 2, 3, 5, 8, 9]
    
    // Searching (array must be sorted)
    int index = Arrays.binarySearch(arr, 5); // Returns index of 5
    
    // Copying
    int[] copy = Arrays.copyOf(arr, 10);  // Copy with new length 10
    int[] range = Arrays.copyOfRange(arr, 1, 4); // [2, 3, 5]
    
    // Filling
    int[] filled = new int[5];
    Arrays.fill(filled, 42);             // [42, 42, 42, 42, 42]
    
    // Conversion
    List<Integer> list = Arrays.asList(1, 2, 3, 4, 5); // Fixed-size list
    String str = Arrays.toString(arr);    // "[1, 2, 3, 5, 8, 9]"
    
    // Comparison
    int[] arr2 = {1, 2, 3, 5, 8, 9};
    boolean equal = Arrays.equals(arr, arr2); // true
}

### Common Array Patterns

#### Pattern 1: Two Pointers Technique
**When to use**: Palindromes, sorted arrays, finding pairs
**Time Complexity**: O(n), **Space Complexity**: O(1)

**Template**:
```java
int left = 0, right = array.length - 1;
while (left < right) {
    // Process elements at left and right
    if (condition) {
        left++;
    } else {
        right--;
    }
}
```
**Example - Valid Palindrome**:
```java
public boolean isPalindrome(String s) {
    char[] chars = s.toLowerCase().replaceAll("[^a-z0-9]", "").toCharArray();
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

#### Pattern 2: Sliding Window
**When to use**: Subarray problems, finding max/min in windows
**Time Complexity**: O(n), **Space Complexity**: O(1)

**Fixed Window Template**:
```java
int windowSum = 0;
for (int i = 0; i < k; i++) {
    windowSum += arr[i];
}
int maxSum = windowSum;

for (int i = k; i < arr.length; i++) {
    windowSum = windowSum + arr[i] - arr[i - k];
    maxSum = Math.max(maxSum, windowSum);
}
```

**Variable Window Template**:
```java
int left = 0, right = 0;
while (right < array.length) {
    // Expand window
    // Add arr[right] to window
    
    while (window needs shrinking) {
        // Shrink window
        // Remove arr[left] from window
        left++;
    }
    
    // Update result
    right++;
}
```
**Example - Maximum Subarray Sum of Size K**:
```java
public int maxSubarraySum(int[] arr, int k) {
    int windowSum = 0;
    
    // Calculate sum of first window
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    int maxSum = windowSum;
    
    // Slide the window
    for (int i = k; i < arr.length; i++) {
        windowSum += arr[i] - arr[i - k];
        maxSum = Math.max(maxSum, windowSum);
    }
    return maxSum;
}
```

#### Pattern 3: Prefix Sum
**When to use**: Range sum queries, subarray sum problems
**Time Complexity**: O(n) build, O(1) query, **Space Complexity**: O(n)

**Template**:
```java
int[] prefixSum = new int[arr.length + 1];
for (int i = 0; i < arr.length; i++) {
    prefixSum[i + 1] = prefixSum[i] + arr[i];
}
// Range sum from index i to j: prefixSum[j+1] - prefixSum[i]
```
**Example - Range Sum Query**:
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

#### Pattern 4: Kadane's Algorithm (Maximum Subarray)
**When to use**: Finding maximum sum contiguous subarray
**Time Complexity**: O(n), **Space Complexity**: O(1)

```java
public int maxSubArray(int[] nums) {
    int maxSoFar = nums[0];
    int maxEndingHere = nums[0];
    
    for (int i = 1; i < nums.length; i++) {
        maxEndingHere = Math.max(nums[i], maxEndingHere + nums[i]);
        maxSoFar = Math.max(maxSoFar, maxEndingHere);
    }
    return maxSoFar;
}
```

#### Pattern 5: Dutch National Flag (3-way Partitioning)
**When to use**: Sorting with 3 colors/values, partitioning
**Time Complexity**: O(n), **Space Complexity**: O(1)

```java
public void sortColors(int[] nums) {
    int low = 0, mid = 0, high = nums.length - 1;
    
    while (mid <= high) {
        if (nums[mid] == 0) {
            swap(nums, low++, mid++);
        } else if (nums[mid] == 1) {
            mid++;
        } else {
            swap(nums, mid, high--);
        }
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

### ArrayList Patterns

#### Pattern 1: Dynamic Programming with Lists
**When to use**: Building results incrementally, Pascal's triangle, etc.

```java
// Pascal's Triangle Row
public List<Integer> generatePascalRow(int rowIndex) {
    List<Integer> row = new ArrayList<>(Collections.nCopies(rowIndex + 1, 1));
    
    for (int i = 1; i < rowIndex; i++) {
        for (int j = i; j > 0; j--) {
            row.set(j, row.get(j) + row.get(j - 1));
        }
    }
    return row;
}
```

#### Pattern 2: Two Pointers on List
**When to use**: Removing elements, rearranging elements

```java
// Move Zeros to End
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

### Complete Method Reference

#### HashSet Methods
```java
// CONSTRUCTORS
HashSet<T> set = new HashSet<>();               // Empty set
HashSet<T> set = new HashSet<>(capacity);       // With initial capacity
HashSet<T> set = new HashSet<>(capacity, loadFactor); // With capacity and load factor
HashSet<T> set = new HashSet<>(collection);     // From existing collection

// ADDING ELEMENTS
boolean add(E element)                    // Add element, returns true if added (wasn't present)
boolean addAll(Collection<? extends E> c) // Add all elements from collection

// CHECKING ELEMENTS
boolean contains(Object o)                // Check if contains element
boolean isEmpty()                         // Check if empty
int size()                               // Number of elements

// REMOVING ELEMENTS
boolean remove(Object o)                  // Remove element, returns true if was present
boolean removeAll(Collection<?> c)        // Remove all elements in collection
boolean retainAll(Collection<?> c)        // Keep only elements in collection
boolean removeIf(Predicate<? super E> filter) // Remove elements matching predicate
void clear()                             // Remove all elements

// SET OPERATIONS
boolean containsAll(Collection<?> c)      // Check if contains all elements in collection

// CONVERSION AND COPYING
Object[] toArray()                       // Convert to Object array
<T> T[] toArray(T[] a)                  // Convert to typed array
HashSet<E> clone()                       // Shallow copy

// ITERATION
Iterator<E> iterator()                   // Iterator over elements
void forEach(Consumer<? super E> action) // Apply action to each element
Spliterator<E> spliterator()            // Spliterator for parallel processing

// STREAM OPERATIONS (Java 8+)
Stream<E> stream()                       // Sequential stream
Stream<E> parallelStream()               // Parallel stream
```

#### LinkedHashSet Methods
```java
// Inherits all HashSet methods plus:
// - Maintains insertion order
// - Same time complexity as HashSet
// - Slightly more memory overhead
```

#### TreeSet Methods
```java
// CONSTRUCTORS
TreeSet<T> set = new TreeSet<>();               // Natural ordering
TreeSet<T> set = new TreeSet<>(comparator);     // Custom comparator
TreeSet<T> set = new TreeSet<>(collection);     // From collection
TreeSet<T> set = new TreeSet<>(sortedSet);      // From sorted set

// All Set methods PLUS NavigableSet methods:

// NAVIGATION METHODS
E first()                                // Lowest element
E last()                                 // Highest element
E lower(E element)                       // Largest element < given element
E floor(E element)                       // Largest element ≤ given element
E ceiling(E element)                     // Smallest element ≥ given element
E higher(E element)                      // Smallest element > given element

// POLLING METHODS
E pollFirst()                            // Remove and return first element
E pollLast()                             // Remove and return last element

// VIEW METHODS
NavigableSet<E> descendingSet()          // Reverse-ordered view
Iterator<E> descendingIterator()         // Reverse iterator
SortedSet<E> subSet(E fromElement, E toElement)     // View between elements
SortedSet<E> headSet(E toElement)        // View of elements < toElement
SortedSet<E> tailSet(E fromElement)      // View of elements ≥ fromElement
NavigableSet<E> subSet(E from, boolean fromInc, E to, boolean toInc) // Inclusive/exclusive bounds

// COMPARATOR
Comparator<? super E> comparator()       // Comparator used, or null for natural ordering
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

#### Pattern 1: Detecting Duplicates
**When to use**: Finding duplicates, unique elements
**Time Complexity**: O(n), **Space Complexity**: O(n)

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

#### Pattern 2: Set Intersection/Union
**When to use**: Finding common elements, combining sets

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

#### Pattern 3: Sliding Window with Set
**When to use**: Finding longest substring without repeating characters

```java
public int lengthOfLongestSubstring(String s) {
    Set<Character> window = new HashSet<>();
    int left = 0, maxLength = 0;
    
    for (int right = 0; right < s.length(); right++) {
        while (window.contains(s.charAt(right))) {
            window.remove(s.charAt(left));
            left++;
        }
        window.add(s.charAt(right));
        maxLength = Math.max(maxLength, right - left + 1);
    }
    return maxLength;
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

### Complete Method Reference

#### HashMap Methods
```java
// CONSTRUCTORS
HashMap<K,V> map = new HashMap<>();             // Empty map
HashMap<K,V> map = new HashMap<>(capacity);     // With initial capacity
HashMap<K,V> map = new HashMap<>(capacity, loadFactor); // With capacity and load factor
HashMap<K,V> map = new HashMap<>(map);          // Copy constructor

// ADDING/UPDATING
V put(K key, V value)                    // Put key-value pair, returns previous value
void putAll(Map<? extends K, ? extends V> m) // Put all mappings from another map
V putIfAbsent(K key, V value)           // Put only if key not present, returns existing/null

// ACCESSING
V get(Object key)                        // Get value for key, null if not found
V getOrDefault(Object key, V defaultValue) // Get value or default if key not found
boolean containsKey(Object key)          // Check if key exists
boolean containsValue(Object value)      // Check if value exists
boolean isEmpty()                        // Check if empty
int size()                              // Number of key-value pairs

// REMOVING
V remove(Object key)                     // Remove mapping, return previous value
boolean remove(Object key, Object value) // Remove only if key maps to value
void clear()                            // Remove all mappings

// REPLACEMENT METHODS (Java 8+)
V replace(K key, V value)               // Replace value if key exists
boolean replace(K key, V oldValue, V newValue) // Replace only if current value matches
void replaceAll(BiFunction<? super K, ? super V, ? extends V> function) // Replace all values

// COMPUTE METHODS (Java 8+)
V compute(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction)
V computeIfAbsent(K key, Function<? super K, ? extends V> mappingFunction)
V computeIfPresent(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction)
V merge(K key, V value, BiFunction<? super V, ? super V, ? extends V> remappingFunction)

// VIEW COLLECTIONS
Set<K> keySet()                         // Set view of keys
Collection<V> values()                  // Collection view of values
Set<Map.Entry<K,V>> entrySet()         // Set view of key-value mappings

// ITERATION
void forEach(BiConsumer<? super K, ? super V> action) // Apply action to each entry

// COPYING
HashMap<K,V> clone()                    // Shallow copy
```

#### LinkedHashMap Methods
```java
// Inherits all HashMap methods plus:
// - Maintains insertion order (or access order if specified)
// - Constructor with accessOrder parameter:
LinkedHashMap<K,V> map = new LinkedHashMap<>(capacity, loadFactor, accessOrder);

// SPECIAL METHODS
protected boolean removeEldestEntry(Map.Entry<K,V> eldest) // Override for LRU cache behavior
```

#### TreeMap Methods
```java
// CONSTRUCTORS
TreeMap<K,V> map = new TreeMap<>();            // Natural ordering
TreeMap<K,V> map = new TreeMap<>(comparator);  // Custom comparator
TreeMap<K,V> map = new TreeMap<>(map);         // Copy constructor
TreeMap<K,V> map = new TreeMap<>(sortedMap);   // From sorted map

// All Map methods PLUS NavigableMap methods:

// NAVIGATION METHODS
Map.Entry<K,V> firstEntry()             // Lowest key entry
Map.Entry<K,V> lastEntry()              // Highest key entry
K firstKey()                            // Lowest key
K lastKey()                             // Highest key
Map.Entry<K,V> lowerEntry(K key)        // Entry with largest key < given key
Map.Entry<K,V> floorEntry(K key)        // Entry with largest key ≤ given key
Map.Entry<K,V> ceilingEntry(K key)      // Entry with smallest key ≥ given key
Map.Entry<K,V> higherEntry(K key)       // Entry with smallest key > given key
K lowerKey(K key)                       // Largest key < given key
K floorKey(K key)                       // Largest key ≤ given key
K ceilingKey(K key)                     // Smallest key ≥ given key
K higherKey(K key)                      // Smallest key > given key

// POLLING METHODS
Map.Entry<K,V> pollFirstEntry()         // Remove and return first entry
Map.Entry<K,V> pollLastEntry()          // Remove and return last entry

// VIEW METHODS
NavigableMap<K,V> descendingMap()       // Reverse-ordered view
NavigableSet<K> navigableKeySet()       // Navigable set of keys
NavigableSet<K> descendingKeySet()      // Reverse-ordered key set
SortedMap<K,V> subMap(K fromKey, K toKey) // View between keys
SortedMap<K,V> headMap(K toKey)         // View of entries < toKey
SortedMap<K,V> tailMap(K fromKey)       // View of entries ≥ fromKey
NavigableMap<K,V> subMap(K from, boolean fromInc, K to, boolean toInc) // Inclusive/exclusive

// COMPARATOR
Comparator<? super K> comparator()      // Comparator used, or null for natural ordering
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

#### Pattern 1: Frequency Counter
**When to use**: Counting occurrences, anagram problems
**Time Complexity**: O(n), **Space Complexity**: O(k) where k is unique elements

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

#### Pattern 2: Two Sum/Complement Pattern
**When to use**: Finding pairs that sum to target
**Time Complexity**: O(n), **Space Complexity**: O(n)

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

#### Pattern 3: Grouping/Categorizing
**When to use**: Group anagrams, group by properties

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

#### Pattern 4: Prefix Sum with Map
**When to use**: Subarray sum equals K, continuous subarray problems

```java
public int subarraySum(int[] nums, int k) {
    Map<Integer, Integer> prefixSumCount = new HashMap<>();
    prefixSumCount.put(0, 1);
    
    int count = 0, prefixSum = 0;
    for (int num : nums) {
        prefixSum += num;
        count += prefixSumCount.getOrDefault(prefixSum - k, 0);
        prefixSumCount.put(prefixSum, prefixSumCount.getOrDefault(prefixSum, 0) + 1);
    }
    return count;
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

### Easy Level Problems

#### Problem 1: Two Sum
**Problem**: Given an array of integers and a target, return indices of two numbers that add up to target.
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
// Time: O(n), Space: O(n)
```

#### Problem 2: Valid Anagram
**Problem**: Check if two strings are anagrams.
```java
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) return false;
    
    Map<Character, Integer> count = new HashMap<>();
    
    // Count characters in first string
    for (char c : s.toCharArray()) {
        count.put(c, count.getOrDefault(c, 0) + 1);
    }
    
    // Subtract characters from second string
    for (char c : t.toCharArray()) {
        count.put(c, count.getOrDefault(c, 0) - 1);
        if (count.get(c) == 0) {
            count.remove(c);
        }
    }
    
    return count.isEmpty();
}
// Time: O(n), Space: O(1) - at most 26 characters
```

#### Problem 3: Contains Duplicate
**Problem**: Check if array contains any duplicate values.
```java
public boolean containsDuplicate(int[] nums) {
    Set<Integer> seen = new HashSet<>();
    for (int num : nums) {
        if (!seen.add(num)) {
            return true;
        }
    }
    return false;
}
// Time: O(n), Space: O(n)
```

#### Problem 4: Best Time to Buy and Sell Stock
**Problem**: Find maximum profit from buying and selling stock once.
```java
public int maxProfit(int[] prices) {
    int minPrice = prices[0];
    int maxProfit = 0;
    
    for (int i = 1; i < prices.length; i++) {
        if (prices[i] < minPrice) {
            minPrice = prices[i];
        } else {
            maxProfit = Math.max(maxProfit, prices[i] - minPrice);
        }
    }
    return maxProfit;
}
// Time: O(n), Space: O(1)
```

### Medium Level Problems

#### Problem 5: Group Anagrams
**Problem**: Group strings that are anagrams of each other.
```java
public List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> groups = new HashMap<>();
    
    for (String str : strs) {
        char[] chars = str.toCharArray();
        Arrays.sort(chars);
        String key = new String(chars);
        
        groups.computeIfAbsent(key, k -> new ArrayList<>()).add(str);
    }
    
    return new ArrayList<>(groups.values());
}
// Time: O(n * m log m) where n = number of strings, m = average length
// Space: O(n * m)
```

#### Problem 6: Product of Array Except Self
**Problem**: Return array where each element is product of all other elements.
```java
public int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    
    // Left products
    result[0] = 1;
    for (int i = 1; i < n; i++) {
        result[i] = result[i - 1] * nums[i - 1];
    }
    
    // Right products
    int rightProduct = 1;
    for (int i = n - 1; i >= 0; i--) {
        result[i] *= rightProduct;
        rightProduct *= nums[i];
    }
    
    return result;
}
// Time: O(n), Space: O(1) excluding output array
```

#### Problem 7: Longest Consecutive Sequence
**Problem**: Find length of longest consecutive elements sequence.
```java
public int longestConsecutive(int[] nums) {
    Set<Integer> numSet = new HashSet<>();
    for (int num : nums) {
        numSet.add(num);
    }
    
    int maxLength = 0;
    for (int num : numSet) {
        // Check if it's the start of a sequence
        if (!numSet.contains(num - 1)) {
            int currentNum = num;
            int currentLength = 1;
            
            while (numSet.contains(currentNum + 1)) {
                currentNum++;
                currentLength++;
            }
            
            maxLength = Math.max(maxLength, currentLength);
        }
    }
    return maxLength;
}
// Time: O(n), Space: O(n)
```

### Hard Level Problems

#### Problem 8: Sliding Window Maximum
**Problem**: Find maximum in each sliding window of size k.
```java
public int[] maxSlidingWindow(int[] nums, int k) {
    if (nums == null || nums.length == 0) return new int[0];
    
    Deque<Integer> deque = new ArrayDeque<>();
    int[] result = new int[nums.length - k + 1];
    
    for (int i = 0; i < nums.length; i++) {
        // Remove elements outside window
        while (!deque.isEmpty() && deque.peekFirst() < i - k + 1) {
            deque.pollFirst();
        }
        
        // Remove smaller elements from rear
        while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
            deque.pollLast();
        }
        
        deque.offerLast(i);
        
        // Add to result when window is complete
        if (i >= k - 1) {
            result[i - k + 1] = nums[deque.peekFirst()];
        }
    }
    
    return result;
}
// Time: O(n), Space: O(k)
```

#### Problem 9: Top K Frequent Elements
**Problem**: Find k most frequent elements in array.
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
// Time: O(n), Space: O(n)
```

#### Problem 10: Valid Parentheses
**Problem**: Check if parentheses string is valid.
```java
public boolean isValid(String s) {
    Stack<Character> stack = new Stack<>();
    Map<Character, Character> pairs = Map.of(')', '(', '}', '{', ']', '[');
    
    for (char c : s.toCharArray()) {
        if (pairs.containsValue(c)) {  // Opening bracket
            stack.push(c);
        } else if (pairs.containsKey(c)) {  // Closing bracket
            if (stack.isEmpty() || stack.pop() != pairs.get(c)) {
                return false;
            }
        }
    }
    
    return stack.isEmpty();
}
// Time: O(n), Space: O(n)
```

### Problem-Solving Tips
1. **Identify the pattern** - Two pointers, sliding window, frequency counting, etc.
2. **Choose optimal data structure** - Consider time/space tradeoffs
3. **Handle edge cases** - Empty arrays, single elements, duplicates
4. **Optimize step by step** - Start with brute force, then optimize
5. **Test with examples** - Walk through your solution with sample inputs
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
