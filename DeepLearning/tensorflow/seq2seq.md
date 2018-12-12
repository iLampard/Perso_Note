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