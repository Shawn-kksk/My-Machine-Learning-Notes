## 迭代宽度 (IW) 算法详解

### 中文 (Chinese)

 迭代宽度（Iterated Width，简称 IW）算法是一种基于“新颖性”概念的广度优先搜索（Breadth-First Search, BFS）变体，用于解决人工智能规划问题 。

**核心思想：**
 IW 算法的核心思想是平衡搜索中的“探索”和“利用”，通过引入“新颖性”这个度量来指导搜索 。它不是盲目地扩展所有可能的路径，而是有选择地只保留那些能够带来新颖性的状态。

 **新颖性 (Novelty) 的定义 ：**
 “新颖性”是 IW 算法的关键概念，它衡量一个状态有多“新”  。一个状态 $s$ 的新颖性 $w(s)$ 定义为，在搜索过程中，该状态 $s$ 中首次出现为真的原子（可以理解为问题的基本事实或属性）的最小子集的集合的大小 。
* 例如：
    *  如果状态 $s$ 中只有一个原子 $p$ 是首次出现为真的，那么 $w(s)=1$ 。
    *  如果状态 $s$ 中有两个不同的原子 $p, q$ 是首次出现为真的（并且没有单个原子是首次出现为真的），那么 $w(s)=2$ 。
    *  以此类推，如果有三个不同的原子是首次出现为真的，则 $w(s)=3$ 。

 **IW 算法的运作方式 ：**
 IW 算法通过一系列的广度优先搜索调用来逐步解决问题，每次调用的搜索深度限制基于新颖性阈值 $k$ 。具体步骤如下：
1.   **迭代增加 $k$ 值：** IW 算法会从 $k=0, 1, 2, ...$ 开始，顺序调用 $IW(k)$ 函数 。
2.   **广度优先搜索与剪枝：** 每个 $IW(k)$ 函数执行一次广度优先搜索。在搜索过程中，每当生成一个新的状态 $s'$ 时，算法会计算其新颖性 $w(s')$ 。
3.   **基于新颖性的剪枝：** 如果新生成状态 $s'$ 的新颖性 $w(s')$ 大于当前的 $k$ 值，那么这个状态 $s'$ 就会被剪枝，即不会被添加到搜索队列中或进一步扩展 。
4.   **停止条件：** 这个迭代过程会一直进行，直到问题被解决（找到目标状态）或者 $k$ 值超过问题中变量的数量 。

**IW 算法的性质与性能：**
*  **状态扩展数量：** $IW(k)$ 在最坏情况下会扩展 $O(n^k)$ 个状态，其中 $n$ 是原子（或变量）的数量 。这意味着如果问题的“宽度”很小，那么算法的效率会很高。
*  **完备性和最优性：** 对于“宽度”为 $k$ 的规划问题 $\Pi$， $IW(k)$ 算法能够在 $O(n^k)$ 时间内解决 $\Pi$  。如果问题的成本函数是均匀的（即每一步的成本都相同），那么 $IW(k)$ 能够找到最优解  。此外，$IW(k)$ 对此类问题是完备的，即如果存在解，它最终能够找到 。
*  **实际应用效果：** 尽管 IW 算法简单且不使用启发式信息，但它在处理目标是单个原子（单一目标）的基准规划问题时表现出色 。实验结果显示，IW 在 IPC（国际规划竞赛）测试的 37921 个单目标实例中，解决了 91% 的问题，与 $GBFS+h_{add}$（一种常用的启发式规划器）的覆盖率相当 。
*  **低宽度特性：** 许多经典的规划领域，如积木世界 (Blocks)、物流 (Logistics)、抓手 (Gripper) 和 N-谜题 (n-puzzle)，当目标被限制为单个原子时，它们的宽度值都很小，甚至与问题规模无关  。这解释了 IW 在这些领域表现优异的原因  。实际上，对于 IPC 问题中的单目标实例，$k=1$ 和 $k=2$ 的情况合计解决了 88.3% 的问题 。

