# 策略迭代 (Policy Iteration)

---

## 核心思想 (Core Idea)

  策略迭代是另一种在已知**马尔可夫决策过程 (MDP)** 模型的情况下，求解最优策略的经典算法  。与价值迭代直接计算最优价值函数不同，策略迭代在一个循环中交替进行两个核心步骤：**策略评估 (Policy Evaluation)** 和 **策略改进 (Policy Improvement)** 。它从一个任意的初始策略开始，不断地评估并改进这个策略，直到策略不再发生变化，从而找到最优策略。

  Policy Iteration is another classic algorithm for finding an optimal policy when the model of the **Markov Decision Process (MDP)** is known.   Unlike Value Iteration, which directly computes the optimal value function, Policy Iteration alternates between two core steps in a loop: **Policy Evaluation** and **Policy Improvement**. It starts with an arbitrary initial policy and iteratively evaluates and improves it until the policy no longer changes, thus finding the optimal policy.

---

## 算法步骤 (Algorithm Steps)

  策略迭代主要包含以下两个交替进行的步骤 ：

  Policy Iteration mainly consists of the following two alternating steps:

### 1. 策略评估 (Policy Evaluation)

在这一步，我们固定当前的策略 $\pi_t$，然后计算在该策略下每个状态的价值函数 $V^{\pi_t}(s)$。这相当于解一个线性方程组，其方程是针对当前策略 $\pi_t$ 的贝尔曼方程。

In this step, we fix the current policy $\pi_t$ and then compute the value function $V^{\pi_t}(s)$ for each state under this policy. This is equivalent to solving a system of linear equations, where the equations are the Bellman equations for the current policy $\pi_t$.

$$V^{\pi_t}(s) = \sum_{s'} T(s, \pi_t(s), s') [R(s, \pi_t(s), s') + \gamma V^{\pi_t}(s')]$$

* 这里的 $\pi_t(s)$ 是当前策略在状态 $s$ 下指定的行为。
* Here, $\pi_t(s)$ is the action specified by the current policy in state $s$.

实际上，这个方程组可以通过迭代的方式求解，类似于价值迭代，但关键区别是这里的行为是固定的（由策略 $\pi_t$ 决定），而不是取最大值。

In practice, this system of equations can be solved iteratively, similar to value iteration, but the key difference is that the action is fixed (determined by policy $\pi_t$) instead of taking the maximum over all actions.

### 2. 策略改进 (Policy Improvement)

在计算出当前策略 $\pi_t$ 对应的价值函数 $V^{\pi_t}$ 后，我们进入策略改进阶段。对于每个状态 $s$，我们检查是否存在比当前策略 $\pi_t(s)$ 更好的行为。我们通过“向前看”一步来贪婪地更新策略：

After calculating the value function $V^{\pi_t}$ corresponding to the current policy $\pi_t$, we enter the policy improvement phase. For each state $s$, we check if there is a better action than the one specified by the current policy $\pi_t(s)$. We greedily update the policy by looking one step ahead:

$$\pi_{t+1}(s) \leftarrow \arg\max_a \sum_{s'} T(s, a, s') [R(s, a, s') + \gamma V^{\pi_t}(s')]$$

* 如果新的策略 $\pi_{t+1}$ 与旧的策略 $\pi_t$ 完全相同，那么算法收敛，我们已经找到了最优策略 $\pi^*$ 和最优价值函数 $V^*$。
* If the new policy $\pi_{t+1}$ is identical to the old policy $\pi_t$, then the algorithm has converged, and we have found the optimal policy $\pi^*$ and the optimal value function $V^*$.
* 否则，我们将 $\pi_{t+1}$ 作为新的当前策略，并返回到步骤1（策略评估）。
* Otherwise, we set $\pi_{t+1}$ as the new current policy and go back to step 1 (Policy Evaluation).

---

## 与价值迭代的比较 (Comparison with Value Iteration)

* **价值迭代**: 在每次主循环中，只对价值函数进行一次更新，然后隐式地调整策略。它将评估和改进合并在一个更新规则中。
* **Value Iteration**: In each main loop, it performs only one update on the value function and then implicitly adjusts the policy. It combines evaluation and improvement into a single update rule.

* **策略迭代**: 在每次主循环中，它会完整地进行策略评估（可能需要多次迭代才能收敛得到 $V^{\pi_t}$），然后再进行一次完整的策略改进。
* **Policy Iteration**: In each main loop, it performs a full policy evaluation (which may require multiple iterations to converge to $V^{\pi_t}$), followed by a complete policy improvement.

---

## 优缺点分析 (Strengths and Weaknesses)

#### 优点 (Strengths) 👍

* **收敛速度快 (Fast Convergence)**: 尽管每次迭代的计算量可能很大（尤其是在策略评估阶段），但策略迭代通常只需要很少的迭代次数就能收敛。对于许多问题，它比价值迭代收敛得更快。
* **Fast Convergence**: Although each iteration can be computationally intensive (especially the policy evaluation step), policy iteration often converges in a very small number of iterations. For many problems, it converges faster than value iteration.

#### 缺点 (Weaknesses) 👎

* **单次迭代成本高 (High Cost per Iteration)**: 策略评估步骤需要求解一个完整的线性方程组或进行多次迭代扫描，这可能非常耗时，特别是当状态空间很大时。
* **High Cost per Iteration**: The policy evaluation step requires solving a full system of linear equations or performing multiple iterative sweeps, which can be very time-consuming, especially when the state space is large.
* **依赖模型 (Requires a Model)**: 和价值迭代一样，策略迭代也需要知道环境的完整模型（转移概率和奖励函数）。
* **Requires a Model**: Like value iteration, policy iteration also requires complete knowledge of the environment's model (transition probabilities and reward function).