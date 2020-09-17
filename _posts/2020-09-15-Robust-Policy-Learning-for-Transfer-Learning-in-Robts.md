---
layout:     post
title:      Robust Policy Learning for Transfer Learning in Robts
subtitle:   summary
date:       2020-09-15
author:     wucy
header-img: img/yu.jpg
catalog: true
tags:
    - reinforcementlearning paper


---


# <center>论文笔记</center>

# 介绍

强化学习代理已被广泛应用于各种任务，如游戏和机器人控制。agent通过与环境的交互，尝试通过试错来学习策略和值函数，直到收敛到一个最优策略。鲁棒性和稳定性在强化学习中是非常重要的；然而，神经网络容易受到来自意外来源的噪声的影响，并且不太可能抵抗干扰。此外，无论何时使用强化学习代理来训练机器人，它们通常要么用手工制定的策略初始化，要么用模拟代理的策略初始化。然而，模拟的智能体可能无法捕捉到真实世界机器人的所有噪音。因此，对于机器人的快速收敛来说，迁移学习可能不够有效。在本文中，我们评估了两种鲁棒强化学习算法，并与文中进一步描述的香草算法和朴素算法进行了比较。本文的主要思想是在agent训练过程中，我们可以使用对抗性的例子来选择扰动。本文尝试在OpenAI-Gym和MujoCo环境下实现具有扰动的鲁棒强化学习算法。我们使用OpenAI Gym的Reacher-v2环境和MujoCo后端进行评估。

# 结论

基于Reacher环境下不同鲁棒算法的评价结果，我们可以对它们的性能得出一些结论。

1.扰动&epsilon;=0.1

如图3所示，在较低的扰动频率下，所有四种算法都表现得相当好。随着扰动频率的增加，香草算法和朴素算法的性能会下降。相比之下，ARPL和RARL的降解程度要小得多。这表明，与香草和天真相比，ARPL和RARL对噪声的鲁棒性相当强。该图的另一个奇特之处在于，ARPL在较小频率下的性能略优于RARL，但RARL在较高的扰动频率下表现更好。这是因为在训练时，RARL在训练时总是有干扰（对手代理），而在ARPL中，我们有课程学习，其中扰动的频率随着训练时的事件数而不断增加。在没有扰动的情况下，RARL的表现比其他的差。这是因为，RARL会因为采取不必要的行动来对抗不存在的干扰而获得更高的惩罚（更高的负回报）。此外，在这种情况下，ARPL比RARL做得更好，因为ARPL代理在状态中包含扰动，而RARL代理在训练时有对手干扰其行为。因此，RARL代理假设对手代理在任何时候都存在，并且即使不存在，也会对其影响进行过度补偿。

2.扰动&epsilon;=0.01

如图4所示，在较低的频率下，我们在所有四个方面的性能几乎相当算法频率增加了香草算法和朴素算法的性能下降。在ARPL中，随着扰动频率的增加，性能几乎不变。随着扰动频率的增加，RARL的性能略有提高。这为RARL和ARPL的健壮性提供了证据。然而，由于前一小节所述的原因，当没有扰动时，RARL的性能比其他3种算法差。在这种情况下，扰动强度ε仅为前一种情况的十分之一。因此，在这种情况下，所有4种算法的性能通常比前一种情况下要好。

最后的结论是，ARPL和RARL都对噪声具有鲁棒性。当我们知道噪声发生的频率和系统中的噪声强度为很高。但是当我们知道噪声发生的频率和在系统中的强度很低时，ARPL工作得更好（与RARL相比）。

# 未来的工作

未来的工作包括建立一个模型，我们将ARPL和RARL结合起来。在这个模型中，我们扰动状态，就像在ARPL中所做的那样，并用一个对手代理来扰动动作，就像在RARL中一样。这种双重扰动可能包括ARPL和RARL的优点。

# 实验

在我们所有的实验中，我们使用TRPO作为基本算法，在此基础上我们构建了各种鲁棒算法（ARPL，RARL和Naive）。三参数化算法是一个参数化的参数化网络。动作空间是连续的。我们假设策略中动作的分布是高斯分布。在我们的例子中，策略网络预测高斯分布的平均值和标准差。当从策略中抽样操作时，这些高斯分布的平均值和标准偏差由网络策略和价值网络在以下配置的所有实验中保持不变。

在Reacher状态下，不只是角度，而是指定角度的余弦和正弦。这是为了避免在试图表示其主域中的角度时出现的不连续性

# 实验结果

