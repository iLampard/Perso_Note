# 贝叶斯统计

## 贝叶斯公式

后验概率$$P(\theta \vert x)$$可以写成

$$ 
P(\theta \vert x) = \frac{P(x \vert \theta)P(\theta)}{P(x)}
$$

其中 $$P(x \vert \theta)$$是似然概率，$$P(\theta)$$是先验概率，$$P(x)$$是evidence。


## 最大似然估计、最大后验估计与贝叶斯估计

三种参数估计方法的区别
- 最大似然估计(ML)的目标是找出参数$$\theta$$使得模型产出的观察数据$$x$$的概率最大

$$
\mathop{\arg\max}_{\theta} P(x \vert \theta )
$$

- 最大后验估计(MAP)的目标是找出参数$$\theta$$使得模型产出的观察数据的后验概率最大

$$
\mathop{\arg\max}_{\theta} P(\theta \vert x)  = \mathop{\arg\max}_{\theta} P(x \vert \theta) P(\theta) 
$$

最大似然估计和最大后验估计都假设参数是个确定值，而数据集是该分布下产生的随机变量。
- 贝叶斯估计假设参数是个随机值。贝叶斯估计把参数看出是符合某种先验分布的随机变量，而不是确定数值。在样本分布上，计算参数所有可能的情况，并通过计算参数的期望，得到后验概率密度。所以贝叶斯的计算量很大，涉及后验分布的计算以及参数$$\theta$$分布的处理。

对于新出现的数据$$x_{m+1}$$，最大似然估计使用参数$$\theta$$的点估计对样本的概率值做出预测，而贝叶斯估计则使用参数$$\theta$$的全分布进行预测

$$
P(x_{m+1} \vert x_1, x_2, ...,x_m) = \int P(x_{m+1} \vert \theta) P(\theta \vert x_1, x_2, ..., x_m)) d \theta
$$

