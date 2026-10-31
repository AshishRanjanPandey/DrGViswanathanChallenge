# 🎯 LeetCode 146: LRU Cache

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Design a data structure that follows the constraints of a **Least Recently Used (LRU) cache**.

Implement the `LRUCache` class:
- `LRUCache(int capacity)`: Initialize the LRU cache with positive size `capacity`.
- `int get(int key)`: Return the value of the key if the key exists, otherwise return `-1`.
- `void put(int key, int value)`: Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.

The functions `get` and `put` must each run in $O(1)$ average time complexity.

#### Examples

- **Example 1**:
  - **Input**: 
    `["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]`
    `[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]`
  - **Output**: `[null, null, null, 1, null, -1, null, -1, 3, 4]`
  - **Explanation**: 
    
    ```java
    LRUCache lRUCache = new LRUCache(2);
    lRUCache.put(1, 1); // cache is {1=1}
    lRUCache.put(2, 2); // cache is {1=1, 2=2}
    lRUCache.get(1);    // return 1
    lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
    lRUCache.get(2);    // returns -1 (not found)
    lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
    lRUCache.get(1);    // return -1 (not found)
    lRUCache.get(3);    // return 3
    lRUCache.get(4);    // return 4
    ```

#### Constraints

- $1 \le \text{capacity} \le 3000$
- $0 \le \text{key} \le 10^4$
- $0 \le \text{value} \le 10^5$
- At most $2 \times 10^5$ calls will be made to `get` and `put`.

---

### 💡 Intuition & Strategy

1. **Leveraging Built-in Data Structures**:
   - To achieve $O(1)$ time complexity for both lookup and insertion/deletion, we typically need a HashMap combined with a Doubly Linked List. 
   - Java provides `LinkedHashMap`, which natively combines a hash table and a linked list to maintain predictable iteration order, making it the perfect tool for this problem.

2. **Access-Order Configuration**:
   - By default, `LinkedHashMap` maintains *insertion* order. 
   - By calling `super(capacity, 0.75f, true)` in the constructor, we activate **access-order**. The `true` flag ensures that every time an entry is accessed (via `get` or `put`), it is automatically moved to the end of the internal linked list (marking it as the most recently used).

3. **Automatic Eviction Mechanism**:
   - `LinkedHashMap` provides a virtual method `removeEldestEntry(Map.Entry<K,V> eldest)` which is invoked automatically after every `put` operation.
   - By overriding this method to return `size() > capacity`, the map automatically deletes the node at the head of the list (the least recently used item) whenever the defined capacity is exceeded.

4. **Complexity**:
   - **Time Complexity**: $O(1)$
     - Both `get` and `put` operations rely on the underlying HashMap for lookups and linked list pointer updates, taking constant time.
   - **Space Complexity**: $O(\text{capacity})$
     - The map stores at most `capacity` number of key-value pairs at any given time.

---

### 💻 Java Solution

```java
/**
 * Problem: LRU Cache (LeetCode 146)
 * Language: Java
 * Time Complexity: O(1) for get and put
 * Space Complexity: O(capacity)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.LinkedHashMap;
import java.util.Map;

class LRUCache extends LinkedHashMap<Integer, Integer> {
    private int capacity;

    public LRUCache(int capacity) {
        // Initialize LinkedHashMap with:
        // - capacity
        // - 0.75f load factor (default)
        // - true for access-order (crucial for LRU)
        super(capacity, 0.75f, true);
        this.capacity = capacity;
    }
    
    public int get(int key) {
        return super.getOrDefault(key, -1);
    }
    
    public void put(int key, int value) {
        super.put(key, value);
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
        // Automatically evict the least recently used entry if capacity is exceeded
        return size() > capacity;
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```