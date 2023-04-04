<div><a href="https://space.bilibili.com/383551518?spm_id_from=333.1007.0.0" style="text-decoration: none; color: rgba(7, 137, 224, 1)" target="_blank">博客配套视频链接: https://space.bilibili.com/383551518?spm_id_from=333.1007.0.0  b 站直接看</a></div>

<div><a href="https://github.com/nickchen121/Pre-training-language-model" style="text-decoration: none; color: rgba(7, 137, 224, 1)" target="_blank">配套 github 链接：https://github.com/nickchen121/Pre-training-language-model</a></div>

<div><a href="https://www.cnblogs.com/nickchen121/p/16470443.html" style="text-decoration: none; color: rgba(7, 137, 224, 1)" target="_blank">配套博客链接：https://www.cnblogs.com/nickchen121/p/15105048.html</a></div><br>

# Attention（注意力机制）

你会注意什么？

大数据（什么数据都有，重要的，不重要的）

对于重要的数据，我们要使用

对于不重要的数据，我们不太想使用

但是，对于一个模型而言（CNN、LSTM），很难决定什么重要，什么不重要

由此，注意力机制诞生了（有人发现了如何去在深度学习的模型上做注意力）

![img](https://imgmd.oss-cn-shanghai.aliyuncs.com/BERT_IMG/%E4%BA%BA%E7%B1%BB%E7%9A%84%E8%A7%86%E8%A7%89%E6%B3%A8%E6%84%8F%E5%8A%9B.jpg)

红色的是科学家们发现，如果给你一张这个图，你眼睛的重点会聚焦在红色区域

人--》看脸

文章看标题

段落看开头

后面的落款

这些红色区域可能包含更多的信息，更重要的信息



注意力机制：我们会把我们的焦点聚焦在比较重要的事物上



# 怎么做注意力

我（查询对象 Q），这张图（被查询对象 V）

我看这张图，第一眼，我就会去判断哪些东西对我而言更重要，哪些对我而言又更不重要（去计算 Q 和 V 里的事物的重要度）

重要度计算，其实是不是就是相似度计算（更接近），点乘其实是求内积（不要关心为什么可以）

Q，$K =k_1,k_2,\cdots,k_n$ ，我们一般使用点乘的方式

通过点乘的方法计算Q 和 K 里的每一个事物的相似度，就可以拿到 Q 和$k_1$的相似值$s_1$，Q 和$k_2$的相似值$s_2$，Q 和$k_n$的相似值 $s_n$

做一层 $softmax(s_1,s_2,\cdots,s_n)$ 就可以得到概率$(a_1,a_2,\cdots,a_n)$

进而就可以找出哪个对Q 而言更重要了

![img](https://imgmd.oss-cn-shanghai.aliyuncs.com/BERT_IMG/attention-%E8%AE%A1%E7%AE%97%E5%9B%BE.png)

我们还得进行一个汇总，当你使用 Q 查询结束了后，Q 已经失去了它的使用价值了，我们最终还是要拿到这张图片的，只不过现在的这张图片，它多了一些信息（多了于我而言更重要，更不重要的信息在这里）

$V = (v_1,v_2,\cdots,v_n)$

$  V'= (a_1,a_2,\cdots,a_n)*+(v_1,v_2,\cdots,v_n)=(a_1*v_1+a_2*v_2+\cdots+a_n*v_n)$

这样的话，就得到了一个新的 V'，这个新的 V' 就包含了，哪些更重要，哪些不重要的信息在里面，然后用 V' 代替 V 

一般 K=V，在 Transformer 里，K!=V 可不可以，可以的，但是 K 和 V 之间一定具有某种联系，这样的 QK 点乘才能指导 V 哪些重要，哪些不重要



### $softmax$ 的归一化

<img src="https://static.packt-cdn.com/products/9781800565791/graphics/Images/B16681_01_005.png" alt="img" style="zoom: 50%;" />

51， 49  ==> 0.51，0.49

80/8，20/8 ==> 0.9999999999， 0.0000000001

### 10 / 3 --> 0.9, 0.1

a1 和 a2 之间的差额越大，这个概率就越离谱