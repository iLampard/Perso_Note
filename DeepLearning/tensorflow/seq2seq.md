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
ivocab = ['the','like','between','did','just','national','day','country','under','such','second']

emb = np.array([[0.418, 0.24968, -0.41242, 0.1217, 0.34527, -0.044457, -0.49688, -0.17862],
   [0.36808, 0.20834, -0.22319, 0.046283, 0.20098, 0.27515, -0.77127, -0.76804],
   [0.7503, 0.71623, -0.27033, 0.20059, -0.17008, 0.68568, -0.061672, -0.054638],
   [0.042523, -0.21172, 0.044739, -0.19248, 0.26224, 0.0043991, -0.88195, 0.55184],
   [0.17698, 0.065221, 0.28548, -0.4243, 0.7499, -0.14892, -0.66786, 0.11788],
   [-1.1105, 0.94945, -0.17078, 0.93037, -0.2477, -0.70633, -0.8649, -0.56118],
   [0.11626, 0.53897, -0.39514, -0.26027, 0.57706, -0.79198, -0.88374, 0.30119],
   [-0.13531, 0.15485, -0.07309, 0.034013, -0.054457, -0.20541, -0.60086, -0.22407],
   [ 0.13721, -0.295, -0.05916, -0.59235, 0.02301, 0.21884, -0.34254, -0.70213],
   [ 0.61012, 0.33512, -0.53499, 0.36139, -0.39866, 0.70627, -0.18699, -0.77246 ],
   [ -0.29809, 0.28069, 0.087102, 0.54455, 0.70003, 0.44778, -0.72565, 0.62309 ]])


emb.shape
# (11, 8)

from collections import OrderedDict

# embedding as TF tensor (for now constant; could be tf.Variable() during training)
tf_embedding = tf.constant(emb, dtype=tf.float32)

# input for which we need the embedding
input_str = "like the country"

# build index based on our `vocabulary`
word_to_idx = OrderedDict({w:vocab.index(w) for w in input_str.split() if w in vocab})

# lookup in embedding matrix & return the vectors for the input words
tf.nn.embedding_lookup(tf_embedding, list(word_to_idx.values())).eval()

Out[58]: 
array([[ 0.36807999,  0.20834   , -0.22318999,  0.046283  ,  0.20097999,
         0.27515   , -0.77126998, -0.76804   ],
       [ 0.41800001,  0.24968   , -0.41242   ,  0.1217    ,  0.34527001,
        -0.044457  , -0.49687999, -0.17862   ],
       [-0.13530999,  0.15485001, -0.07309   ,  0.034013  , -0.054457  ,
        -0.20541   , -0.60086   , -0.22407   ]], dtype=float32)

```


详细例子可见[stackoverflow](https://stackoverflow.com/questions/34870614/what-does-tf-nn-embedding-lookup-function-do)。

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