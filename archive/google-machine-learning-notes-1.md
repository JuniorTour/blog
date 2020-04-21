## 谷歌机器学习速成课程笔记 -（一）- 2018/10/08

https://developers.google.com/machine-learning/crash-course/prereqs-and-prework



## 一、简介

### 0. 前提条件和准备工作

- Python 基础
- 入门代数知识



TODO：

- Bash、Shell
- 数学：[对数](https://wikipedia.org/wiki/Logarithm)和对数方程式，例如 y=ln(1+e^z)、张量（https://www.tensorflow.org/programmers_guide/tensors）.....（计划随用随学）



## 二、机器学习概念

### 1. 机器学习简介 

机器学习的意义

1. 提高编程效率，缩短编程时间。
2. 自定义产品，增强产品的适应性。
3. 解决一些只有人才能快速解决的问题，例如认识一张面孔。
4. 改变思考问题的方式，从以逻辑和数学思维思考问题到从自然科学的角度观察`不确定的世界`。



### 2. 框架处理

#### 1. 监督式机器学习：机器学习系统通过学习如何组合输入信息来对从未见过的数据做出有用的预测

`标签（label）`：指我们要预测的真实事物：**y**，例如垃圾邮件过滤中的“垃圾邮件或非垃圾邮件”。

`特征（feature）`：指用于描述数据的输入变量：**x下标i**，例如邮件的内容、标题、收发件人。”。

`样本（example）`：指数据的特定实例：**x**，一份数据，例如一封邮件。

样本可以`有标签 - {特征, 标签}：(x, y)，用于训练模型`。

或`无标签 - {特征, 标签}：(x, y)，用于对新数据做出预测`。

在我们的垃圾邮件检测器示例中，有标签样本是用户明确标记为“垃圾邮件”或“非垃圾邮件”的各个电子邮件。

最终，通过这些数据得到一个`模型 modle`，定义了特征与标签之间的关系。

#### 2. 回归与分类

- **回归**模型可预测连续值。例如：加利福尼亚州一栋房产的价值是多少？用户点击此广告的概率是多少？
- **分类**模型可预测离散值。例如：某个指定电子邮件是垃圾邮件还是非垃圾邮件？这是一张狗、猫还是仓鼠图片？



#### 3. 练习题

- 主题标头中的字词适合做`标签`。❌		主题标头中的字词可能是优质`特征`，但不适合做标签。

- 用户喜欢的鞋子是一种实用标签。❌           喜好不是可观察且可量化的指标。我们能做到最好的就是针对用户的喜好来搜索可观察的代理指标。



### 3. 深入了解机器学习(Descending into ML)：从数据中学习模型

#### 1. 视频讲座

![ml-1](/Users/jianannan/Desktop/ml-notes/ml-1.png)

以课程中的`数据集`（上图）为例，x轴为房屋面积，y轴为**我们尝试预测的**房屋价格，一个个的点就是`用标签标记的样本`，图中的直线就是我们引导一个具有初中智力水平的机器得出的`模型`，可以将其定义为`y=wx + b`。

我们如何知道这个模型是否是正确的呢？用误差来判断。

`误差`可以从本质上反映这条线在预测任何给定样本时的效果如何。以图中的这些点为例，有些具有正误差、有些具有负误差、有些误差几乎就是零。

用更正式的方式定义即：给定样本的 **L2 损失**（**平方损失**）也称为`平方误差` =` 预测值和标签值之差的平方 = (观察值 - 预测值)^2 = (y - y')^2`。

并且，**我们在训练模型时，并非专注于尽量减少某一个样本的误差，而是着眼于最大限度的减少整个数据集的误差。**



#### 2. 线性回归

以根据一个数据集来预测蟋蟀鸣叫声与温度的关系为例：

1. 可以用代数知识写出这个关系：y = mx + b，

- y 指的是温度（以摄氏度表示），即我们试图预测的值。
- m 指的是直线的斜率。
- x 指的是每分钟的鸣叫声次数，即输入特征的值。
- b 指的是 y 轴截距。



2. 但是按照机器学习的惯例，模型的方程式应该是这样的：y ' = b + w1 x1，

- y '指的是预测[标签](https://developers.google.com/machine-learning/crash-course/framing/ml-terminology#labels)（理想输出值）。
- w1 指的是特征 1 的`权重（weight）`。权重与上文中用 m 表示的“斜率”的概念相同。
- b 指的是`偏差（bias）`（y 轴截距）。而在一些机器学习文档中，它称为 w0。
- x1 指的是[特征](https://developers.google.com/machine-learning/crash-course/framing/ml-terminology#features)（已知输入项）。

标（例如 w1 和 x1）预示着可以用多个特征来表示更复杂的模型。例如，具有三个特征的模型可以采用以下方程式：

> y′= b + w1x1 + w2x2 + w3x3



#### 3. 训练与损失

`训练模型`表示通过有标签样本来学习（确定）所有权重（w1，斜率）和偏差（b，截距）的理想值。

在监督式学习中，机器学习算法通过以下方式构建模型：检查多个样本并尝试找出可最大限度地减少损失的模型；这一过程称为**经验风险最小化**。

**损失**是一个数值，表示对于单个样本而言模型预测的准确程度。

训练模型的目标是从所有样本中找到一组平均损失“较小”的权重和偏差。例如下图，右侧的模型相对于左侧`损失`就更小。

![LossSideBySide](/Users/jianannan/Desktop/ml-notes/LossSideBySide.png)

我们用`平方损失（L2损失）`来更科学地汇总一个模型的损失。

1. 单个样本的`平方损失`公式如下：

```
  = the square of the difference between the label and the prediction
  = (observation - prediction(x))2
  = (y - y')2
```

2. 每个样本的平均平方损失称之为：**均方误差** (**MSE - Mean Squared Error**)，公式如下：

![mse-formula](/Users/jianannan/Desktop/ml-notes/mse-formula.png)

- (x, y) 指的是样本。
- Prediction(x) 指的是权重（w1，斜率）和偏差（）与特征集 x 结合的函数。
- D 指的是包含多个有标签样本（即 (x,y)）的数据集。
- N 指的是 D 中的样本数量。



这是出色的模型吗？您如何判断误差有多大？

> 由于均方误差 (MSE) 很难解读，因此我们经常查看的是均方根误差 (RMSE - Root Mean Squared Error ，即 MSE 开方)。
>
> ——[使用 TensorFlow 的起始步骤](https://colab.research.google.com/notebooks/mlcc/first_steps_with_tensor_flow.ipynb?utm_source=mlcc&utm_campaign=colab-external&utm_medium=referral&utm_content=firststeps-colab&hl=zh-cn)



#### 3. 练习题

![mse-excercise](/Users/jianannan/Desktop/ml-notes/mse-excercise.png)

肉眼直观判断出的均方误差并不一定准确。





### 降低损失

#### 1. 视频讲座（一）



#### 2. 以`迭代方式`降低机器学习模型的损失。



![GradientDescentDiagram](/Users/jianannan/Desktop/ml-notes/GradientDescentDiagram.svg)



简而言之，就是用`猜测、试错`的方式，尽可能地得出损失最低、最佳的模型。

对于线性回归(y' = b + w1x1)问题，事实证明初始值并不重要。我们可以随机选择值作为w1（权重，斜率） 和 b（`偏差（bias）`，截距），然后使用损失函数（例如平方损失函数）将得出的 `预测值 - y'` 和 `特征 x 对应的正确标签 - y`输入，得出损失函数的值。

我们可以通过不断地迭代（在上图中绿色部分，按照一定的策略更新计算的参数），直到总体损失不再变化或至少变化极其缓慢为止。这时候，我们可以说该模型已**收敛**(convergence)。



#### 3. 降低损失 (Reducing Loss)：梯度下降法（gradient descent）

迭代方法中的`更新计算参数`这一步，华而不实，不够具体，效率太低。

**梯度**是偏导数的**矢量**；它可以让您了解哪个方向距离目标“更近”或“更远”。

>  ` 梯度、偏导数`都是微积分中的概念，通常机器学习框架会帮助处理这部分计算，使用者甚至不必了解其具体含义（其实我看了一遍，没看懂。。。）

![GradientDescentGradientStep](/Users/jianannan/Desktop/ml-notes/GradientDescentGradientStep.svg)



梯度下降法即通过计算损失曲线在某处的**负**梯度，从而得知如何改变 权重 ，使得 损失 尽快降低的算法。

在损失曲线上的每一点计算出来的负梯度大小的一部分（之所以只是一部分的原因，见下一节）会与起点相加，得出下一个点的位置，一次这样的操作，称之为`步`。

之后，梯度降低算法会不断重复上一步，直到逐渐接近最低点。



#### 4. 学习速率

梯度是一个具有方向和大小的矢量，梯度下降算法会用梯度以及一个称之为`学习速率`的`标量`，从而确定损失曲线上的下一个点。

例如，梯度大小为 2.5 ，学习速率为 0.01，则梯度下降法算法会选择距离前一个点 0.025 的位置作为下一个点。



学习速率是一个很重要的`超参数（hypeparameter）`，是调节整个算法的重要工具。

过大的学习速率可能会导致永远无法得到最合适的权重，下一个点会在U型曲线的底部来回跳跃，却始终无法到达最底部。

而过小的学习速率则可能拖慢算法的学习效率，导致进行了过多次的计算才到达损失曲线的底部。



通常来说，会有一个 `最合适的学习速率`，发现这个学习速率就是编程人员的主要工作。

![LearningRateJustRight](/Users/jianannan/Desktop/ml-notes/LearningRateJustRight.svg)



#### 5. 优化学习速率

https://developers.google.com/machine-learning/crash-course/fitter/graph，这个页面有一个非常生动的Web App，可以通过交互展示上述过程。



#### 6. 随机梯度下降算法

在梯度下降法中，`批量（batch）`指的是用于在单次迭代中计算梯度的样本总数。

当目前为止，我们一直假定批量是全部所有样本（整个数据集），但有时候，数据集可能非常巨大，包含几十亿、上千亿条样本，这个时候，在选择整个数据集作为批量，就会导致单次迭代要花费很长时间。

并且，批量越大，包含冗余样本（我理解就是不利于高效地确定最佳权重（例如：y' = b + w1x1 w1 即 权重，斜率）的概率也就越大。所以，超大批量所具备的预测价值往往并不比大型批量高。



`随机梯度下降算法（SGD）`每次只迭代一个样本，且样本均为随机挑选，从而通过更少的计算量得出正确的平均梯度。

**小批量(mini-batch)随机梯度下降法**（**小批量 SGD**）是介于全批量迭代与 SGD 之间的折衷方案。小批量通常包含 10-1000 个随机选择的样本。小批量 SGD 可以减少 SGD 中的杂乱样本数量，但仍然比全批量更高效。



#### 7. playground

学习速率和收敛：https://developers.google.com/machine-learning/crash-course/reducing-loss/playground-exercise#%E5%AD%A6%E4%B9%A0%E9%80%9F%E7%8E%87%E5%92%8C%E6%94%B6%E6%95%9B



#### 8. 练习题

基于大型数据集执行梯度下降法时，以下哪个批量大小可能比较高效？

- 全批量。

- 小批量或甚至包含一个样本的批量 (SGD)。

令人惊讶的是，在小批量或甚至包含一个样本的批量上执行梯度下降法通常比全批量更高效。毕竟，计算一个样本的梯度要比计算数百万个样本的梯度成本低的多。为确保获得良好的代表性样本，该算法在每次迭代时都会抽取另一个随机小批量数据（或包含一个样本的批量数据）。





> 学到这里，我有一个感受，文档中的很多名词让我感觉很陌生，例如权重、收敛、标签、特征等等，很影响我的学习效率，可能因为以下两方面原因：

> - 个人的数学基础不好，对这些数学概念不熟悉。
> - 这些名词的中文翻译也过于生硬，不够信雅达。





### 五、使用 TensorFlow 的起始步骤 (First Steps with TensorFlow)



#### 1. 工具包

TensorFlow 工具包有多个层次的 API 可供使用，我们主要学习 `tf.estimator 面向对象的高级API`。

您在练习中所做的一切都可以在较低级别（原始）的 TensorFlow 中完成，但使用 tf.estimator 会大大减少代码行数。



#### 2. 编程练习

> Excited！终于上手了！

1. 首先，需要熟悉一下 广泛应用于 TensorFlow 编码进行数据分析和建模的重要库——Pandas。这个库可以方便地对数据集进行格式化处理，以符合 TensorFlow 训练的需要。
2. [使用 TensorFlow 的起始步骤](https://colab.research.google.com/notebooks/mlcc/first_steps_with_tensor_flow.ipynb?utm_source=mlcc&utm_campaign=colab-external&utm_medium=referral&utm_content=firststeps-colab&hl=zh-cn)：此练习介绍了线性回归。

**VIP: **

这一节练习展示了一次完整的利用线性回归训练模型，并通过均方根误差（RMSE）来评估模型的准确率的过程。

大致流程是这样的：

1. 定义`特征`并配置 `特征列` 、 `目标（标签）`

复习：

`标签（label）`（有时称为`目标`）：指我们要预测的真实事物：**y**，例如垃圾邮件过滤中的“垃圾邮件或非垃圾邮件”，此处是一个街区内房屋的均价。

`特征（feature）`：指用于描述数据的输入变量：**x下标i**，例如邮件的内容、标题、收发件人。”，此处用的是街区的房屋总数量。

即，用一个街区的房屋总数量来预测这个街区的房间价格，训练出来的模型描述的是，街区的房屋总数量和房屋均价之间的关系。

```python
# Define the input feature: total_rooms.
my_feature = california_housing_dataframe[["total_rooms"]]

# Configure a numeric feature column for total_rooms.
feature_columns = [tf.feature_column.numeric_column("total_rooms")]


# Define the label/target.
targets = california_housing_dataframe["median_house_value"]
```





2. 配置 线性回归 模型，并使用 `GradientDescentOptimizer`（它会实现小批量随机梯度下降法 (SGD)）训练该模型。`learning_rate` 参数可控制梯度步长的大小。

```python
# Use gradient descent as the optimizer for training the model.
my_optimizer = tf.train.GradientDescentOptimizer(learning_rate=0.0000001)

# 注意：为了安全起见，我们还会通过 clip_gradients_by_norm 将梯度裁剪应用到我们的优化器。梯度裁剪可确保梯度大小在训练期间不会变得过大，梯度过大会导致梯度下降法失败。
my_optimizer = tf.contrib.estimator.clip_gradients_by_norm(my_optimizer, 5.0)


# Configure the linear regression model with our feature columns and optimizer.
# Set a learning rate of 0.0000001 for Gradient Descent.
linear_regressor = tf.estimator.LinearRegressor(
    feature_columns=feature_columns,
    optimizer=my_optimizer
)
```





3. 定义输入函数，格式化输入数据为`一个批量（batch）`的 `LinearRegressor` 可以处理的`特征 - 标签键值对(return features, labels)`

````python
def my_input_fn(features, targets, batch_size=1, shuffle=True, num_epochs=None):
    """Trains a linear regression model of one feature.
  
    Args:
      features: pandas DataFrame of features
      targets: pandas DataFrame of targets
      batch_size: Size of batches to be passed to the model
      shuffle: True or False. Whether to shuffle the data.
      num_epochs: Number of epochs for which data should be repeated. None = repeat indefinitely
    Returns:
      Tuple of (features, labels) for next data batch
    """
  
    # Convert pandas data into a dict of np arrays.
    features = {key:np.array(value) for key,value in dict(features).items()}                                           
 
    # Construct a dataset, and configure batching/repeating.
    ds = Dataset.from_tensor_slices((features,targets)) # warning: 2GB limit
    ds = ds.batch(batch_size).repeat(num_epochs)
    
    # Shuffle the data, if specified.
    if shuffle:
      ds = ds.shuffle(buffer_size=10000)
    
    # Return the next batch of data.
    features, labels = ds.make_one_shot_iterator().get_next()
    return features, labels
````



4. 初步训练并评估模型

训练 100 步：

```python
_ = linear_regressor.train(
    input_fn = lambda:my_input_fn(my_feature, targets),
    steps=100
)
```

基于训练100次后的该训练模型（linear_regressor）做一次预测，看看我们的模型在训练期间与这些数据的拟合情况。

```python
# Create an input function for predictions.
# Note: Since we're making just one prediction for each example, we don't 
# need to repeat or shuffle the data here.
prediction_input_fn =lambda: my_input_fn(my_feature, targets, num_epochs=1, shuffle=False)

# Call predict() on the linear_regressor to make predictions.
predictions = linear_regressor.predict(input_fn=prediction_input_fn)

# Format predictions as a NumPy array, so we can calculate error metrics.
predictions = np.array([item['predictions'][0] for item in predictions])

# Print Mean Squared Error and Root Mean Squared Error.
mean_squared_error = metrics.mean_squared_error(predictions, targets)
root_mean_squared_error = math.sqrt(mean_squared_error)
print("Mean Squared Error (on training data): %0.3f" % mean_squared_error)
print("Root Mean Squared Error (on training data): %0.3f" % root_mean_squared_error)
```

Output:

```
Mean Squared Error (on training data): 56425.111
Root Mean Squared Error (on training data): 237.540
```

可以通过比较 RMSE 与目标最大值和最小值的差值来判断这个模型的质量

```python
min_house_value = california_housing_dataframe["median_house_value"].min()
max_house_value = california_housing_dataframe["median_house_value"].max()
min_max_difference = max_house_value - min_house_value

print("Min. Median House Value: %0.3f" % min_house_value)
print("Max. Median House Value: %0.3f" % max_house_value)
print("Difference between Min. and Max.: %0.3f" % min_max_difference)
print("Root Mean Squared Error: %0.3f" % root_mean_squared_error)
```

output:

```
Min. Median House Value: 14.999
Max. Median House Value: 500.001
Difference between Min. and Max.: 485.002
Root Mean Squared Error: 237.295
```

误差跨越目标值的近一半范围，总的目标 - 房价范围是14至500，但总误差之和的N分之一，均方根误差达到了237，标示预测结果的误差可能和真实情况有237的差值。

需要进一步缩小误差。



> 效果越好，回归线与数据的拟合度（`拟合度`即模型与[**训练数据**](https://developers.google.com/machine-learning/glossary/#training_set)匹配的程度）就越高，最终 RMSE （均方根误差） 也会越低。
>
> —— [合成特征和离群值](https://colab.research.google.com/notebooks/mlcc/synthetic_features_and_outliers.ipynb?utm_source=mlcc&utm_campaign=colab-external&utm_medium=referral&utm_content=syntheticfeatures-colab&hl=zh-cn)

> ##### 过拟合 (overfitting)
>
> 创建的模型与[**训练数据**](https://developers.google.com/machine-learning/glossary/#training_set)过于匹配，以致于模型无法根据新数据做出正确的预测。



复习：

`RMSE - 均方根误差`可以用来判断模型的质量和误差大小，RMSE 的一个很好的特性是，它可以在与原目标相同的规模下解读。

我们用`平方损失（L2损失）`来更科学地汇总一个模型的损失。

- 单个样本的`平方损失`公式如下：

```
  = the square of the difference between the label and the prediction
  = (observation - prediction(x))2
  = (y - y')2
```

- 每个样本的平均平方损失称之为：**均方误差** (**MSE - Mean Squared Error**)，公式如下：

![mse-formula](/Users/jianannan/Desktop/ml-notes/mse-formula.png)

- `RMSE（均方根误差） = 均方误差 开根号`



还可以利用 `数形结合` 判断模型的质量：

```
...
```



![model-plot](/Users/jianannan/Desktop/ml-notes/model-plot.png)

图中蓝点为样本，红线为模型，可以直观地看出，偏差较大。





5. 调整超参数，重新训练模型，降低损失，促进模型收敛

将整个训练过程封装成一个函数，方便调试、训练。

```python
def train_model(learning_rate, steps, batch_size, input_feature="total_rooms"):

....
```



```
train_model(
    learning_rate=0.0001,
    steps=200,
    batch_size=1
)
```

Output:

```
Training model...
RMSE (on training data):
  period 00 : 214.42
  period 01 : 194.62
  period 02 : 180.53
  period 03 : 170.91
  period 04 : 167.02
  period 05 : 166.53
  period 06 : 167.02
  period 07 : 168.84
  period 08 : 170.92
  period 09 : 173.57
Model training finished.
```



![change-super-params-traning](/Users/jianannan/Desktop/ml-notes/change-super-params-traning.png)

通过多次调整超参数（learning_rate、step等）可以发现，不同的超参数对模型的损失、训练用时等均有显著的不同影响。



也可以通过调整 特征，验证其对模型的影响：

```python
train_model(
    learning_rate=0.0002,
    steps=200,
    batch_size=1,
    input_feature="population"
)
```



2018/10/28



3. [合成特征和离群值](https://colab.research.google.com/notebooks/mlcc/synthetic_features_and_outliers.ipynb?utm_source=mlcc&utm_campaign=colab-external&utm_medium=referral&utm_content=syntheticfeatures-colab&hl=zh-cn)

这一节练习在上一节练习的基础上，介绍了：

- 通过`将两个特征合成为一个 合成特征`来当作线性回归的输入训练模型。

我自己的尝试时，写出了如下代码：

```python
california_housing_dataframe["rooms_per_person"] = 1000 * (california_housing_dataframe["population"] / ( california_housing_dataframe["total_rooms"]))

calibration_data = train_model(
    learning_rate=0.0003,
    steps=200,
    batch_size=5,
    input_feature="rooms_per_person"
)


Training model...
RMSE (on training data):
  period 00 : 223.98
  period 01 : 212.23
  period 02 : 202.60
  period 03 : 196.01
  period 04 : 191.25
  period 05 : 189.43
  period 06 : 190.03
  period 07 : 191.76
  period 08 : 192.15
  period 09 : 193.99
Model training finished.
```



`1000 *`是因为，我发现不加这个 RMSE  均方根误差 下降的很慢，加上之后，很快就能下降到 200 以下。

看了参考答案后，知道这个思路不太对，应该通过`调整超参数来降低损失`。

```python
california_housing_dataframe["rooms_per_person"] = (
    california_housing_dataframe["total_rooms"] / california_housing_dataframe["population"])

# 应该通过加大 学习速率 和 步，来加快损失的降低
calibration_data = train_model(
    learning_rate=0.05,
    steps=500,
    batch_size=5,
    input_feature="rooms_per_person")
```



```
Training model...
RMSE (on training data):
  period 00 : 212.76
  period 01 : 189.71
  period 02 : 169.05
  period 03 : 151.68
  period 04 : 141.42
  period 05 : 134.12
  period 06 : 131.18
  period 07 : 130.64
  period 08 : 130.88
  period 09 : 131.29
Model training finished.
```





- 通过识别和截取（移除）输入数据中的离群值（大于一定界限的值）来提高模型的有效性。

> 我们可以通过创建预测值与目标值的散点图来可视化模型效果。
>
> 理想情况下，这些值将位于一条完全相关的对角线上。

但实际上，结果是这样的：

```python
# calibration_data = train_model(
    learning_rate=0.05,
    steps=500,
    batch_size=5,
    input_feature="rooms_per_person")

plt.figure(figsize=(15, 6))
plt.subplot(1, 2, 1)
plt.scatter(calibration_data["predictions"], calibration_data["targets"])
```



![outliners-1](/Users/jianannan/Desktop/ml-notes/outliners-1.png)

大多数根据训练过的模型得到的预测值都与一条几乎垂直的线对齐，也有相对较少的点远远偏离这条垂直的线，这两点在直方图📊上尤其明显：

![outliers-zhifangtu](/Users/jianannan/Desktop/ml-notes/outliers-zhifangtu.png)

如果能`排除（截取）`掉占少数的离群值，对模型的`拟合情况（模型与训练数据匹配的情况）`会有很大的改善。

可以这样做：

```python
california_housing_dataframe["rooms_per_person"] = (
    california_housing_dataframe["rooms_per_person"]).apply(lambda x: min(x, 5))

_ = california_housing_dataframe["rooms_per_person"].hist()
```

![outliers-zhifangtu-2](/Users/jianannan/Desktop/ml-notes/outliers-zhifangtu-2.png)

再次训练，会发现模型的拟合度好多了：

![training_after_outliers](/Users/jianannan/Desktop/ml-notes/training_after_outliers.png)

再次创建预测值与目标值的散点图来可视化地观察模型效果也能看出这一点：



![fitting-after-outliers](/Users/jianannan/Desktop/ml-notes/fitting-after-outliers.png)







2018/10/31