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


#### tf.nn.embedding_lookup

```python
embedding_lookup(
    params,
    ids,
    partition_strategy='mod',
    name=None,
    validate_indices=True,
    max_norm=None
)
```

tf.nn.embedding_lookup的作用就是找到要寻找的embedding data中的对应的（索引）行下的vector。
- params就是输入张量。
- id就是张量对应的索引。

![embedding_matrix](https://upload-images.jianshu.io/upload_images/1129359-51ec3c90c272f132.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/621/format/webp)

```python
import numpy as np
import tensorflow as tf
data = np.array([[[2],[1]],[[3],[4]],[[6],[7]]])
data = tf.convert_to_tensor(data)  # (3, 2, 1)
lk = [[0,1],[1,0],[0,0]]  # (3, 2)
lookup_data = tf.nn.embedding_lookup(data,lk) # (3, 2, 2, 1)： 第一个(3,2) 来自于lk; (2,1)来自于data 
init = tf.global_variables_initializer()
# lk[0]也就是[0,1]对应着下面sess.run(lookup_data)的结果恰好是把data中的[[2],[1]],[[3],[4]]
sess.run(lookup_data)
```
output
```python
array([[[[2],
         [1]],

        [[3],
         [4]]],


       [[[3],
         [4]],

        [[2],
         [1]]],


       [[[2],
         [1]],

        [[2],
         [1]]]])

```


详细例子可见[tf.nn.embedding_lookup记录](https://www.jianshu.com/p/abea0d9d2436)。

#### tf.contrib.seq2seq.dynamic_decode
```python
outputs, state, seq_len = tf.contrib.seq2seq.dynamic_decode(
                            decoder=decoder,
                            output_time_major=False,
                            impute_finished=True,
                            maximum_iterations=20)
```
outputs是一个namedtuple，里面包含两项(rnn_outputs, sample_id)
- rnn_outputs: [batch_size, decoder_targets_length, vocab_size], 保存decode每个时刻每个单词的概率，可以用来计算loss。
- sample_id： [batch_size], tf.int32，保存最终的编码结果。可以表示最后的答案。


### Reference
- [seq2seq模型初探](https://www.jianshu.com/p/779e022a8644?hmsr=toutiao.io)
- [tf.nn.embedding_lookup记录](https://www.jianshu.com/p/abea0d9d2436)