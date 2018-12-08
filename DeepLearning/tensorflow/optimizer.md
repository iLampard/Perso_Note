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

### Reference
[TensorFlow交叉熵函数(cross_entropy)·理解](https://www.jianshu.com/p/cf235861311b)