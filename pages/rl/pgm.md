# 策略梯度方法 (Policy Gradient Methods)

---

## 核心思想 (Core Idea)

策略梯度方法是强化学习中的一类重要算法，它与基于价值的方法（如Q-learning）有根本性的不同。其核心思想是**直接对策略本身进行参数化，然后通过梯度上升来优化这个策略，从而最大化期望的总回报**。它不去学习状态的价值，而是直接学习一个能够做出最优决策的策略函数 $\pi_\theta(a|s)$。

Policy Gradient methods are a major class of algorithms in reinforcement learning, fundamentally different from value-based methods (like Q-learning). The core idea is to **directly parameterize the policy itself and then optimize this policy via gradient ascent to maximize the expected total return**. Instead of learning state values, it directly learns a policy function $\pi_\theta(a|s)$ that makes optimal decisions.

---

## 算法工作原理 (Algorithm Working Principle)

1.  **参数化策略 (Parameterize the Policy)**:
    用一个带有参数 $\theta$ 的函数（通常是神经网络）来表示策略，记为 $\pi_\theta(a|s)$。
    A function with parameters $\theta$ (usually a neural network) is used to represent the policy, denoted as $\pi_\theta(a|s)$.

2.  **定义目标函数 (Define the Objective Function)**:
    定义一个目标函数 $J(\theta)$ 来衡量当前策略的好坏，通常是所有可能轨迹的期望总回报。
    An objective function $J(\theta)$ is defined to measure the quality of the current policy, typically the expected total return over all possible trajectories.

3.  **计算梯度 (Compute the Gradient)**:
    根据**策略梯度定理 (Policy Gradient Theorem)**，目标函数 $J(\theta)$ 相对于参数 $\theta$ 的梯度 $\nabla_\theta J(\theta)$ 可以被计算出来。其核心直觉是增加那些能够带来高回报的行为被选中的概率。
    According to the **Policy Gradient Theorem**, the gradient of the objective function $J(\theta)$ with respect to the parameters $\theta$, denoted as $\nabla_\theta J(\theta)$, can be computed. The core intuition is to increase the probability of selecting actions that lead to high returns.
    $$\nabla_\theta J(\theta) \propto \sum_s d^{\pi}(s) \sum_a \nabla_\theta \pi_\theta(a|s) Q^{\pi}(s,a)$$

4.  **更新参数 (Update Parameters)**:
    使用梯度上升来更新策略的参数，其中 $\alpha$ 是学习率。
    Use gradient ascent to update the policy parameters, where $\alpha$ is the learning rate.
    $$\theta \leftarrow \theta + \alpha \nabla_\theta J(\theta)$$

---

## 典型算法 (Typical Algorithms)

根据如何估算回报，策略梯度方法有几种主要的实现：
Depending on how the return is estimated, there are several main implementations of policy gradient methods:

*    **REINFORCE (蒙特卡洛策略梯度)**: 这是最基础的策略梯度算法 。它使用一个完整的经验回合 (episode) 结束后得到的实际回报 $G_t$ 来作为回报的无偏估计。它的优点是简单且无偏，但缺点是方差很高，导致训练过程不稳定。
*    **REINFORCE (Monte Carlo Policy Gradient)**: This is the most fundamental policy gradient algorithm. It uses the actual return $G_t$ from a complete episode as an unbiased estimate of the return. Its advantage is simplicity and being unbiased, but its disadvantage is high variance, which leads to unstable training.

*    **Actor-Critic (演员-评论家)**: 这类方法结合了策略梯度和价值学习 。
    * **Actor (演员)**: 负责根据当前策略 $\pi_\theta(a|s)$ 选择行为。
    * **Critic (评论家)**: 负责学习一个价值函数，用于评估“演员”所选行为的好坏，并提供一个更低方差的梯度信号来指导“演员”的更新。相比REINFORCE，Actor-Critic方法通常方差更小，学习效率更高。
*    **Actor-Critic**: This class of methods combines policy gradients with value learning.
    * **Actor**: Responsible for selecting actions according to the current policy $\pi_\theta(a|s)$.
    * **Critic**: Responsible for learning a value function to evaluate the quality of the Actor's actions and provide a lower-variance gradient signal to guide the Actor's updates. Compared to REINFORCE, Actor-Critic methods generally have lower variance and higher learning efficiency.

---

## 与基于价值的方法对比 (Comparison with Value-Based Methods)

| 特性 (Feature) |    策略梯度方法 (Policy Gradient Methods)  |    基于价值的方法 (Value-Based Methods)  |
| :--- | :--- | :--- |
| **学习目标 (Learning Target)** | 直接学习策略函数 $\pi_\theta(a|s)$ (Directly learns the policy function $\pi_\theta(a|s)$) | 学习价值函数 $V(s)$ 或 $Q(s,a)$ (Learns the value function $V(s)$ or $Q(s,a)$) |
| **策略 (Policy)** | 策略是显式的，可以是随机策略 (Policy is explicit and can be stochastic) | 策略是隐式的，通常是确定性策略 (Policy is implicit and usually deterministic) |
| **行为空间 (Action Space)** | 能很自然地处理连续行为空间 (Naturally handles continuous action spaces) | 通常难以处理连续行为空间 (Generally difficult to handle continuous action spaces) |
| **收敛性 (Convergence)** | 梯度上升，容易陷入局部最优 (Gradient ascent, prone to local optima) | 动态规划，通常有更好的收敛保证 (Dynamic programming, often has better convergence guarantees) |