**总结：**
 IW 算法通过利用状态的“新颖性”来智能地剪枝搜索空间，从而在许多低宽度（特别是单目标）的规划问题上实现了高效且强大的性能  。它展示了即使是简单的盲搜索过程，如果能有效利用问题的结构信息，也能取得显著的成果 。

### English

## Detailed Explanation of Iterated Width (IW) Algorithm

 The Iterated Width (IW) algorithm is a variant of Breadth-First Search (BFS) based on the concept of "novelty," used for solving Artificial Intelligence (AI) planning problems.

**Core Idea:**
 The core idea of the IW algorithm is to balance "exploration" and "exploitation" in search by introducing "novelty" as a measure to guide the search. Instead of blindly expanding all possible paths, it selectively retains only those states that bring about novelty.

 **Definition of Novelty:**
 "Novelty" is a key concept of the IW algorithm, which measures how "new" a state is.  The novelty $w(s)$ of a state $s$ is defined as the size of the smallest subset of atoms in $s$ that are true for the first time in the search.
* For example:
    *  If there is one atom $p \in s$ such that $s$ is the first state that makes $p$ true, then $w(s)=1$.
    *  If there are two different atoms $p, q \in s$ such that $s$ is the first state that makes $p \wedge q$ true (and no single atom is true for the first time), then $w(s)=2$.
    *  Otherwise, if there are three different atoms, $w(s)=3$, and so on.

 **How the IW Algorithm Works:**
 The IW algorithm solves problems iteratively through a sequence of calls to a breadth-first search, where each call's search depth is limited based on a novelty threshold $k$. The specific steps are as follows:
1.   **Iterative Increase of $k$ Value:** The IW algorithm performs a sequence of calls $IW(k)$ for $k=0, 1, 2, ...$.
2.  **Breadth-First Search with Pruning:** Each $IW(k)$ function executes a breadth-first search.  During the search, whenever a newly generated state $s'$ is encountered, the algorithm calculates its novelty $w(s')$.
3.   **Novelty-Based Pruning:** If the novelty $w(s')$ of the newly generated state $s'$ is greater than the current $k$ value, then $s'$ is pruned, meaning it will not be added to the search queue or expanded further.
4.   **Stopping Condition:** This iterative process continues until the problem is solved (a goal state is found) or $k$ exceeds the number of variables in the problem.

**Properties and Performance of the IW Algorithm:**
*  **Number of States Expanded:** $IW(k)$ expands at most $O(n^k)$ states in the worst case, where $n$ is the number of atoms (or variables). This implies high efficiency if the problem's "width" is small.
*  **Completeness and Optimality:** For problems $\Pi \in \mathcal{P}$ where width$(\Pi)=k$, $IW(k)$ solves $\Pi$ in time $O(n^k)$.  Furthermore, $IW(k)$ solves $\Pi$ optimally for problems with uniform cost functions , and $IW(k)$ is complete for $\Pi$.
*  **Practical Application Results:** Despite its simplicity and "blindness" (lack of heuristic guidance), IW is a remarkably good algorithm over benchmarks when goals are restricted to single atoms.  Experimental results show that IW solved 91% of 37921 instances with single goals, which is comparable to $GBFS+h_{add}$ (a commonly used heuristic planner).
*  **Low Width Characteristic:** Domains like Blocks, Logistics, Gripper, and n-puzzle have a bounded width independent of problem size and initial situation, provided that goals are single atoms.  This explains why IW performs so well in these domains.  In practice, for IPC problems with single goals, $IW(k \le 2)$ solved 88.3% of the instances.

**Conclusion:**
 The IW algorithm effectively prunes the search space by leveraging the "novelty" of states, leading to efficient and powerful performance on many low-width (especially single-goal) planning problems.  It demonstrates that even simple blind search procedures can achieve significant results if they effectively utilize the structural information of the problem.