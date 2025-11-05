# Python-机器学习



## Sklearn

### Scikit-learn

#### 数据集API

##### sklearn.datasets

###### 概念介绍

​		用于加载流行数据集，返回datasets.base.Bunch（字典格式）

​		**data**:	特征数据数组，由 [n_samples*n_features] 组成的二维numpy.ndarray数组

​		**target**:	标签数组，是 n_samples 的一维 numpy.ndarray数组

​		**descr**:	数据信息描述

​		**feature_names**:	特征名，但新闻数据/手写数字/回归数据 没有

​		**target_names**:	  标签名



###### 获取方式

​	**datasets.load_*()**

​		获取小规模数据集，数据包含在datasets中



​	**datasets.fetch_(data_home=None,subset = 'train')**

​		获取大规模数据集，需要从网络上下载

​		**data_home**，表示数据集下载的目录（默认为：~/scikit_learn_data/）

​		**subset**：train / test / all 选择要加载的数据集（训练集/测试机/全都要） 