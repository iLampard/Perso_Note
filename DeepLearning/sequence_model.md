### 基本模型的概念

#### Recurrent Neutral Network (RNN)
- 结构：重复神经网络模块的链式形式
- 隐藏层之间的结点是有连接的，隐藏层的输入不仅包括输入层的输出，还包含上一时刻隐藏层的输出。
- 对于长期依赖问题，虽然理论上可以解决，但是实践中学习这些知识代价过大，如果序列过长会导致优化时出现梯度消失的问题。

#### Long Short Term Memory Network（LSTM）
- 结构：与RNN类似，但是重复的模块有不同的结构。
- 不同于单一神经网络层，这里是有四个，以一种非常特殊的方式进行交互。LSTM是一种拥有三个“门”结构的特殊网络结构。
- 输入门，遗忘门，输出门: 符合规则的信息会被留下，不符合的信息会被遗忘，以此原理，可以解决神经网络中长序列依赖问题。


#### Gated Recurrent Unit (GRU)
- 结构: 可以看成是LSTM的变种，GRU把LSTM中的遗忘门和输入门用更新门来替代。


#### Differentiable Neural Computer (DNC)
- 相当于对LSTM添加一个外存储器，提高LSTM的记忆遗忘的问题。


#### LSTM cell setup
- dim: [样本数量，时间步长，特征数量]： 3 dimensions (N, W, F) where N is the number of training sequences, W is the sequence length and F is the number of features of each sequence.

```python
for index in range(len(x) - self.seq_len - self.pred_len):
    _x = x[index: index + self.seq_len]
    # alpha-mind already shift 1-period for y
    _y = y[index + self.seq_len: index + self.seq_len + self.pred_len]
    if normalize:
        _x = normalise_windows(_x)
        _y = np.average(normalise_windows(_y))
    train_x.append(_x)
    # y data is already shifted to future(by alpha-mind)
    train_y.append(_y)
```



### Tensoflow implementation

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

- [深度学习笔记——RNN（LSTM、GRU、双向RNN）学习总结 - CSDN博客](https://blog.csdn.net/mpk_no1/article/details/72875185)
- [浅析至强RNN可微分神经计算机(DNC)](https://zhuanlan.zhihu.com/p/27773709)
- [Copper Price Forecast](https://github.com/liyinwei/copper_price_forecast/blob/master/lstm/core/co_lstm_predict_day.py)
- [TensorFlow中RNN实现的正确打开方式](https://zhuanlan.zhihu.com/p/28196873)