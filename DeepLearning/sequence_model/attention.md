# Attention model 


### 基本原理

#### 与传统Encoder/Decoder模型比较
以机器翻译来作为例子。当给出一句‘我爱你’(sorce)中文，要将它翻译成英文‘I love you’(target)时，利用现在深度学习最为流行的model--encoder to decoder，‘我爱你’被编码(这里指语义编码)成$$C$$，然后在经过非线性函数$g$来decoder得到目标Target中的每一个单词$$y_1$$,$y2$,$y3$。计算如下： 

$$C = f(x_1,x_2,x_3)$$ 

$$y_1 = g(C)$$ 

$$y_2 = g(C,y_1)$$ 

$$y_3 = g(C,y_1,y_2)$$ 


### Reference
- [真正的完全图解Seq2Seq Attention模型](https://zhuanlan.zhihu.com/p/40920384)
- [浅谈Attention机制的理解](https://zhuanlan.zhihu.com/p/35571412)