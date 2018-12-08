# RNN / LSTM implementation

#### RNN
##### RNNCell
- 每个RNNCell都有一个call方法，使用方式是：(output, next_state) = call(input, state)。每调用一次RNNCell的call方法，就相当于在时间上“推进了一步”，这就是RNNCell的基本功能。
- size 参数
    - state_size: 隐层的大小
    - output_size: 输出的大小
    
    通常是将一个batch送入模型计算，设输入数据的形状为(batch_size, input_size)，那么计算时得到的隐层状态就是(batch_size, state_size)，输出就是(batch_size, output_size)。

```python
import tensorflow as tf
import numpy as np

cell = tf.nn.rnn_cell.BasicRNNCell(num_units=128) # state_size = 128
print(cell.state_size) # 128

inputs = tf.placeholder(np.float32, shape=(32, 100)) # 32 是 batch_size
h0 = cell.zero_state(32, np.float32) # 通过zero_state得到一个全0的初始状态，形状为(batch_size, state_size)
output, h1 = cell.call(inputs, h0) #调用call函数

print(h1.shape) # (32, 128)
```

##### 学习如何一次执行多步：tf.nn.dynamic_rnn

TensorFlow提供了一个tf.nn.dynamic_rnn函数，使用该函数就相当于调用了n次call函数。即通过{h0,x1, x2, …., xn}直接得{h1,h2…,hn}。

设我们输入数据的格式为(batch_size, time_steps, input_size)，其中time_steps表示序列本身的长度，如在Char RNN中，长度为10的句子对应的time_steps就等于10。最后的input_size就表示输入数据单个序列单个时间维度上固有的长度。另外我们已经定义好了一个RNNCell，调用该RNNCell的call函数time_steps次。

```python
# inputs: shape = (batch_size, time_steps, input_size) 
# cell: RNNCell
# initial_state: shape = (batch_size, cell.state_size)。初始状态。一般可以取零矩阵
outputs, state = tf.nn.dynamic_rnn(cell, inputs, initial_state=initial_state)

# outputs size (batch_size, time_steps, cell.output_size)
# state size  (batch_size, cell.state_size)
```



### Reference
- [TensorFlow中RNN实现的正确打开方式](https://zhuanlan.zhihu.com/p/28196873)