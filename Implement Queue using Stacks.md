# 🎯 LeetCode 232: Implement Queue using Stacks

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (`push`, `peek`, `pop`, and `empty`).

Implement the `MyQueue` class:
- `void push(int x)` Pushes element `x` to the back of the queue.
- `int pop()` Removes the element from the front of the queue and returns it.
- `int peek()` Returns the element at the front of the queue.
- `boolean empty()` Returns `true` if the queue is empty, `false` otherwise.

#### Notes

- You must use only standard operations of a stack, which means only push to top, peek/pop from top, size, and is empty operations are valid.
- Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.

#### Examples

- **Example 1**:
  - **Input**: `["MyQueue", "push", "push", "peek", "pop", "empty"]`, `[[], [1], [2], [], [], []]`
  - **Output**: `[null, null, null, 1, 1, false]`
  - **Explanation**: 
    - `MyQueue myQueue = new MyQueue();`
    - `myQueue.push(1); // queue is: [1]`
    - `myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)`
    - `myQueue.peek(); // return 1`
    - `myQueue.pop(); // return 1, queue is [2]`
    - `myQueue.empty(); // return false`

#### Constraints

- $1 \le x \le 9$
- At most $100$ calls will be made to `push`, `pop`, `peek`, and `empty`.
- All the calls to `pop` and `peek` are valid.

---

### 💡 Intuition & Strategy

1. **Two-Stack Architecture (`inStack` & `outStack`)**:
   - A stack is Last-In-First-Out (LIFO), whereas a queue requires First-In-First-Out (FIFO). 
   - By using two stacks, we can reverse the order of elements:
     - **`inStack`**: Captures all incoming elements during `push(x)` operations in LIFO order.
     - **`outStack`**: Reverses the order when needed to serve `pop()` and `peek()` in FIFO order.

2. **Lazy Transfer via `shiftStack()`**:
   - When a `pop()` or `peek()` is requested, we check if `outStack` is empty.
   - If it is, we pop all elements from `inStack` one by one and push them into `outStack`. This flips their order so that the oldest element lands at the top of `outStack`.

3. **Amortized $O(1)$ Time Complexity**:
   - While a single `pop` or `peek` operation might take $O(n)$ time when `outStack` is empty, each element is pushed and popped from each stack at a maximum of twice in its lifetime.
   - Thus, over $n$ operations, the total time is bounded by $O(n)$, giving an **amortized $O(1)$** time complexity per operation.

4. **Complexity**:
   - **Time Complexity**: 
     - `push(x)`: $O(1)$
     - `empty()`: $O(1)$
     - `pop()` / `peek()`: Amortized $O(1)$
   - **Space Complexity**: $O(n)$ auxiliary space to store elements across the two stacks.

---

### 💻 Java Solution

```java
/**
 * Problem: Implement Queue using Stacks (LeetCode 232)
 * Language: Java 17
 * Time Complexity: Amortized O(1) for pop/peek, O(1) for push/empty
 * Space Complexity: O(N)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.Stack;

class MyQueue {
    private Stack<Integer> inStack;
    private Stack<Integer> outStack;

    public MyQueue() {
        inStack = new Stack<>();
        outStack = new Stack<>();
    }
    
    public void push(int x) {
        inStack.push(x);
    }
    
    public int pop() {
        shiftStack();
        return outStack.pop();
    }
    
    public int peek() {
        shiftStack();
        return outStack.peek();
    }
    
    public boolean empty() {
        return inStack.isEmpty() && outStack.isEmpty();
    }
    
    /**
     * Helper method to transfer elements from inStack to outStack 
     * only when outStack is completely empty, preserving FIFO order.
     */
    private void shiftStack() {
        if (outStack.isEmpty()) {
            while (!inStack.isEmpty()) {
                outStack.push(inStack.pop());
            }
        }
    }
}