四种不同方法的学习曲线。对于ARPL，奖励在200幕之后达到饱和，然后在600幕之后略有下降。这是因为训练时采用的课程学习方法。1. 扰动的频率增加了，但900幕之后，奖励确实又增加了。在RARL中，由于对手代理的存在，即使在初始阶段，报酬的饱和比其他三种算法都慢。另外，Naive网络和Vanilla网络几乎有相同的学习曲线。我们评估了Vanilla，Navie，ARPL和RARL在不同的扰动频率（或每个时间步的扰动概率）和两种不同的扰动强度（0.1,0.01）下的性能。在评估时，我们对MujoCo环境中的状态进行如下扰动：
$$
s_t=s_t+\epsilon*x
$$
伯努力概率&varphi;,x~N(0,1),在每一步。&epsilon;代表了状态中扰动的力量和&varphi;代表了扰动增加的概率。这些实验是在1000集上进行的，它们的轨迹是通过训练4个代理后获得的策略（来自策略网络）进行采样的。计算了这1000集获得的总回报的平均值和标准差。这些都是以前以表格的形式与相应的图表一起呈现的。

# ARPL

ARPL，Mandlekar等人，是一种在模拟器上训练策略梯度算法的同时对状态产生附加对抗性扰动的算法。这些由ARPL产生的扰动旨在捕获系统动态噪声和过程噪声中的3噪声。动态噪声是测量物理量（如质量、摩擦力等）时的误差。动态噪声影响状态，但只能通过捕捉系统动态的函数来实现。过程噪声是直接增加到状态的噪声。

ARPL用一个基于扰动技术的梯度来增加对抗的扰动到这个状态中。过去有一些关于基于梯度的扰动的研究。Goodfellow等人提出的快速梯度符号法（FGSM）是对抗性扰动最重要的贡献之一。FGSM关注于输入到深度学习模型的图像的逆扰动，其中每个像素的变化由定义的损失函数η上的梯度符号决定，并受因子ε的限制。FGSM产生的扰动可以推广到策略梯度算法，其中它的公式是，
$$
\delta=\varepsilon sign(\nabla_s\eta(\pi_{\theta}))（FGSM）
$$
其中η是由策略网络参数θ参数化的策略π定义的损失函数，ε是表示扰动强度的超参数。FGSM的这种思想在训练深度学习模型时更适合于图像。ARPL将基于梯度的状态扰动的思想扩展到策略梯度算法。

FGSM的这种思想在训练深度学习模型时更适合于图像。ARPL将基于梯度的状态扰动的思想扩展到策略梯度算法。在ARPL，扰动被计算为
$$
\delta=\varepsilon \nabla_{s}\eta(\pi_{\theta})(APRL)
$$
&epsilon;是表示扰动强度的超参数，η是扰动策略π上的损失函数。注意，该δ与TRPO中的δ不同。策略π由策略网络参数进行参数化&theta;。在策略上定义的这个损失函数必须是这样的，对状态的附加梯度扰动通常会鼓励代理在考虑系统中存在的噪声的情况下采取一些纠正措施。注意，这个损失函数不同于策略梯度损失和价值网络损失。我们将描述我们在实验和超参数设置中使用的具体损耗函数第节在一个事件中的每一个时间步，我们用一个概率φ（一个超参数）来扰动状态。
$$
\begin{align}
&\bold {Data}:hyperparameter:\phi,\epsilon,k\\
&Initialize \ \theta_0\\
&\bold{for} \ i=0,1,2,...,max_iter \  do\\
& \ \bold{for} \ \bold{each} \ t=1,...,T_k,\forall k\ do\\
& \  \ 轨迹采样 \pi(\theta_i):\tau_{ik}=\{s_t,a_t\}^{T_{k-1}}_{t=0}\\
& \  \ 计算扰动项 \delta=\epsilon(\nabla_{s_t}\eta(\pi_{\theta_i}))\\
& \  \ 增加扰动(\delta)用伯努力概率分布（\phi）\\
& \ \theta_{i+1}\leftarrow policy\_grad(\theta_i,\{\tau_{i1}...\tau_{ik}\})更新TRPO\\
& \ 用TD误差庚新参数
\end{align}
$$

# RARL

在RARL中，我们有两个智能体：主智能体：努力提高表现的人和对手，对手，试图破坏表现。这些智能体玩着0和游戏。

这种对抗训练帮助主人公探员探索更严峻的情况，从而使其更能适应噪音环境。两者都有主角和对手从环境中得到反馈，并根据观察结果做出行动。不同的是，对手采取的行动试图让主人公在任务中失败，并使其获得较低的回报。这与Goodfellow等人[6]在生成性对抗网络（GAN）中使用的方法非常相似，其中网络：生成器网络鉴别器网络在零和博弈中相互竞争。

