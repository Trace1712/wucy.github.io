---
layout:     post
title:      A Family of Robust Stochastic Operators for
Reinforcement Learning
subtitle:   summary
date:       2020-07-28
author:     wucy
header-img: img/yu.jpg
catalog: true
tags:
    - reinforcementlearning paper
---


# <center>论文笔记</center>

# 摘要

我们考虑了一类新的随机算子用于强化学习，以减轻负面影响，并对逼近或估计误差变得更稳健。建立了理论结果，证明了我们的算子族在随机意义下保持最优性并增加了作用间隙。经验结果说明我们的鲁棒随机算子的强大优势，显著优于经典的Bellman和最近提出的算子

# 准备工作

我们考虑一个标准RL框架，是一个智能体与一个随机环境交互。这种相互作用被建模为离散空间，离散时间折扣，MDP表示(X,A,P,R,&gamma;),X为状态集合，A是动作集合,P是核转移概率，R是状态-动作价值对上的奖励函数,&gamma;属于[0,1)。令Q表示为上有界实值函数集X x A,对于Q属于Q集合，定义V(x):=max<sub>a</sub>Q(x,a)和对近似值使用相同的定义。让x'表示下一个状态。。。

一个固定策略&pi;(.|x):X->A,定义给定当前状态的控制操作的分布x,当条件分布为每个状态呈现一个恒定的动作时，它就变成了一个确定性策略x。

