# An Introduction to Markov Decision Processes (MDPs) and the Bellman Equation (中文版在后面^_^)

This document provides a detailed introduction to Markov Decision Processes (MDPs) and the Bellman Equation, fundamental concepts in reinforcement learning.


---

## Markov Decision Processes (MDPs)

A Markov Decision Process is a mathematical framework for modeling decision-making in situations where outcomes are partly random and partly under the control of a decision-maker.

An MDP is formally defined as a tuple $(S, A, P, R, \gamma)$:

* **$S$**: A finite set of states.
* **$A$**: A finite set of actions.
* **$P$**: The state transition probability function, $P(s'|s, a) = \Pr(S_{t+1}=s' | S_t=s, A_t=a)$.
* **$R$**: The reward function, $R(s, a, s')$.
* **$\gamma$**: The discount factor, $\gamma \in [0, 1]$.

### Key Concepts

* **Policy ($\pi$)**: A function that specifies the action to be taken in each state. It can be deterministic, $\pi(s) = a$, or stochastic, $\pi(a|s) = \Pr(A_t=a | S_t=s)$.
* **Return ($G_t$)**: The total discounted future reward from time step $t$: $G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \dots = \sum_{k=0}^{\infty} \gamma^k R_{t+k+1}$.
* **State-Value Function $v_{\pi}(s)$**: The expected return starting from state $s$ and following policy $\pi$.
    $v_{\pi}(s) = \mathbb{E}_{\pi}[G_t | S_t=s]$.
* **Action-Value Function $q_{\pi}(s, a)$**: The expected return starting from state $s$, taking action $a$, and then following policy $\pi$.
    $q_{\pi}(s, a) = \mathbb{E}_{\pi}[G_t | S_t=s, A_t=a]$.

---

## The Bellman Equation

The Bellman Equation decomposes the value function into two parts: the immediate reward plus the discounted future values.

### Bellman Expectation Equation

The Bellman Expectation Equation relates the value of a state (or state-action pair) to the values of its successor states.

For the state-value function $v_{\pi}(s)$:

$$v_{\pi}(s) = \sum_{a \in A} \pi(a|s) \sum_{s' \in S} P(s'|s, a) [R(s, a, s') + \gamma v_{\pi}(s')]$$

For the action-value function $q_{\pi}(s, a)$:

$$q_{\pi}(s, a) = \sum_{s' \in S} P(s'|s, a) [R(s, a, s') + \gamma \sum_{a' \in A} \pi(a'|s') q_{\pi}(s', a')]$$

### Bellman Optimality Equation

The goal of an MDP is to find the optimal policy $\pi_*$ that maximizes the expected return. The Bellman Optimality Equation provides a condition that the optimal value functions must satisfy.

The optimal state-value function $v_*(s)$ is:

$v_*(s) = \max_{a \in A} \sum_{s' \in S} P(s'|s, a) [R(s, a, s') + \gamma v_*(s')]$

The optimal action-value function $q_*(s, a)$ is:

$q_*(s, a) = \sum_{s' \in S} P(s'|s, a) [R(s, a, s') + \gamma \max_{a' \in A} q_*(s', a')]$

The Bellman equations are the foundation for many reinforcement learning algorithms, such as Value Iteration and Policy Iteration, which are used to find optimal policies for MDPs.

---
---

# 马尔可夫决策过程 (MDP) 与贝尔曼方程简介

本文档详细介绍强化学习中的基本概念——马尔可夫决策过程 (MDP) 和贝尔曼方程。

---

## 马尔可夫决策过程 (MDPs)

马尔可夫决策过程是一个数学框架，用于在结果部分随机、部分由决策者控制的情况下对决策进行建模。

一个 MDP 由一个元组 $(S, A, P, R, \gamma)$ 正式定义：

* **$S$**: 一个有限的状态集合。
* **$A$**: 一个有限的动作集合。
* **$P$**: 状态转移概率函数, $P(s'|s, a) = \Pr(S_{t+1}=s' | S_t=s, A_t=a)$。
* **$R$**: 奖励函数, $R(s, a, s')$。
* **$\gamma$**: 折扣因子, $\gamma \in [0, 1]$。

### 核心概念

* **策略 ($\pi$)**: 一个函数，指定在每个状态下要采取的动作。可以是确定性的 $\pi(s) = a$，也可以是随机的 $\pi(a|s) = \Pr(A_t=a | S_t=s)$。
* **回报 ($G_t$)**: 从时间步 $t$ 开始的总折扣未来奖励：$t$: $G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \dots = \sum_{k=0}^{\infty} \gamma^k R_{t+k+1}$.
* **状态值函数 $v_{\pi}(s)$**: 从状态 $s$ 开始，并遵循策略 $\pi$ 的预期回报。
    $v_{\pi}(s) = \mathbb{E}_{\pi}[G_t | S_t=s]$。
* **动作值函数 $q_{\pi}(s, a)$**: 从状态 $s$ 开始，采取动作 $a$，然后遵循策略 $\pi$ 的预期回报。
    $q_{\pi}(s, a) = \mathbb{E}_{\pi}[G_t | S_t=s, A_t=a]$。

---

## 贝尔曼方程

贝尔曼方程将价值函数分解为两部分：即时奖励加上折扣后的未来价值。

### 贝尔曼期望方程

贝尔曼期望方程将一个状态（或状态-动作对）的价值与其后继状态的价值联系起来。

对于状态值函数 $v_{\pi}(s)$:

$$v_{\pi}(s) = \sum_{a \in A} \pi(a|s) \sum_{s' \in S} P(s'|s, a) [R(s, a, s') + \gamma v_{\pi}(s')]$$

对于动作值函数 $q_{\pi}(s, a)$:

$$q_{\pi}(s, a) = \sum_{s' \in S} P(s'|s, a) [R(s, a, s') + \gamma \sum_{a' \in A} \pi(a'|s') q_{\pi}(s', a')]$$

### 贝尔曼最优方程

MDP 的目标是找到能够最大化预期回报的最优策略 $\pi_*$。贝尔曼最优方程为最优价值函数必须满足的条件。

最优状态值函数 $v_*(s)$ 为：

$$v_*(s) = \max_{a \in A} \sum_{s' \in S} P(s'|s, a) [R(s, a, s') + \gamma v_*(s')]$$

最优动作值函数 $q_*(s, a)$ 为：

$$q_*(s, a) = \sum_{s' \in S} P(s'|s, a) [R(s, a, s') + \gamma \max_{a' \in A} q_*(s', a')]$$

贝尔曼方程是许多强化学习算法的基础，例如价值迭代和策略迭代，这些算法用于为 MDP 找到最优策略。