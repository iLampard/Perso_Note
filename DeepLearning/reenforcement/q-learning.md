# Q-learning

### Algorithms
#### Q matrix
"Q", to the brain of our agent, representing the memory of what the agent has learned through experience.  The rows of matrix Q represent the current state of the agent, and the columns represent the possible actions leading to the next state

#### Transition

Q(state, action) = R(state, action) + Gamma * Max[Q(next state, all actions)]
![](https://upload-images.jianshu.io/upload_images/1825077-07cb6c09b13a00b6.png?imageMogr2/auto-orient/)
- 当前环境下某位置的价值Q可以通过原来的Q和下一步能走的位置的最大值之间进行计算后训练获得。

### Reference
- [A painless q learning tutorial](http://mnemstudio.org/path-finding-q-learning-tutorial.htm)
- [强化学习之q-learning](https://www.jianshu.com/p/1c0d5e83b066)