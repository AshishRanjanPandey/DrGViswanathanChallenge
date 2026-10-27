# 🎯 LeetCode 133: Clone Graph

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Given a reference of a node in a **connected undirected graph**.

Return a **deep copy** (clone) of the graph.

Each node in the graph contains a value (`int`) and a list (`List[Node]`) of its neighbors.

```java
class Node {
    public int val;
    public List<Node> neighbors;
}
```

**Test case format:**
For simplicity, each node's value is the same as the node's index (1-indexed). For example, the first node with `val == 1`, the second node with `val == 2`, and so on. The graph is represented in the test case using an adjacency list.

An adjacency list is a collection of unordered lists used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.

The given node will always be the first node with `val = 1`. You must return the **copy of the given node** as a reference to the cloned graph.

#### Examples

- **Example 1**:
  - **Input**: `adjList = [[2,4],[1,3],[2,4],[1,3]]`
  - **Output**: `[[2,4],[1,3],[2,4],[1,3]]`
  - **Explanation**: There are 4 nodes in the graph.
    - 1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
    - 2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
    - 3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
    - 4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).

- **Example 2**:
  - **Input**: `adjList = [[]]`
  - **Output**: `[[]]`
  - **Explanation**: Note that the input contains one empty list. The graph consists of only one node with `val = 1` and it does not have any neighbors.

- **Example 3**:
  - **Input**: `adjList = []`
  - **Output**: `[]`
  - **Explanation**: This an empty graph, it does not have any nodes.

#### Constraints

- $0 \le \text{Number of nodes} \le 100$
- $1 \le \text{Node.val} \le 100$
- `Node.val` is unique for each node.
- There are no repeated edges and no self-loops in the graph.
- The Graph is connected and all nodes can be visited starting from the given node.

---

### 💡 Intuition & Strategy

1. **Graph Traversal (Depth-First Search)**:
   - To copy a graph, we need to visit every node and every edge. A standard Depth-First Search (DFS) or Breadth-First Search (BFS) is perfect for this. DFS is used here for its clean, recursive structure.

2. **Tracking Visited Nodes**:
   - Because the graph is undirected, edges go both ways (e.g., $1 \leftrightarrow 2$). If we aren't careful, our traversal could get caught in an infinite loop, continuously bouncing between nodes.
   - We solve this using a Hash Map (`visited`) that maps the **original node** to its **cloned counterpart**.

3. **Deep Copying the Graph**:
   - When visiting a node, we first check if it already exists in our `visited` map. If it does, we immediately return the cloned node to establish the connection without redundantly traversing it again.
   - If it doesn't exist, we create a new cloned node, add it to the `visited` map *immediately* (crucial to do this before recursively exploring neighbors to prevent cycles), and then recursively clone all of its neighbors.

4. **Complexity**:
   - **Time Complexity**: $\mathcal{O}(V + E)$
     - Where $V$ is the number of vertices (nodes) and $E$ is the number of edges. We visit every node exactly once and iterate through every edge exactly once.
   - **Space Complexity**: $\mathcal{O}(V)$
     - The Hash Map requires $\mathcal{O}(V)$ space to store the cloned nodes. The recursive call stack also takes $\mathcal{O}(V)$ space in the worst-case scenario (e.g., a completely linear graph).

---

### 💻 Java Solution

```java
/**
 * Problem: Clone Graph (LeetCode 133)
 * Language: Java 17
 * Time Complexity: O(V + E)
 * Space Complexity: O(V)
 * Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.HashMap;
import java.util.ArrayList;
import java.util.List;

/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/

class Solution {
    // HashMap to store the visited nodes and their respective clones
    private HashMap<Node, Node> visited = new HashMap<>();
    
    public Node cloneGraph(Node node) {
        // Edge case: Empty graph
        if (node == null) {
            return null;
        }
        
        // If the node was already visited, return its clone from the map
        if (visited.containsKey(node)) {
            return visited.get(node);
        }
        
        // Create a clone for the current node
        // Note: We don't have the cloned neighbors yet, so initialize an empty list
        Node cloneNode = new Node(node.val, new ArrayList<>());
        
        // Add the cloned node to the map BEFORE iterating through its neighbors
        // This is crucial for avoiding infinite recursion in cyclic graphs
        visited.put(node, cloneNode);
        
        // Iterate through the neighbors and recursively clone them
        for (Node neighbor : node.neighbors) {
            cloneNode.neighbors.add(cloneGraph(neighbor));
        }
        
        return cloneNode;
    }
}
```