Greedy Best-First Search (GBFS) is an informed search algorithm used in artificial intelligence and pathfinding. Its core idea is to always expand the node that appears to be **closest to the goal**, based solely on a heuristic estimate. It's called "greedy" because at each step, it makes the choice that seems best at that moment, without considering the long-term consequences or the actual path cost from the start.

---

## How it Works

GBFS operates with the following key components:

1.  **Heuristic Function ($h(n)$):** This is the **only** evaluation function used by Greedy Best-First Search. For any given node 'n', $h(n)$ estimates the cost or distance from 'n' to the goal node. The algorithm always chooses to explore the node with the *lowest* $h(n)$ value from its list of unexpanded (open) nodes.
    * **Examples of Heuristics:**
        * **Euclidean Distance (straight-line distance):** In a grid or map, the straight-line distance from the current node to the goal.
        * **Manhattan Distance:** In a grid, the sum of the absolute differences of the x and y coordinates between the current node and the goal (useful for movement restricted to horizontal/vertical).

2.  **Open List (Priority Queue):** Similar to A*, GBFS maintains an open list (often implemented as a priority queue) to store nodes that have been discovered but not yet expanded. The nodes in this list are prioritized based on their $h(n)$ values, with the node having the lowest $h(n)$ at the top.

3.  **Closed List:** This list keeps track of nodes that have already been expanded, preventing the algorithm from revisiting them.

---

## The Algorithm Steps

1.  **Initialization:**
    * Add the starting node to the open list.
    * Calculate its $h(n)$ value.
    * The closed list is empty.

2.  **Main Loop:** Continue as long as the open list is not empty:
    * **Select Current Node:** Remove the node with the lowest $h(n)$ value from the open list. This becomes the "current node."
    * **Check for Goal:** If the current node is the goal node, the search is successful. Reconstruct the path by backtracking through parent pointers.
    * **Move to Closed List:** Add the current node to the closed list.
    * **Explore Neighbors:** For each neighbor of the current node:
        * **Skip if in Closed List:** If the neighbor is already in the closed list, ignore it.
        * **Add to Open List:** If the neighbor is not in the open list (or is already there but with a higher heuristic value, though typically in GBFS we just add if not present and update if a better path isn't a concern):
            * Calculate its $h(n)$ value.
            * Set the current node as its "parent."
            * Add the neighbor to the open list.

3.  **No Path:** If the open list becomes empty and the goal hasn't been reached, it means there's no path to the goal.

---

## Key Characteristics

* **Evaluation Function:** $f(n) = h(n)$ (only the heuristic is considered).
* **Speed:** It's generally very fast because it greedily pushes towards the goal, often exploring fewer nodes than uninformed search methods like Breadth-First Search (BFS) or Depth-First Search (DFS), or even Dijkstra's.
* **Optimality:** **Not guaranteed to find the optimal (shortest) path.** Because it only looks at the estimated cost to the goal and ignores the actual cost incurred so far ($g(n)$), it can sometimes take a "locally optimal" step that leads to a longer overall path or even a dead end. It might get "stuck" in a local minimum if the heuristic misleads it.
* **Completeness:** **Not guaranteed to be complete.** It might get stuck in cycles or explore paths that never lead to the goal if the heuristic is misleading and there are no mechanisms to detect and break cycles.
* **Memory Usage:** Can be more memory-efficient than A* or Dijkstra's in some cases, as it might not need to store as many nodes if the heuristic is very good at guiding the search.

---

## Advantages

* **Simple to implement.**
* **Fast** in many scenarios, especially when the heuristic is accurate and the goal is easily reachable.
* **Low memory requirements** compared to algorithms that need to store all explored paths (like Dijkstra's or A* on large graphs).

---

## Disadvantages

* **Does not guarantee optimality.**
* **May not be complete** (can get stuck in loops or dead ends).
* **Heavily dependent on the quality of the heuristic.** A poor heuristic can lead to inefficient searches or non-optimal results.

---

## Applications

* **Game AI:** Often used in simpler games for quick, though not always perfectly optimal, pathfinding for NPCs.
* **Robotics:** For quick navigation decisions where sub-optimality is acceptable.
* **Optimization problems:** As a heuristic for quickly finding "good enough" solutions.

In summary, Greedy Best-First Search is like someone trying to find their way to a destination by always taking the street that *looks* like it's pointing most directly towards their target, without bothering to check traffic or actual distance traveled so far. It's fast and intuitive, but there's a risk of taking a scenic (or dead-end) route!