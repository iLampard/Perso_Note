# Convolution network 
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>


### 2-D convolution

#### Padding(扩充)
在图片外围补充一些像素点，把这些像素点初始化为0。
- Valid 卷积：不填充。
- Same 卷积：填充后，输出大小和输入 大小是一样的。
  
#### Stride(步长)
Stride表示过滤器在原图片中水平方向和垂直方向每次的步进长度。若stride=2，则表示filter每次步进长度为2，即隔一点移动一次。

#### Output size

对于一个 $$n \times n$$ 的图像，用 $$f \times f$$ 的过滤器做卷积
- 输出的大小 $$(n − f + 1) \times (n − f + 1)$$。
- Padding: 用 p 个像素填充边缘，输出的大小 $$(n + 2p − f + 1) \times (n + 2p − f + 1)$$。
- Padding + stride: 用 p 个像素填充边缘，步幅为s，那么输出的大小 $$((n + 2p − f)/s + 1) \times ((n + 2p − f)/s + 1)$$。




### 3-D Convolution
#### Chanel
原始图像数据的第三个维度通常叫做通道。卷积核中的通道数必须与原始图像的通道数一致。

#### Size of variables
不同的卷积核可以得到不同的一维特征图，把这些特征图堆叠到一起可以得到三维的输出立方体。

假设 layer l 是卷积层。过滤器宽度 $$f^l$$, padding $$p^l$$, 步长$$s^l$$, 过滤器的数量 $$n_c^l$$。
- input layer维度: $$ n_H^{l-1} * n_w^{l-1} * n_c^{l-1} $$
- output layer维度: $$n_H^l * n_w^l * n_c^l$$
    - $$n_H^l = (n_H^{l-1} + 2p^l - f^l) / s^l + 1$$
    - $$ n_w^l = (n_w^{l-1} + 2p^l - f^l) / s^l + 1 $$
  
- 过滤器维度: $$f^l * f^l * n_c^{l-1}$$
- 激活函数维度: $$a^l$$ same size with output layer
- weight维度: $$f^l * f^l * n_c^{l-1} * n_c^l$$。权重是所有过滤器的集合再乘以过滤器的总数量。
- bias维度: $$1 * 1 * 1 * n_c^l$$。每个过滤器都有一个偏差参数，它是一个实数。总的偏差集合要再乘以过滤器的总数量。


### Reference

- [卷积神经网络](http://www.ai-start.com/dl2017/html/lesson4-week1.html)