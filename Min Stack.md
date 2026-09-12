# 🎯 LeetCode 155: Min Stack

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:
- `MinStack()` initializes the stack object.
- `void push(int val)` pushes the element `val` onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with $O(1)$ time complexity for each function.

#### Examples

- **Example 1**:
  - **Input**:
    ```text
    ["MinStack","push","push","push","getMin","pop","top","getMin"]
    [[],[-2],[0],[-3],[],[],[],[]]
    ```
  - **Output**: `[null,null,null,null,-3,null,0,-2]`
  - **Explanation**:
    ```java
    MinStack minStack = new MinStack();
    minStack.push(-2);
    minStack.push(0);
    minStack.push(-3);
    minStack.getMin(); // return -3
    minStack.pop();
    minStack.top();    // return 0
    minStack.getMin(); // return -2
    ```

#### Constraints

- $-2^{31} \le \text{val} \le 2^{31} - 1$
- Methods `pop`, `top`, and `getMin` operations will always be called on non-empty stacks.
- At most $3 \times 10^4$ calls will be made to `push`, `pop`, `top`, and `getMin`.

---

### 💡 Intuition & Strategy

1. **The Core Challenge**:
   - A standard stack provides $O(1)$ operations for `push`, `pop`, and `top`.
   - The challenge is supporting `getMin()` in $O(1)$ time without rescanning the stack when elements are pushed or popped.

2. **Tracking State with Nodes**:
   - To make `getMin()` run in $O(1)$ time, every entry in the stack must remember the minimum value present in the stack up to that point.
   - Using an explicit singly linked list (where each node tracks both its `val` and the `min` at its level) avoids overhead from container objects or multiple data structures.

3. **Operation Logic**:
   - **`push(val)`**: Create a new node. If the stack is empty, `min = val`. Otherwise, `min = Math.min(val, head.min)`. Set this new node as the new `head`.
   - **`pop()`**: Advance `head = head.next`. The previous minimum is naturally restored because the new `head` already caches the minimum of the remaining elements.
   - **`top()`**: Return `head.val`.
   - **`getMin()`**: Return `head.min`.

4. **Complexity**:
   - **Time Complexity**: $O(1)$ for `push`, `pop`, `top`, and `getMin`.
   - **Space Complexity**: $O(N)$ auxiliary space to store $N$ elements and their associated running minimums.

---

### 💻 Java Solution

```java
/**
 * Problem: Min Stack (LeetCode 155)
 * Language: Java 17
 * Time Complexity: O(1) for all operations
 * Space Complexity: O(N)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
class MinStack {

    private static class Node {
        int val;
        int min;
        Node next;

        Node(int val, int min, Node next) {
            this.val = val;
            this.min = min;
            this.next = next;
        }
    }

    private Node head;

    public MinStack() {
        head = null;
    }
    
    public void push(int val) {
        if (head == null) {
            head = new Node(val, val, null);
        } else {
            head = new Node(val, Math.min(val, head.min), head);
        }
    }
    
    public void pop() {
        head = head.next;
    }
    
    public int top() {
        return head.val;
    }
    
    public int getMin() {
        return head.min;
    }
}
