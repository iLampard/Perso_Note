# Q-learning

### Q-learning Algorithms
#### Q matrix
"Q", to the brain of our agent, representing the memory of what the agent has learned through experience.  The rows of matrix Q represent the current state of the agent, and the columns represent the possible actions leading to the next state

#### Transition

Q(state, action) = R(state, action) + Gamma * Max[Q(next state, all actions)]
![](https://upload-images.jianshu.io/upload_images/1825077-07cb6c09b13a00b6.png?imageMogr2/auto-orient/)
- 当前环境下某位置的价值Q可以通过原来的Q和下一步能走的位置的最大值之间进行计算后训练获得。

### Deep Q-learning
- 将状态和动作当成神经网络的输入, 然后经过神经网络分析后得到动作的 Q 值, 这样就没必要在表格中记录 Q 值, 而是直接使用神经网络生成 Q 值。
- 也可以只输入状态值, 输出所有的动作值, 然后按照 Q learning 的原则, 直接选择拥有最大值的动作当做下一步要做的动作. 


### Reference
- [A painless q learning tutorial](http://mnemstudio.org/path-finding-q-learning-tutorial.htm)
- [小例子-强化学习](https://morvanzhou.github.io/tutorials/machine-learning/reinforcement-learning/2-1-general-rl/ks)
- [强化学习之q-learning](https://www.jianshu.com/p/1c0d5e83b066)