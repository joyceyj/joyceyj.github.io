---
layout: post
title: Data Mining期末报告
date: 2018-07-18
categories: Data Mining
tag: Data Mining
---

# Data Mining期末报告
15331356杨婕
## 分类算法介绍与分析
* 课堂上所介绍的算法，或者相关研究领域的算法(请引用相关 的参考文献)
* 分析所用算法理论上的优缺点、时间复杂度、内存需求等

期末项目是一个分类问题，但难点在于样本的数量多，大约有7万个样本，并且特征的维数大，有6812维，对于分类器的训练除了正确率，还存在模型训练的占用内存问题和训练时间复杂度。
所以基于这两点，进行了各种尝试。

### 1.基于边缘检测的SVM分类算法
* 参考论文：
	* [A fast SVM training method for very large datasets](https://ieeexplore.ieee.org/document/5178618/?arnumber=5178618)
	* Fast SVM Training Using Edge Detection on Very Large Datasets
* 原理：
	* 对于大数据的SVM算法，可以采用**抽样减少训练样本**的方法。
	* 但是减少训练集会影响分类器的性能，在支持向量机中，**分类边界是由支持向量决定的**，为了保证分类效率，应该保留最有可能成为支持向量的样本。
	* 根据SVM理论，**在分类边界附近的样本，成为支持向量的可能性更高**，因此通过**检测边缘样本**训练SVM模型可以保持训练分类器的性能，也能大大减少训练集数量，达到减小训练占用内存和训练时间的问题。
	* 但是仅仅采用边界检测得到的样本不能有效反映原训练集的分布状况，会影响分类器的分类精度，所以通过聚类技术，对原训练集进行聚类得到聚类中心，加入压缩后的训练集。在论文中是使用K-means算法，也可以使用其他聚类算法。
* 边缘检测算法和聚类算法重构训练集：
	* 确定最近邻数目m
	* 通过边界检测技术，寻找训练集的边界点
		* （1）对于每个样本点，查找其m个最近邻居（使用KNN算法）
		* （2）检查最近邻居的样本类别（label），如果和该样本点属于同一个类别（说明不是边界样本），那么这个样本点将被删除，如果不是同一个类别，则保留该样本点
	* 确定K-means的K值，对整个训练集进行聚类
	* 重构训练集，将边缘样本和聚类中心取并集得到压缩的训练集
* 论文中模拟实验效果：
	* 利用边缘检测和聚类算法减少样本，重构训练集：
	
		<img src='/image/DataMing/边缘检测1.png' width=50% hegiht=50% align=center />
	
	* 原始训练集及SVM训练超平面与重构训练集及SVM训练超平面：
	
		<img src='/image/DataMing/边缘检测2.png' width=50% hegiht=50% align=center />
	
	* 训练准确率及训练时间复杂度对比：
	
		<img src='/image/DataMing/边缘检测3.png' width=50% hegiht=50% align=center />
	
* 算法存在的问题：
	* 虽然可以减少大量样本，并且加快训练的速度，但是前期数据处理需要花费大量时间
* 算法应用：
	* 由于项目中的数据类别有397个，训练一对多分类器也需要训练约400个，在检测边缘时，需要对每个样本点检测，处理时间需要一周多（甚至更长时间），所以实际对训练的内存有减少但时间却增加了。
	* 聚类的K值难以确定（论文中没有说明如何选取，所以应用中对每个类按50个样本取一个聚类中心），对数据重构的效果可能不如论文中的重构效果。
	* 该算法**减少大量样本**，训练不需要大量的内存，但是由于按照预处理**时间太长**，最终没有运行出结果。
	* **使用其他方法查找边缘**：
		* 使用每个类聚类的中心，查找每个类中距离每个中心最远的10个样本作为边缘，从而得到减少样本的训练集，进行SVM训练(使用sk-learn的SVM接口），使用一对多的思想分别训练397个分类器，再分别用不同的分类器进行预测，如果在某个分类器预测为1则标记为该分类器正样本的label。
		* 该算法占用较少的内存，大约4G的内存，训练时间大约为10小时
		* 但是这个算法的**准确率非常低**，原因可能是聚类的K值不合理，或者查找的边缘样本不足，或者这样查找的样本不是边缘样本。

### 2.LightGBM
* 参考文献：
	* LightGBM官方文档http://lightgbm.readthedocs.io/en/latest/Python-API.html
* 原理：Light GBM是微软的开源项目，是对基础提升算法的改进，对数据和特征进行并行化，提高训练效率；用GOSS对训练样本进行采样，在保持分类性能的基础上加速训练；用EFB对数据降维，在保持分类性能的基础上加速训练。
* 实现：
	* 训练397个分类器，每个分类器的正样本取该分类器对应标签的所有样本作为正样本，其余每个类取2个样本，大约800个样本作为负样本

	```
	# create dataset for lightgbm
    x_train = df_train_x.values
    y_train = df_train_y.values
    lgb_train = lgb.Dataset(x_train, y_train)
    # specify your configurations as a dict
    params = {
        'boosting_type': 'gbdt',
        'objective': 'binary',
        'metric': 'binary_logloss',
        'num_leaves': 31,
        'learning_rate': 0.05,
        'feature_fraction': 0.9,
        'bagging_fraction': 0.8,
        'bagging_freq': 5,
        'verbose': 1
    }

    # train
    print("Start training...")
    gbm = lgb.train(params,
                lgb_train,
                num_boost_round=10)
	```
	* 用test数据进行预测时，对一个样本按分类器遍历预测，如果某个分类器预测结果为1，则将该样本预测为该分类器的label，如果预测为-1，则读取下一个分类器进行预测。
	* 实验结果：
		* 训练时间非常短，预处理大约需要20分钟，训练397个分类器大约花了一个半小时 
		* 大部分样本分类为2，kaggle上准确率只有0.04
		* 猜想原因：由于分开训练分类器进行预测，使得分类非常依赖于前几个分类器，比如实验中label为2的分类器效果不好，使得大量样本分为2
		* 如果用Light GBM的多分类直接对所有样本进行训练可能效果会不错，但是需要占用大量内存，所以没有进行实验，而是选择其他算法进行实验

### 3.多层感知机MLPClassifier
* 参考文献：
	* SK-learn官方文档：http://scikit-learn.org/dev/modules/neural_networks_supervised.html#classification
* 原理：多层感知机（MLP，Multilayer Perceptron）也叫人工神经网络（ANN，Artificial Neural Network），除了输入输出层，它中间可以有多个隐层，最简单的MLP只含一个隐层，即三层的结构，如下图：

	<img src='/image/DataMing/mlp1.jpeg' width=50% hegiht=50% align=center />
	
	多层感知机层与层之间是全连接的，假设输入层用向量X表示，则隐藏层的输出就是f(W1X+b1)，W1是权重（也叫连接系数），b1是偏置，函数f 可以是常用的sigmoid函数或者tanh函数。隐藏层到输出层可以看成是一个多类别的逻辑回归，也即softmax回归，所以输出层的输出就是softmax(W2X1+b2)，X1表示隐藏层的输出f(W1X+b1)。解决最优化问题是用梯度下降法（SGD）：首先随机初始化所有参数，然后迭代地训练，不断地计算梯度和更新参数，直到满足某个条件为止（比如误差足够小、迭代次数足够多时）。
* 实现：
	* 使用python的sk-learn机器学习库的MLPClassifier接口，直接调用，修改隐藏层的层数及神经元个数进行调参
	* 使用全部的训练集，对特征集进行归一化，然后传入MLPClassifier函数
	* 由于使用全部数据集，并且没有降维，所以读取csv格式数据需要很长时间，48分钟左右，大约1个小时，归一化大约需要10分钟，并且占用大约10G的内存
	* 使用2个隐藏层，各100个神经元
	```
	clf = MLPClassifier(solver='adam', alpha=1e-5,hidden_layer_sizes=(100,100), random_state=1,verbose=True,max_iter=100)
	```
	
		<img src='/image/DataMing/mlp2.png' width=50% hegiht=50% align=center />
	
		经过22个迭代，模型收敛，损失函数loss=0.22404616，模型训练大约6分钟，预测准确率为0.17229
	* 使用2个隐藏层，各500个神经元
	```
	clf = MLPClassifier(solver='adam', alpha=1e-5,hidden_layer_sizes=(500,500), random_state=1,verbose=True,max_iter=100)
	```
	<img src='/image/DataMing/mlp3.png' width=50% hegiht=50% align=center />
	
		经过9个迭代，模型收敛，损失函数loss=0.33467016，模型训练大约9分钟，预测准确率为0.21360
	
### 4.SGDClassifier
* 参考文献：
	* SK-learn官方文档：http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.SGDClassifier.html

* 原理：SGDClassifier是一个用随机梯度下降算法训练的线性分类器的集合。默认情况下是一个线性（软间隔）支持向量机分类器，用梯度下降算法来最小化支持向量机的合页损失函数。
* 实现：
	* 使用python的sk-learn机器学习库的linear_model.SGDClassifier接口，直接调用
	* 使用全部的训练集，对特征集进行归一化，然后传入linear_model.SGDClassifier函数
	* 由于使用全部数据集，并且没有降维，所以读取csv格式数据需要很长时间，48分钟左右，大约1个小时，归一化大约需要10分钟，并且占用大约10G的内存
	* 训练时间大约为34分钟，准确率为0.25978


## 实验结果分析
* 对比不同算法的分类效果和计算复杂度
<table>
    <thead>
        <tr>
            <th style="text-align: center;">使用算法</th>
            <th style="text-align: center;">运行时间</th>
            <th style="text-align: center;">分类准确率</th>
        </tr>
    </thead>
    <tbody>
       <tr>
            <th>基于边缘检测的SVM算法：KNN边缘检测</th>
            <th>预计大于2周</th>
            <th>没有运行出来</th>
        </tr>
        <tr>
            <th>基于边缘检测的SVM算法：利用聚类中心查找边缘</th>
            <th>约20小时</th>
            <th>0.00251</th>
        </tr>
        <tr>
            <th>LightGBM</th>
            <th>约2小时</th>
            <th>0.04860</th>
        </tr>
        <tr>
            <th> MLPClassifier：2个隐藏层，各100个神经元 </th>
            <th>读取处理数据约1小时，训练约6分钟</th>
            <th>0.17229</th>
        </tr>
        <tr>
            <th> MLPClassifier：2个隐藏层，各500个神经元 </th>
            <th>读取处理数据约1小时，训练约9分钟</th>
            <th>0.21360</th>
        </tr> 
        <tr>
            <th> SGDClassifier </th>
            <th>读取处理数据约1小时，训练约34分钟</th>
            <th>0.25978</th>
        </tr>
    </tbody>
</table>