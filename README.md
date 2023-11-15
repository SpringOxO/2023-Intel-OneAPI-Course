# 2023 Intel 校企合作课程——OneAPI加速机器学习实践技术博客文档

## 基于Intel® Distribution of Modin 和 Intel® Extension for Scikit-learn 及 Intel® DAAL加速的信用卡交易欺诈检测

### 案例概述：

根据欧洲持卡人 2013 年 9 月通过信用卡进行的交易数据集训练模型，用以分类预测信用卡诈骗。

### 分析和计划：

数据集中分类为诈骗的数据非常少，极不平衡，需要对此进行预处理。计划采用SMOTE解决数据不平衡问题。取70%数据作为训练集。

后续先使用默认xgboost试水，然后使用网格查找+k折交叉验证的随机森林做参数调优，获得尽量好的结果，最后对xgboost做参数调优，与随机森林比较。

根据题目要求，使用F1和AUPRC作为主要评价标准。混淆矩阵精度在此类不平衡数据中是无关紧要的。

实验环境为Intel devcloud，使用了Intel OneAPI的modin来提高数据读取和预处理的效率，使用OneAPI对sklearn和xgboost的优化提高训练速度，使用daal4py转化训练模型，提高预测速度。

### 代码实现：

见21307130097.ipynb（内有注释）

modin确实很大提升了数据处理效率，daal4py对数据预测的提升也是很显著的。由于网格搜索成本实在太高，时间所限并没有完成特别细致的参数调优。

附：devcloud上使用modin-hdk并装载其他库的方法

打开terminal，做

1. Install the Intel® Distribution of Modin* python environment (Only python 3.8 - 3.10 are supported).
   ```
   conda create -n modin-hdk python=3.x -y
   ```
2. Activate the Conda environment.
   ```
   conda activate modin-hdk
   ```
3. Install modin-hdk, Intel® Extension for Scikit-learn* and related libraries.
   ```
   conda install modin-hdk -c conda-forge -y
   pip install scikit-learn-intelex
   pip install matplotlib
   ```
4. Install Jupyter Notebook
   ```
   pip install jupyter ipykernel
   ```
5. Add kernel to Jupyter Notebook.
   ```
   python -m ipykernel install --user --name modin-hdk
   ```
然后在jupyter notebook中选择modin-hdk作为内核。
（以上来自OneAPI-samples/End-to-End-Workloads/Census中的引导）
向内核中添加库：
打开terminal，激活环境：
   ```
   conda activate modin-hdk
   ```
安装库
   ```
   pip install ...
   ```
装载到jupyter内核
   ```
   python -m ipykernel install --user --name modin-hdk
   ```
