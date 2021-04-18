
Wide & Deep是推荐系统的经典模型

原始论文中已经说得很清楚了，笔者在这里说一说自己对Wide & Deep的理解：

从模型内容Pooling的思路出发，如果Wide & Deep两个部分同时喂相同的数据，Wide & Deep是可以用残差网络解释的，并且从一定程度上说，Wide & Deep 自带残差网络。为什么？

F(x) = f(x) + x， f(x)可视为DNN（非线性），x可视为LR（线性）

从数学公式看，如果没有非线性激活函数，残差网络存在与否意义不大。如果残差网络存在，则只是做了简单的平移：

![image](https://user-images.githubusercontent.com/68730894/115150910-06d71280-a09d-11eb-85bf-9f21d34334e3.png)

增加非线性激活函数之后， 模型的特征表达能力大幅提升。这也是为什么Residual Block有2个权重（W_1,W_2）的原因。

![image](https://user-images.githubusercontent.com/68730894/115150929-23734a80-a09d-11eb-966b-2f7a3d417de3.png)

由此可见，残差连接的核心是：非线性特征的数据和线性特征的数据融合。

![image](https://user-images.githubusercontent.com/68730894/115150953-51588f00-a09d-11eb-80a7-12630fe431bd.png)

能实现非线性特征的方法有分段函数（比如：ReLU、MLR）、多项式（比如：x^2,√x,log x）、激活函数（ReLU、tanh、sigmoid、gelu、leakyrelu）、神经网络（FM、FFM、DNN、GRU、LSTM、CNN、Transformer、自定义神经网络W_2 σ(W_1 x)）等等。

![image](https://user-images.githubusercontent.com/68730894/115151002-88c73b80-a09d-11eb-9f5f-0546e85ee426.png)

线性特征则是不经过激活函数就行，维度能想办法转换和非线性特征的维度一致就行。因此笔者认为残差网络可以不被维度相等且对位相加所限制，可作为一种常用的Pooling策略，只要保证：上一层（或几层）之前的输出与本层计算的输出相加，维度可以根据需要调整一致即可。

工程上来说，该模块在原始论文中提到由特别强的记忆能力，笔者认为，这应该是是依靠LR强大的“评分卡”实现的，此外，该模块也赋予模型强大的可解释性能力

在不追求复杂模型那么一点点准确率的前提下，DNN部分作为LR的补充，兼具可解释性和高阶特征提取能力。
