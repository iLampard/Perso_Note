# Bidirectional RNN


### Bidirectional Classifier

#### Input

输入数据为MNIST的图像数据，每个样本的尺寸为28*28。可以理解为一个时间序列。
- 样本的第一个维度：n_steps = 28 (图像的高)
- 样本的第二个维度：n_input = 28 (图像的宽)

因为是双向LSTM，有forward和backward两个LSTM cell，所以weight参数的数量也翻倍。

```python
n_input = 28
n_steps = 28
n_hidden = 256 # 隐藏节点数
n_classes = 10 

x = tf.placeholder('float', [None, n_steps, n_input])
y = tf.placeholder('float', [None, n_classes])

weights = tf.Variable(tf.random_normal([2*n_hidden, n_classes]))
biases = tf.Variable(tf.random_normal([n_classes]))


```

#### 网络结构

```python
def BiRNN(x, weights, biases):
    # 将第一个维度和第二个维度交换
    # x.shape = (n_steps, batch_size, n_input)
    x = tf.transpose(x, [1, 0, 2])

    # x.shape = (n_steps * batch_size, n_input)
    x = tf.reshape(x, [-1, n_input])

    # x.shape = n_steps * (batch_size, n_input)
    x = tf.split(x, n_steps)

    lstm_fw_cell = tf.contrib.rnn.BasicLSTMCell(n_hidden, forget_bias=1.0)

    lstm_bw_cell = tf.contrib.rnn.BasicLSTMCell(n_hidden, forget_bias=1.0)
    
    outputs, _, _ = tf.contrib.rnn.static_bidirectional_rnn(lstm_fw_cell, lstm_bw_cell, dtype=tf.float32)

    return tf.matmul(outputs[-1], weights) + biases

```


#### 损失函数

```python
pred = BiRNN(x, weights, biases)
cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(logits=pred, lables=y))
optimizer = tf.train.AdamOptimizer(learning_rate=learning_rate).minize(cost)
correct_pred = tf.equal(tf.argmax(pred, 1), tf.argmax(y, 1))
accuracy = tf.reduce_mean(correct_pred, tf.float32)


```

### Reference
- [Tensorflow实战-第七章]()