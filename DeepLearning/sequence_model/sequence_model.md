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






### Reference

- [深度学习笔记——RNN（LSTM、GRU、双向RNN）学习总结 - CSDN博客](https://blog.csdn.net/mpk_no1/article/details/72875185)
- [浅析至强RNN可微分神经计算机(DNC)](https://zhuanlan.zhihu.com/p/27773709)
- [Copper Price Forecast](https://github.com/liyinwei/copper_price_forecast/blob/master/lstm/core/co_lstm_predict_day.py)