贝尔曼等式为
$$
Q^{\pi}(x,a)=R(x,a)+\gamma E_pQ^\pi(x',\pi(x'))
$$
我们的目标是找到一个策略&pi;<sup>*</sup>可以达到最优化价值函数
$$
V^*(x):=sup_{\pi}V^{\pi}(x),\forall x\in X
$$
最优动作价值函数
$$
Q^*(x,a):=sup_{\pi}Q^{\pi}(x,a),\forall(x,a)\in X,A
$$
贝尔曼算子&tau;<sub>B</sub>:Q->Q被定义为
$$
\tau_BQ(x,a):=R(x,a)+\gamma E_PQ^{\pi}\max_{b\in A}(x',b)\tag{2}
$$
或
$$
\tau_BQ(x,a):=R(x,a)+\gamma E_PV(x')
$$
Bellman算子&tau;<sub>B</sub>是上确界范数上的压缩映射，其唯一不动点与最优作用值函数一致,即上上式或者等价于
$$
Q^*(x,a)=R(x,a)+\gamma E_pV^*(x')
$$
这反过来表明最优策略&pi;*可以通过
$$
\pi^*(x)=argmax_{a\in A}Q^*(x,a),\forall x\in X
$$
获得。当不存在近似/估计误差时，Bellman算子可以精确地求解MDP，但是在存在这些误差的情况下，先前所注意到的最优状态和次最优状态作用值函数之间的差异会导致对最优动作的错误识别。

为了解决实际中产生的近似/估计误差的这些和相关的非平稳效应，Bellemare 目的叫做固定贝尔曼算子，定义为
$$
\tau_CQ(x,a):=R(x,a)+\gamma E_p[l_{x\neq x'}\max _{b\in A}Q(x',b)+l_{x=x'}Q(x,a)]
$$
当l代表指示器功能。固定贝尔曼算子&tau;<sub>C</sub>通过重新定义动作值函数Q来保持局部形式的平稳性.

# 鲁棒随机操作

在这一节中，我们提出了我们的随机框架，其中包括提出一个一般的RSO家族。在随机意义下，给出了随机意义下保最优性和增加间隙概念的精确定义，证明了这类算子族的任何序列都是保最优性和增加间隙的。我们引入了一个新的算子族，并将焦点从一个确定性算子转移到一系列随机算子，这对w.r.t.提出的开放性问题具有重要意义。明确的，我们的结果展示了，最优的条件要弱的多和我们的操作员的统计效率可以大大提高。这两种方法都允许为不同的目的和应用寻找贝尔曼算子的替代品。同时，这些重要的改进完全改变和澄清了从[6]中提出的有限维参数优化问题到无限维空间（r.v.s.的无限序列）中寻找最大有效算子的问题，在此基础上，我们建立了随机算子序列间的随机性和可变性序导致了动作间隙之间的对应序。注意到，我们的方法可以被延申。需要注意的是，我们的方法可以扩展到Bellman操作符的变体，比如SARSA，策略评价，合理Q迭代，
$$
对于Q_0\in Q,x\in X,a\in A和序列\{\beta_k:k\in Z_+\}独立非负，期望\bar{\beta}_k:=E_\beta[\beta_k]在0和1之间，包括每个k∈Z+,我们定义\\ \tau_{\beta_k}Q_k(x,a):=R(x,a)+\gamma E_p{\underset{b\in A}{\operatorname{\max}}}Q_k(x',b)-\beta_k(V_k(x)-Q_k(x,a))\tag{4} \\
或者等价的\tau_{\beta_k}Q_k(x,a):=R (x,a)+\gamma E_pV(x')-\beta_k((V_k(x))-Q_k(x,a))
$$
(注意，上面操作跟贝尔曼算子等价当操作a是最优时或者&beta;<sub>k</sub>=0,因此在这些情况下使差项为零。)然后，一般RSO族的成员包括在序列{βk}中定义的序列{&tau;<sub>&beta;<sub>k</sub></sub>}，其中每个r.v.具有βk∈[0，1] (特别要注意的是，r.v.s βk对于每个k可以遵循不同的概率分布)。我们进一步将T定义为包含所有算子序列{T}，每个算子序列如（4）中所定义，因此存在{βk}序列，并且对于所有x∈x和a∈a，以下不等式成立
$$
上面的\tau为\tau_\beta^\cal{F},\\\rm \tau_{B}Q(x,a)-\beta_k(V_k(x))-Q_k(x,a)\le\tau Q(x,a)\le\tau_BQ(x,a)
$$
观察到，对于任何(x,a)在（4）中，当a不是最优动作，我们有V<sub>k</sub>>Q<sub>k</sub>(x,a)。经常发生（例如，对于许多k），导致Q（x，a）偏离V（x）更多；否则，对于Q(x,a)=V(x)，那么V<sub>k</sub>(x)>Q<sub>k</sub>(x,a)只会相对很少发生，因此不会影响V(x)的最终值。由于值函数V(x)没改变，但动作价值函数Q(x,a)确实改变了，者可能导致更大的动作间隔，并可能通过Q(x,a)的迭代更新来提供更有效的最终找到V(x)的方法。此外，我们观察到，V<sub>k</sub>(x)-Q<sub>k</sub>(x,a)前面的乘数β<sub>k</sub>需要单独较大，但其整体努力不应太大，从而影响V(x)的最终值。我们因此介绍RSOs家族，当&beta;<sub>k</sub>可以取任何值，只要它的平均值小于等于1。显然，这些情况比6定义的弱很多——它们是确定性的，并被限制在[0，1]，而我们的基于r.v.s β<sub>k</sub>的值可以远远超出[0，1)。由于r.v.sβk不需要完全相同分布（唯一的要求是βk在0和1之间），并且由于βk的实现可以具有远远超过或等于1的值，运算符 &tau;<sub>&beta;</sub><sup>F</sup>族显然包含了先前确定的确定性运算符族。
$$
定义3.1 一系列随机操作{\tau_k}对于M=(X,A,P,R,\gamma) 是随机意义上的最优储备吗如果\\对于Q_o\in Q,x\in X，\\Q_{k+1}:=\tau_kQ_k,V_K(x):=max_{a\in A}Q_k(x,a)收敛到\tilde{V}(x)当k\rightarrow \infty和所有a \in A,我们有\\
Q^*(x,a)<V^*(x)\Rightarrow \lim_{k\rightarrow\infty}supQ_k(x,a)<\tilde{V}(x)
$$

$$
定义3.2 一系列随机操作\{\tau_k\},M=(X,A,P,R,\gamma) 在随机系列中是间隙增大的，\\如果对于所有的Q_0\in Q,x\in X和a\in A,以下的不等式成立:\\
A(x,a):=\lim_{k\rightarrow\infty}inf[V_k(x)-Q_k(x,a)]\ge V^*(x)-Q^*(x,a)\dots(3)
$$

保最优性定义的性质本质上保证了a.s.至少一个最优动作保持最优，所有次优动作保持次优,当不等式6对至少一个严格a.s(x,a)时，间隔增加定义的性质意味着鲁棒性。特别是，当一个算子的作用间隙增大而保持最优保留时，最终结果对逼近/估计误差具有更大的鲁棒性。

接下来，我们将给出一个主要的理论结果，证明我们的一般rso族在随机意义下既保持了最优性，又增加了间隙。
$$
命题3.1 让\tau_B为(2)中定义的贝尔曼算子，{\tau_{\beta_k}}是4中定义的一系列RSOs。\\考虑到在样本路径上Q_o \in Q的r.v.s Q_{k+1}:=\tau_{\beta_k}Q_k序列，算子序列{T_{\beta_k}}在随机意义下既保持最优性，又增加间隙。\\此外，在随机意义下，\tau_{\beta}^{F}族中的所有算子都是保最优性和间隙递增的。
$$
即使&tau;<sub>&beta;</sub><sup>F</sup>中随机算子不是压缩映射，因此没有不动点

，命题3.1证明了所有&tau;<sub>&beta;</sub><sup>F</sup>中随机算子仍然是保存最优性的。而且，&tau;<sub>&beta;</sub><sup>F</sup>的定义和命题三显著得扩大了最优保存集合和间隙增大操作，超过了6中定义得确定性算子。特别地，我们在随机意义下保最优性算子的新充分条件意味着在不损失最优性的情况下，与Bellman算子的显著偏差是可能的；

