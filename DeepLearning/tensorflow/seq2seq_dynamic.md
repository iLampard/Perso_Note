# Seq2Seq forward-only encoder / decoder without attention.

### 网络结构

本文使用如下机器翻译的流程结构

![](DeepLearning/../translation.png)

- 长方形表示 encoder 和  decoder 单元结构。
- encoder接收到序列[A, B, C]作为输入。
- 我们不关心encoder的输出，故不在图中显示。encoder的隐层最后输出到decoder。
- decoder的输入序列为[<EOS>, W, X, Y, Z]，训练后输出序列[W, X, Y, Z, <EOS>]。 


### Reference
- [Simple dynamic seq2seq with TensorFlow](https://github.com/ematvey/tensorflow-seq2seq-tutorials/blob/master/1-seq2seq.ipynb)