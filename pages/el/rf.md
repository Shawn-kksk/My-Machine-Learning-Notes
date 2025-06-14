# 随机森林
一种基于决策树的、利用集成学习思想的机器学习算法，用于分类和回归任务。

## 核心思想

1. 构建多棵决策树： 它不是只使用一棵决策树，而是创建大量的决策树。
2. 引入随机性： 在构建每棵树时，引入了两种随机性：
   - 数据随机性（Bagging/自助采样）： 每棵树都从原始数据集中有放回地随机抽样一部分数据进行训练。
   - 特征随机性： 在每棵树的每个分裂节点，只随机选择一部分特征来寻找最佳分裂点。
3. 聚合预测：
    - 分类： 所有树的预测结果进行多数投票来决定最终分类。
    - 回归： 所有树的预测结果进行平均来得到最终回归值。

通过这种“少数服从多数”或“取平均”的策略，随机森林能够：
1. 显著提高模型的准确性。
2. 降低过拟合的风险，提高模型的泛化能力。
3. 对噪声和异常值具有更强的鲁棒性。

## 构建过程
1. 数据准备：
   -  收集和清洗训练数据。处理缺失值。
   - 将数据分为特征（X）和目标变量（y）。

2. 设置超参数：
   - n (树的数量): 森林中决策树的数量。通常，数量越多，模型的性能越好，但计算成本也越高。
   - max_features (每次分裂考虑的特征数量): 在每个分裂节点处，随机选择的特征子集的数量。对于分类问题，通常取N的平方根(N 为总特征数)；对于回归问题，通常取 N/3。也可以是一个固定的整数或一个百分比。
   - max_depth (每棵树的最大深度): 限制每棵决策树的生长深度，防止过拟合。
   - min_samples_split (分裂所需的最小样本数): 节点在分裂前所需的最小样本数。
   - min_samples_leaf (叶子节点所需的最小样本数): 叶子节点所需的最小样本数。
   - random_state (随机种子): 用于控制随机过程，确保结果可复现。

3. 构建单棵决策树（Bagging + 随机特征选择）
重复以下过程 n 次，以构建n棵独立的决策树：
   - 自助采样 (Bootstrap Sampling)： 从原始训练数据集中有放回地随机抽取一个与原始数据集大小相同的子集作为当前决策树的训练数据。这意味着一些样本可能被多次抽取，一些样本可能从未被抽取。
   - 决策树构建：从根节点开始，在每个节点分裂时，随机选择 max_features 个特征，从这 max_features 个特征中，选择最佳的特征和分裂点来划分数据（例如，使用信息增益、基尼不纯度等标准）。递归地重复这个过程，直到达到预设的停止条件（如 max_depth、min_samples_split、min_samples_leaf 等）。

4. 组合预测结果
当需要对新的未知数据进行预测时，将新数据输入到森林中的每一棵决策树，每棵决策树都会给出一个预测结果。
   - 对于分类问题： 统计所有树的预测结果，选择出现次数最多的类别作为最终预测结果（多数投票）。
   - 对于回归问题： 计算所有树的预测结果的平均值作为最终预测结果。

# Random Forest
A machine learning algorithm based on decision trees that leverages the idea of ensemble learning for classification and regression tasks.

## Core Idea

1. Building Multiple Decision Trees: Instead of using just one decision tree, it creates a large number of them.
2. Introducing Randomness: Two types of randomness are introduced when building each tree:
   - Data Randomness (Bagging/Bootstrap Sampling): Each tree is trained on a random sample of data drawn with replacement from the original dataset.
   - Feature Randomness: At each split node in every tree, only a random subset of features is considered to find the best split point.
3. Aggregating Predictions:
   - Classification: The predictions from all trees are combined through a majority vote to determine the final classification.
   - Regression: The predictions from all trees are averaged to get the final regression value.

Through this "majority-wins" or "averaging" strategy, Random Forests can:

- Significantly improve model accuracy.
- Reduce the risk of overfitting and enhance the model's generalization ability.
- Exhibit stronger robustness to noise and outliers.

## Construction Process
1.  **Data Preparation:**
    * Collect and clean training data. Handle missing values.
    * Divide the data into features (X) and the target variable (y).

2.  **Setting Hyperparameters:**
    * **n (Number of Trees):** The number of decision trees in the forest. Generally, more trees lead to better model performance, but at a higher computational cost.
    * **max_features (Number of Features to Consider for Each Split):** The number of randomly selected feature subsets at each split node. For classification problems, it's typically $\sqrt{N}$ (where N is the total number of features); for regression problems, it's usually $N/3$. It can also be a fixed integer or a percentage.
    * **max_depth (Maximum Depth of Each Tree):** Limits the growth depth of each decision tree to prevent overfitting.
    * **min_samples_split (Minimum Samples Required to Split):** The minimum number of samples a node must have before it can be split.
    * **min_samples_leaf (Minimum Samples Required in a Leaf Node):** The minimum number of samples required to be at a leaf node.
    * **random_state (Random Seed):** Used to control the random processes, ensuring reproducible results.

3.  **Building Individual Decision Trees (Bagging + Random Feature Selection):**
    Repeat the following process 'n' times to build 'n' independent decision trees:
    * **Bootstrap Sampling:** Randomly draw a subset of the same size as the original dataset, with replacement, from the original training dataset to serve as the training data for the current decision tree. This means some samples may be drawn multiple times, and some may never be drawn.
    * **Decision Tree Construction:** Start from the root node. At each node split, randomly select `max_features` features. From these `max_features` features, choose the best feature and split point to divide the data (e.g., using information gain, Gini impurity, etc., as criteria). Recursively repeat this process until the preset stopping conditions are met (such as `max_depth`, `min_samples_split`, `min_samples_leaf`, etc.).

4.  **Combining Prediction Results:**
    When predicting new, unseen data, input the new data into each decision tree in the forest. Each decision tree will provide a prediction.
    * **For Classification Problems:** Tally the predictions from all trees and select the class that appears most frequently as the final prediction (majority vote).
    * **For Regression Problems:** Calculate the average of the predictions from all trees as the final prediction.
