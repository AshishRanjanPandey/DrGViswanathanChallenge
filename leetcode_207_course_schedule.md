# 🎯 LeetCode 207: Course Schedule

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

#### Examples

- **Example 1**:
  - **Input**: `numCourses = 2`, `prerequisites = [[1,0]]`
  - **Output**: `true`
  - **Explanation**: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So it is possible.

- **Example 2**:
  - **Input**: `numCourses = 2`, `prerequisites = [[1,0],[0,1]]`
  - **Output**: `false`
  - **Explanation**: There are a total of 2 courses to take. To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.

#### Constraints

- $1 \le \text{numCourses} \le 2000$
- $0 \le \text{prerequisites.length} \le 5000$
- $\text{prerequisites}[i]\text{.length} == 2$
- $0 \le a_i, b_i < \text{numCourses}$
- All the pairs `prerequisites[i]` are unique.

---

### 💡 Intuition & Strategy

1. **Graph Representation**:
   - This problem can be modeled as a **Directed Graph**. Each course is a node, and a prerequisite `[a, b]` represents a directed edge from `b` to `a` ($b \rightarrow a$).
   - The problem boils down to **Cycle Detection**. If there is a cycle in the graph, it means two or more courses are prerequisites for each other, making it impossible to complete them. If there is no cycle, a valid ordering exists (Topological Sort).

2. **Kahn's Algorithm (BFS for Topological Sort)**:
   - Calculate the **in-degree** (number of prerequisites) for every node.
   - Build an **adjacency list** to easily traverse from a course to all courses that depend on it.

3. **Queue Processing**:
   - Initialize a queue and add all courses that have an **in-degree of 0** (courses with no prerequisites).
   - While the queue is not empty:
     - Pop a course. Increment our "completed courses" counter.
     - For every course that depends on the popped course, decrement its in-degree by 1 (since we just completed one of its prerequisites).
     - If any dependent course's in-degree drops to 0, add it to the queue.

4. **Complexity**:
   - **Time Complexity**: $O(V + E)$
     - Where $V$ is `numCourses` (vertices) and $E$ is `prerequisites.length` (edges). We initialize the graph by visiting every edge once, and during the BFS, we visit every node and every edge exactly once.
   - **Space Complexity**: $O(V + E)$ auxiliary memory
     - The adjacency list takes $O(V + E)$ space. The `inDegree` array and the Queue each take up to $O(V)$ space.

---

### 💻 Java Solution

```java
/**
 * Problem: Course Schedule (LeetCode 207)
 * Language: Java 17
 * Time Complexity: O(V + E)
 * Space Complexity: O(V + E)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // Step 1: Build the adjacency list and in-degree array
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) {
            adj.add(new ArrayList<>());
        }
        
        int[] inDegree = new int[numCourses];
        
        for (int[] pre : prerequisites) {
            int course = pre[0];
            int prerequisite = pre[1];
            adj.get(prerequisite).add(course); // Edge: prerequisite -> course
            inDegree[course]++; // Increment in-degree for the dependent course
        }
        
        // Step 2: Add all courses with 0 prerequisites to a queue
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) {
                queue.offer(i);
            }
        }
        
        // Step 3: Process the queue using BFS
        int count = 0;
        while (!queue.isEmpty()) {
            int current = queue.poll();
            count++; // Successfully took this course
            
            // Decrease the in-degree of all dependent courses
            for (int nextCourse : adj.get(current)) {
                inDegree[nextCourse]--;
                // If a course now has 0 remaining prerequisites, add it to the queue
                if (inDegree[nextCourse] == 0) {
                    queue.offer(nextCourse);
                }
            }
        }
        
        // Step 4: If we processed all courses, there was no cycle
        return count == numCourses;
    }
}
```