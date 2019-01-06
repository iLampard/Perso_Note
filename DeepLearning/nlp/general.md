# General techniques in NLP

### Mask 矩阵的作用


#### 处理不定长输入
在NLP中，一个常见的问题是输入序列长度不等，而mask可以帮助我们处理。虽然RNN等模型可以处理不定长的input，但是在实践中，需要对input做batchize，转换成固定大小的tensor，方便矩阵操作，常见的shape如下：(seq_len, batch_size,
dim)。

假设 seq_len = 5，处理下面两个句子。

```bash
case 1: I like cats.
case 2: He does not like cats.
```
对case 1做pad处理，变成
```bash
I like cats <PAD> <PAD>
```

在上述例子数字编码后，开始做embedding，而pad也会有embedding向量，但pad本身没有实际意义，参与训练可能还是有害的。因此，有必要维护一个mask tensor来记录哪些是真实的value，上述例子的两个mask如下：
```bash
1 1 1 0 0
1 1 1 1 1
```
后续再梯度传播中，mask起到了过滤的作用，在pytorch中，有参数可以设置.


#### 语言模型中防止未来信息泄露

在语言模型中，常常需要从上一个词预测下一个词，而现阶段attention是标配，比如Transformer中的self attention，如果不做mask，在decoder的时候很容易把下一个词的信息泄露了，即按上述例子，不能在预测like这个词时已经知道like后面的词了。

一个常见的trick就是生成一个mask对角矩阵。

```python
def subsequent_mask(size):
    "Mask out subsequent positions."
    attn_shape = (1, size, size)
    subsequent_mask = np.triu(np.ones(attn_shape), k=1).astype('uint8')
    return torch.from_numpy(subsequent_mask) == 0
```
