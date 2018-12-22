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

### Encode / Decode

对于RNN语言模型，本质上是计算在给定输入F下输出E的条件概率P(E|F)。 在计算输出序列E的概率时，先令一个RNN处理源序列F，来计算语言模型的初始状态。

encoder-decoder的含义是：通过第一个神经网络来“编码”F的信息到一个隐层状态，在使用第二个神经网络来预测E“解码”该隐层到输出序列。
- encoder层的处理单元是RNN(f)，decoder层是RNN(e)，对decoder层的输出采用softmax来获得时刻t输出该隐层的概率。
- encoder和decoder的区别在于：encoder的隐层初始值是零，而decoder层的初始值是encoder层的最终输出向量，这意味着decoder层获得了源序列的所有语义信息。
- 在每次decoder出一个et时，我们将其embedding和输出hidden state连结输入到下一个RNN单元，直到解码到句尾标记。（hidden state经过softmax后得到的概率向量的维度为词典维，类似于one-hot representation，每一维代表词典中的一个词，概率最高即为预测得到的词）。

![encoder_example](https://pic3.zhimg.com/80/v2-cf0fb9418d3ca6894d729a93b3d08746_hd.png
)




### Reference

- [深度学习笔记——RNN（LSTM、GRU、双向RNN）学习总结 - CSDN博客](https://blog.csdn.net/mpk_no1/article/details/72875185)
- [浅析至强RNN可微分神经计算机(DNC)](https://zhuanlan.zhihu.com/p/27773709)
- [Copper Price Forecast](https://github.com/liyinwei/copper_price_forecast/blob/master/lstm/core/co_lstm_predict_day.py)
