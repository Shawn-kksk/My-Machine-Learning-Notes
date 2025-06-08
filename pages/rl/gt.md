# 博弈论 (Game Theory)

---

博弈论是研究多个决策者（“玩家”）之间策略互动的数学理论。它分析在特定规则下，当每个玩家的选择都会影响其他玩家的结果时，玩家们如何做出最优决策。

Game Theory is a mathematical theory that studies the strategic interactions among multiple rational decision-makers ("players"). It analyzes how players make optimal decisions when their choices affect the outcomes of other players under a specific set of rules.

## 核心组成部分 (Core Components)

* **玩家 (Players)**：参与决策的实体。 (The entities involved in the decision-making.)
* **策略 (Strategies)**：每个玩家可以选择的完整行动方案。 (The complete plan of action a player can choose.)
* **收益 (Payoffs)**：当所有玩家都确定了各自的策略后，每个玩家获得的收益或损失。 (The rewards or losses each player receives after all players have chosen their strategies.)

---

##     博弈的表示方法 (Modeling Scenarios) 

####     1. 标准式博弈 (Normal Form Games) 

标准式博弈，也常被称为“收益矩阵”，它描述的是所有玩家**同时**做出决策的情景。 

Normal Form Games, also known as "payoff matrices," describe scenarios where all players make decisions **simultaneously**. 

**示例：囚徒困境 (Example: Prisoner's Dilemma)**
(A的收益, B的收益) / (Payoff for A, Payoff for B)

| | 囚犯B：沉默 / Prisoner B: Silent | 囚犯B：坦白 / Prisoner B: Confess |
| :--- | :---: | :---: |
| **囚犯A：沉默 / Prisoner A: Silent** | (-1, -1) | (-10, 0) |
| **囚犯A：坦白 / Prisoner A: Confess** | (0, -10) | (-8, -8) |

####     2. 扩展式博弈 (Extensive Form Games) 

扩展式博弈描述的是玩家**序贯**决策的情景，通常用一个“博弈树”来表示。      对于这种博弈，通常可以使用**逆向归纳法 (backward induction)** 来求解。 

Extensive Form Games describe scenarios where players make decisions **sequentially**, often represented by a "game tree."      These games can often be solved using **backward induction**. 

---

## 策略的类型 (Types of Strategies)

**纯策略 (Pure Strategy)**：指玩家在每个决策点上都选择一个确定的行动。 

**Pure Strategy**: A player chooses a specific, deterministic action at every decision point. 

**混合策略 (Mixed Strategy)**：指玩家以一定的概率分布来选择不同的纯策略。 

**Mixed Strategy**: A player chooses among different pure strategies according to a probability distribution. 

---

## 核心概念与求解 (Core Concepts and Solutions)

**最佳应对 (Best Response)**：给定其他所有玩家的策略选择，一个玩家能使自己收益最大化的策略。

**Best Response**: A strategy that maximizes a player's payoff, given the strategies chosen by all other players. 

**纳什均衡 (Nash Equilibrium)**：一个策略组合，在该组合中，没有任何一个玩家可以通过**单方面**改变自己的策略而获得更高的收益。 

**Nash Equilibrium**: A strategy profile where no single player can gain a higher payoff by **unilaterally** changing their own strategy. 

---

## 与强化学习的联系 (Connection to Reinforcement Learning)

####     随机博弈 (Stochastic Games) 

随机博弈可以看作是多智能体版本的马尔可夫决策过程（MDP）。      在一个随机博弈中，环境的状态转换取决于所有智能体的联合行为，并且每个智能体都有自己的奖励函数。 

Stochastic games can be seen as a multi-agent version of a Markov Decision Process (MDP).      In a stochastic game, the state transitions depend on the joint actions of all agents, and each agent has its own reward function. 

####     单智能体与多智能体强化学习 (Single- vs. Multi-Agent Reinforcement Learning) 

**单智能体RL** 通常在MDP框架下解决问题，目标是最大化单个智能体的累积奖励。 

**Single-agent RL** typically operates within the MDP framework, aiming to maximize the cumulative reward for a single agent. 

**多智能体RL (MARL)** 则是在随机博弈的框架下，需要考虑多个智能体之间的互动。 

**Multi-agent RL (MARL)** operates within the framework of stochastic games, needing to consider the interactions among multiple agents.