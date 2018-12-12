# RNN / LSTM implementation

#### RNN
##### RNNCell
- 通过BasicRnnCell定义的实例对象RNNCell，其中两个属性Cell.state_size和Cell.output_size返回的都是state_size(RNN层神经元的个数， 参数名num_units).
- 每个RNNCell都有一个call方法，使用方式是：(output, next_state) = call(input, state)。每调用一次RNNCell的call方法，就相当于在时间上“推进了一步”，这就是RNNCell的基本功能。
- size 参数
    - state_size: 隐层的大小
    - output_size: 输出的大小
    
    通常是将一个batch送入模型计算，设输入数据的形状为(batch_size, input_size)，那么计算时得到的隐层状态就是(batch_size, state_size)，输出就是(batch_size, output_size)。
- 在BasicRNNCell中，output其实和隐状态的值是一样的。因此，我们还需要额外对输出定义新的变换，才能得到图中真正的输出y。state_size 永远等于 output_size。
    
```python
import tensorflow as tf
import numpy as np

cell = tf.nn.rnn_cell.BasicRNNCell(num_units=128) # state_size = 128
print(cell.state_size) # 128
print(cell.output_size) # 128

inputs = tf.placeholder(np.float32, shape=(32, 100)) # 32 是 batch_size
h0 = cell.zero_state(32, np.float32) # 通过zero_state得到一个全0的初始状态，形状为(batch_size, state_size)
output, h1 = cell.call(inputs, h0) #调用call函数
# output = new_state = act(W * input + U * state + B) 
print(h1.shape) # (32, 128)
```
##### LSTMCell
*tf.contrib.rnn.BasicLSTMCell*的用法和*tf.nn.rnn_cell.BasicRNNCell*基本类似，但是每个隐层包含两个隐状态，每个都是(batch_size, state_size)的形状。

```python
lstm_cell = tf.nn.rnn_cell.BasicLSTMCell(num_units=128)
inputs = tf.placeholder(np.float32, shape=(32, 100)) # 32 是 batch_size
h0 = lstm_cell.zero_state(32, np.float32) # 通过zero_state得到一个全0的初始状态
output, h1 = lstm_cell.call(inputs, h0)

print(h1.h)  # shape=(32, 128)
print(h1.c)  # shape=(32, 128)


```

##### 学习如何一次执行多步：tf.nn.dynamic_rnn

TensorFlow提供了一个tf.nn.dynamic_rnn函数，使用该函数就相当于调用了n次call函数。即通过{h0,x1, x2, …., xn}直接得{h1,h2…,hn}。

设我们输入数据的格式为(batch_size, time_steps, input_size)，其中time_steps表示序列本身的长度，如在Char RNN中，长度为10的句子对应的time_steps就等于10。最后的input_size就表示输入数据单个序列单个时间维度上固有的长度。另外我们已经定义好了一个RNNCell，调用该RNNCell的call函数time_steps次。

```python
# inputs: shape = (batch_size, time_steps, input_size) 
# cell: RNNCell
# initial_state: shape = (batch_size, cell.state_size)。初始状态。一般可以取零矩阵
outputs, state = tf.nn.dynamic_rnn(cell, inputs, initial_state=initial_state)
```
- outputs: outputs就是time_steps步里所有的输出, (batch_size, time_steps, cell.output_size)
- state: 最后一步的隐状态 (batch_size, cell.state_size)


##### 学习如何堆叠RNNCell：MultiRNNCell

```python
import tensorflow as tf
import numpy as np

# 每调用一次这个函数就返回一个BasicRNNCell
def get_a_cell():
    return tf.nn.rnn_cell.BasicRNNCell(num_units=128)
# 用tf.nn.rnn_cell MultiRNNCell创建3层RNN
cell = tf.nn.rnn_cell.MultiRNNCell([get_a_cell() for _ in range(3)]) # 3层RNN
# 得到的cell实际也是RNNCell的子类
# 它的state_size是(128, 128, 128)
# (128, 128, 128)并不是128x128x128的意思
# 而是表示共有3个隐层状态，每个隐层状态的大小为128
print(cell.state_size) # (128, 128, 128)
# 使用对应的call函数
inputs = tf.placeholder(np.float32, shape=(32, 100)) # 32 是 batch_size
h0 = cell.zero_state(32, np.float32) # 通过zero_state得到一个全0的初始状态
output, h1 = cell.call(inputs, h0)
print(h1) # tuple中含有3个32x128的向量 
```


### Reference
- [TensorFlow中RNN实现的正确打开方式](https://zhuanlan.zhihu.com/p/28196873)
- [BasicLSTMCell中num_units参数解释](https://blog.csdn.net/notHeadache/article/details/81164264)