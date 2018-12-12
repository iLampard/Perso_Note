# Seq2seq model

### tensorflow函数介绍

#### tf.contrib.layers.embed_sequence

```python
embed_sequence(
    ids,
    vocab_size=None,
    embed_dim=None,
    unique=False,
    initializer=None,
    regularizer=None,
    trainable=True,
    scope=None,
    reuse=None
)

```

- ids: tensor with size [batch_size, doc_length] 
- return: tensor with size [batch_size, doc_length, embed_dim]


#### tf.strided_slice

```python
def strided_slice(input_,
                  begin,
                  end,
                  strides=None,
                  begin_mask=0,
                  end_mask=0,
                  ellipsis_mask=0,
                  new_axis_mask=0,
                  shrink_axis_mask=0,
                  var=None,
                  name=None)
```

##### 3-d tensor
三维数组的维度是从外向内计数的。

##### exmple

```python
input = [[[1, 1, 1], [2, 2, 2]],
        [[3, 3, 3], [4, 4, 4]],
        [[5, 5, 5], [6, 6, 6]]]
```

```python
res = tf.strided_slice(input, [0,0,0], [2,2,2], [1,2,1])
# 第一维度取 0，1 
# 第二维度取 0，因为步长为 2
# 第三维度取 0，1 
with tf.Session() as sess: 
       print(sess.run(x)) # [[[1,1],[3,3]]]
```

#### tf.contrib.seq2seq.TrainingHelper
```python
tf.contrib.seq2seq.TrainingHelper(inputs=decoder_embed_input,                                                                                sequence_length=target_sequence_length,                                                                    time_major=False)
```
这个函数不会把t-1阶段的输出作为t阶段的输入，而是把target中的真实值直接输入给RNN。返回helper对象，可以作为BasicDecoder函数的参数。
- inputs：对应Decoder框架图中的embedded_input，time_major=False的时候，inputs的shape就是[batch_size, sequence_length, embedding_size] ，time_major=True时，inputs的shape为[sequence_length, batch_size, embedding_size] 
- sequence_length：在源码中可以看出指的是当前batch中每个序列的长度(self._batch_size = array_ops.size(sequence_length))。
  
TrainingHelper用于train阶段，next_inputs方法一样也接收outputs与sample_ids，但是只是从初始化时的inputs返回下一时刻的输入。

#### tf.contrib.seq2seq.GreedyEmbeddingHelper
它和TrainingHelper的区别在于它会把t-1下的输出进行embedding后再输入给RNN。
- embedding：params argument for embedding_lookup，也就是 定义的embedding 变量传入即可。 
- start_tokens： batch中每个序列起始输入的token_id 
- end_token：序列终止的token_id



### Reference
- [seq2seq模型初探](https://www.jianshu.com/p/779e022a8644?hmsr=toutiao.io)