# Neutral network snippets


### 简单神经网络的搭建
 
定义添加神经层的函数
- 训练的数据
- 定义节点准备接收数据
- 定义神经层：隐藏层和预测层
- 定义 loss 表达式
- 选择 optimizer 使 loss 达到最小

然后对所有变量进行初始化，通过 sess.run optimizer，迭代若干次进行学习



```python 
import tensorflow as tf
import numpy as np

# 添加层
# inputs / outputs 都是tf.placeholder
# in_size 是输入特征的维度
def add_layer(inputs, in_size, out_size, activation_function=None):
   # add one more layer and return the output of this layer
   Weights = tf.Variable(tf.random_normal([in_size, out_size]))
   biases = tf.Variable(tf.zeros([1, out_size]) + 0.1)
   Wx_plus_b = tf.matmul(inputs, Weights) + biases
   if activation_function is None:
       outputs = Wx_plus_b
   else:
       outputs = activation_function(Wx_plus_b)
   return outputs

```


```python
# 1.训练的数据
# Make up some real data 
x_data = np.linspace(-1,1,300)[:, np.newaxis] # shape (300, 1)
noise = np.random.normal(0, 0.05, x_data.shape)
y_data = np.square(x_data) - 0.5 + noise

# 2.定义节点准备接收数据
# define placeholder for inputs to network  
xs = tf.placeholder(tf.float32, [None, 1])
ys = tf.placeholder(tf.float32, [None, 1])

# 3.定义神经层：隐藏层和预测层
# add hidden layer 输入值是 xs - tf.placeholder，在隐藏层有 10 个神经元   
l1 = add_layer(xs, 1, 10, activation_function=tf.nn.relu)
# add output layer 输入值是隐藏层 l1，在预测层输出 1 个结果
prediction = add_layer(l1, 10, 1, activation_function=None)

# 4.定义 loss 表达式
# the error between prediciton and real data    
loss = tf.reduce_mean(tf.reduce_sum(tf.square(ys - prediction),
                    reduction_indices=[1])) # 还是要用ys - tf.placeholder

# 5.选择 optimizer 使 loss 达到最小               
# 这一行定义了用什么方式去减少 loss，学习率是 0.1     
train_step = tf.train.GradientDescentOptimizer(0.1).minimize(loss)


# important step 对所有变量进行初始化
init = tf.global_variables_initializer()
sess = tf.Session()
# 上面定义的都没有运算，直到 sess.run 才会开始运算
sess.run(init)

# 迭代 1000 次学习，sess.run optimizer
for i in range(1000):
   # training train_step 和 loss 都是由 placeholder 定义的运算，所以这里要用 feed 传入参数
   sess.run(train_step, feed_dict={xs: x_data, ys: y_data})
   if i % 50 == 0:
       # to see the step improvement
       print(sess.run(loss, feed_dict={xs: x_data, ys: y_data}))
```


### 卷积神经网络

#### Strides
在二维卷积函数tf.nn.conv2d()，最大池化函数tf.nn.max_pool()，平均池化函数tf.nn.avg_pool()中，卷积核的移动步长都需要制定一个参数strides。如果strides=[b,h,w,c]，其中strides[0]和strides[3]默认为1。
- b表示在样本上的步长默认为1，也就是每一个样本都会进行运算。
- h表示在高度上的默认移动步长为1，这个可以自己设定，根据网络的结构合理调节。
- w表示在宽度上的默认移动步长为1，这个同上可以自己设定。
- c表示在通道上的默认移动步长为1，这个表示每一个通道都会进行运算。

该解释同样应用于卷积核参数*kisze*。
```python
# 池化的核函数大小为2x2，因此ksize=[1,2,2,1]，步长为2，因此strides=[1,2,2,1]
def max_poo_2x2(x): 
	return tf.nn.max_pool(x,ksize=[1,2,2,1],strides=[1,2,2,1])
```

#### SAME vs VALID
- padding='SAME': 输出大小等于输入大小除以步长向上取整，s是步长大小.
- padding='VALID': 输出大小等于输入大小减去滤波器大小加上1，最后再除以步长（f为滤波器的大小，s是步长大小）。

#### reduction_index

```python
# 'x' is [[1, 1, 1]
#         [1, 1, 1]]
tf.reduce_sum(x) ==> 6
tf.reduce_sum(x, 0) ==> [2, 2, 2]
tf.reduce_sum(x, 1) ==> [3, 3]
tf.reduce_sum(x, 1, keep_dims=True) ==> [[3], [3]]
tf.reduce_sum(x, [0, 1]) ==> 6
```


### Reference
- [一文学会用 Tensorflow 搭建神经网络](http://www.jianshu.com/p/e112012a4b2d)
- [TensorFlow中padding卷积的两种方式](https://blog.csdn.net/syyyy712/article/details/80272071)