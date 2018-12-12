# Optimizer in tensorflow

### 交叉熵损失函数
针对分类问题，实现了四个交叉熵函数，分别是
```python
tf.nn.sigmoid_cross_entropy_with_logits
tf.nn.softmax_cross_entropy_with_logits
tf.nn.sparse_softmax_cross_entropy_with_logits
tf.nn.weighted_cross_entropy_with_logits
```
- 交叉熵计算函数输入中的logits都不是softmax或sigmoid的输出，而是softmax或sigmoid函数的输入，因为它在函数内部进行sigmoid或softmax操作。


#### sigmoid_cross_entropy_with_logits
- 适用：每个类别相互独立但互不排斥的情况：例如一幅图可以同时包含一条狗和一只大象。
- output不是一个数，而是一个一维数组，包含batch中每个样本的loss,所以一般配合tf.reduce_mean(loss)使用。


#### softmax_cross_entropy_with_logits
- labels：和logits具有相同type和shape的张量(tensor)，,是一个有效的概率，sum(labels)=1, one_hot=True(向量中只有一个值为1.0，其他值为0.0)。
- 适用：每个类别相互独立且排斥的情况，一幅图只能属于一类，而不能同时包含一条狗和一只大象
- outputs 与 *sigmoid_cross_entropy_with_logits* 相同

### batch normalization

在启用激活函数前对batch进行更新。
```python
fc_mean, fc_var = tf.nn.moments(
    Wx_plus_b,
    axes=[0],   # 想要 normalize 的维度, [0] 代表 batch 维度
                # 如果是图像数据, 可以传入 [0, 1, 2], 相当于求[batch, height, width] 的均值/方差, 注意不要加入 channel 维度
)
scale = tf.Variable(tf.ones([out_size]))
shift = tf.Variable(tf.zeros([out_size]))
epsilon = 0.001
Wx_plus_b = tf.nn.batch_normalization(Wx_plus_b, fc_mean, fc_var, shift, scale, epsilon)
# 上面那一步, 在做如下事情:
# Wx_plus_b = (Wx_plus_b - fc_mean) / tf.sqrt(fc_var + 0.001)
# Wx_plus_b = Wx_plus_b * scale + shift
```


### Reference
- [TensorFlow交叉熵函数(cross_entropy)·理解](https://www.jianshu.com/p/cf235861311b)
- [Batch Normalization 批标准化](https://morvanzhou.github.io/tutorials/machine-learning/tensorflow/5-13-BN/)


