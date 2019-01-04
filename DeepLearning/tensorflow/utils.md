# Utility functions in Tensorflow


#### reduction_index

##### reduce_sum
```python
# 'x' is [[1, 1, 1]
#         [1, 1, 1]]
tf.reduce_sum(x) ==> 6
tf.reduce_sum(x, 0) ==> [2, 2, 2]
tf.reduce_sum(x, 1) ==> [3, 3]
tf.reduce_sum(x, 1, keep_dims=True) ==> [[3], [3]]
tf.reduce_sum(x, [0, 1]) ==> 6
```

##### reduce_mean

```python
x = tf.constant([[1., 1.],
                 [2., 2.]])
tf.reduce_mean(x)  # 1.5
tf.reduce_mean(x, axis=0)  # [1.5, 1.5]
tf.reduce_mean(x, 1)  # [1.,  2.]

xx = tf.constant([[[1., 1, 1],
                   [2., 2, 2]],

                  [[3, 3, 3],
                   [4, 4, 4]]])
tf.reduce_mean(xx, [0, 1]) # [2.5 2.5 2.5]

```

#### Axis

##### concat

2-dimentional case

```python
t1 = [[1, 2, 3], [4, 5, 6]]
t2 = [[7, 8, 9], [10, 11, 12]]
tf.concat([t1, t2], axis=0) == > [[1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12]]

t1 = [[1, 2, 3], [4, 5, 6]]
t2 = [[7, 8, 9], [10, 11, 12]]
tf.concat([t1, t2], axis=1) ==> [[1, 2, 3, 7, 8, 9], [4, 5, 6, 10, 11, 12]]

```

3-dimentional case
```python
t1 = [[[1,2,3], [4,5,6]], [[4,5,6],[7,8,9]]] # shape (2, 2, 3)
t2 = [[[1,2,3], [4,5,6]], [[4,5,6],[7,8,9]]]
tf.concat([t1, t2], axis=0) =>  shape (4, 2, 3)

[[[1, 2, 3],
[4, 5, 6]],

[[4, 5, 6],
[7, 8, 9]],

[[1, 2, 3],
[4, 5, 6]],

[[4, 5, 6],
[7, 8, 9]]]
        
```