在RARL中，有两个策略网络，一个用于主角，另一个用于对手，分别生成策略π<sub>p</sub>和π<sub>a</sub>。同样，有两个价值网络分别用来预测主角和对手的价值函数。同样，有两个价值网络分别用来预测主角和对手的价值函数。现在采取的行动取样的轨迹是影响双方的主角和对手。精确的数学公式将在下一节中描述。

**更新规则**

主策略&pi;和对抗策略&pi;<sub>a</sub>都参与了一个零和博弈。主人公和对手的行为都会影响下一个状态。算法对轨迹采样所采取的操作如下所示
$$
a=a_{protagonist}+D*a_{adversary}
$$
D是其中D是一个标量超参数，称为难度等级。在这里，这个参数'D'被设置为限制对抗性代理的动作幅度。这是为了确保学习不会变得不稳定。

期待奖励Q(s,a)是积累折扣奖励
$$
Q(s_k,a_k)=E(\sum_{t=k}^T\gamma^{t-k}*(r(s_t,a_t)))=E(r+\gamma(s_t,a_t))=E(r+\gamma*V(\bar{s}))
$$
优势函数A(s,a)是指预期报酬与预计报酬之间的差额：
$$
A(s,a)=Q(s,a)-V(s)
$$
在零和游戏中，我们有
$$
r_{proragonist}=-r_{proragonist}=r，参数这样更新
$$

$$
\theta_p \leftarrow\theta_p+\alpha*A_{protagonist}*\nabla_{\theta_p}log(\pi_p)\\
\theta_a \leftarrow\theta_\alpha+\alpha*A_{adversary}*\nabla_{\theta_a}log(\pi_a)\\
A_{protagonist}是Q（s,a）和网络预测值的差。
$$

折现收益率由当前轨迹的报酬与价值网络预测的下一状态值的折现期望值之和来近似。同样的道理也适用于对手，但在这里奖励被否定，因此折扣回报也被否定。

对于TRPO，优势是广义估计，而不是普通优势
$$
A^{gen}_t=A_t + \gamma*\tau*A^{gen}_{t+1}
$$

$$
\begin{align}
&算法3\  RARL\\
&1.Data:超参数：D\\
&2.初始化\theta_p 和\theta_a对于策略网络\\
&3.初始化w_p和w_a\\
&for\ i = 0,1,2... max_iter do:\\
& \ \ perform \ k-batch\\
& \ \ for \ t =1,2,3...max_timesteps\\
& \ \ \ \ \ a = a_{protagonist}+D*a_{adversary}\\
& \ 用TRPO 更新\theta_p和\theta_a with\ respective \ R和\\
& \ 更新w_P和w_a

\end{align}
$$

# Naive算法和Vanilla模型

在Vanilla模型中，我们在训练时不会扰动状态。本质上，代理人不知道关于扰动的任何事情。在朴素算法中，我们在训练时随机扰动代理的状态，如下所示：
$$
s_t\leftarrow s_t+\alpha*x \\
\alpha是一个超参数，x服从N（ 0,1）是一个随机数从高斯分布中。
$$
注意，ARPL、RARL和Naive算法是Vanilla TRPO算法的补充。因此，它们可以推广到任何其他策略梯度算法。

## ARPL

在ARPL算法中，我们训练智能体在扰动状态下。这个扰动是策略基础的
$$
\delta=\epsilon \nabla_s\eta(\pi_{\theta})
$$
训练ARPL智能体，我们用&eta;(&pi;<sub>&theta;</sub>)，作为预测的平均向量的L2范数的平方策略网络。如果&mu;=[&mu;1,&mu;2]是平均向量通过策略网络的预测。我们用||&mu;||^2作为A的损失函数。这种损失背后的直觉是，受该损失正梯度扰动的状态将导致策略网络预测的平均值更高。因此，通过用这种方法扰动状态，我们欺骗代理相信扰动状态与先前的未扰动状态以相同的动作给予相同的回报。因此，鼓励代理人采取更高价值的行动。

我们使用ε=0.1作为微扰强度。同时，我们采用课程学习的方法来增加φ，增加扰动的概率。我们在1000次迭代中将φ从0.0变为0.1，每100次迭代增加一次。这样做是为了在训练。为了训练朴素算法我们使用了一个常数概率φ=1.0。这里扰动强度为ε=0.1。

## RARL

在RARL中，主角和对手都有相同的策略和价值网络配置（如上所述）。训练时，为了保持稳定性，难度等级“D”设为0.1。为了提高稳定性，我们只为前50次迭代训练主角。然后，从第50次迭代开始，在每次迭代中一起训练主角和对手。