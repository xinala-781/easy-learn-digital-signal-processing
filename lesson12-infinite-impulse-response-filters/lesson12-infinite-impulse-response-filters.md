# 前言
本章节我们将会介绍到IIR滤波器的原理，以及与FIR滤波器的区别，最后在文末将会介绍到IIR滤波器的应用。
# IIR滤波器的定义和特点
## 定义
若输入信号为$x[n]$，输出信号为$y[n]$， IIR滤波器的系统函数一般表示为$$H(z)=\frac{\sum_{k = 0}^{M}b_{k}z^{-k}}{1-\sum_{k = 1}^{N}a_{k}z^{-k}}$$
其中\(z\)是复变量，$a_{k}$和$b_{k}$是滤波器系数，$M$和$N$分别是分子和分母多项式的阶数。

从时域角度来看，IIR滤波器的输出$y(n)$不仅取决于当前和过去的输入$x(n),x(n - 1),\cdots$，还取决于过去的输出$y(n - 1),y(n - 2),\cdots$。其差分方程一般形式为$$y(n)=\sum_{k = 0}^{M}b_{k}x(n - k)-\sum_{k = 1}^{N}a_{k}y(n - k)$$
## 特点
1. 无限脉冲响应：IIR滤波器的一个显著特点就是它具有无限长的脉冲响应。这是因为输出信号$y[n]$依赖于过去的输出信号$y[n - k]$，即使输入信号$x[n]$是一个有限的脉冲（如单位脉冲$\delta[n]$），由于这种递归关系，输出信号会在很长时间内持续受到影响，产生的脉冲响应会持续很长时间，不像FIR滤波器那样脉冲响应在有限时间内结束。
2. 反馈结构：它是一种反馈系统，通过将过去的输出信号反馈回来参与当前输出的计算（由$\sum_{k = 1}^{N}a_k y[n - k]$体现），这种反馈机制使得滤波器能够根据自身之前的输出情况对输入信号进行更精细的处理，但也使得其稳定性分析等方面相对复杂一些。
3. 计算复杂度与性能优势：在某些情况下，IIR滤波器可以用相对较少的系数实现与FIR滤波器类似的滤波效果，因为它可以利用反馈来反复调整输出。例如，要实现对某一频率的精确滤波，IIR滤波器可能只需要几个合适的系数设置，而FIR滤波器可能需要更多的系数（也就是更高的阶数）来达到同样的效果，所以在计算资源有限的情况下，IIR滤波器可能具有一定的优势。
## 与FIR滤波器的区别
1. 脉冲响应特性：如前面所述，IIR滤波器有无限脉冲响应，而FIR滤波器的脉冲响应是有限的。FIR滤波器的输出$y[n]$只取决于当前和过去的输入信号$x[n - k]$，不存在对过去输出信号的依赖，所以一旦输入脉冲结束，其脉冲响应也会很快结束。
2. 结构差异：FIR滤波器主要由乘法器、加法器和单位延迟等基本元件构成，是一种前馈结构，通过对输入信号的不同时刻样本进行加权求和得到输出。而IIR滤波器除了有类似的对输入信号的处理部分（由$\sum_{k = 0}^{M}b_k x[n - k]$体现），还增加了反馈部分（由$\sum_{k = 1}^{N}a_k y[n - k]$体现），其结构更加复杂。
3. 系数设置与滤波效果：FIR滤波器的滤波效果主要通过调整滤波器系数$b_k$来实现对不同频率成分的处理，而IIR滤波器不仅要调整与输入信号相关的系数$b_k$，还要调整与反馈相关的系数$a_k$，通过两者的协同作用来实现更复杂的滤波效果。例如，在实现相同的低频滤波效果时，FIR滤波器可能需要通过设置一系列的$b_k$系数来逐渐衰减高频成分，而IIR滤波器可能通过合理设置$a_k$和$b_k$系数，利用反馈机制更快地实现对低频的突出和对高频的抑制。
## 常系数差异方程式在IIR滤波器中的作用
常系数差异方程式$$\sum_{k = 0}^{N}a_k y[n - k]=\sum_{k = 0}^{M}b_k x[n - k]$$（其中$a_k$、$b_k$为常系数）,也就是差分方程的一般形式等于0时得到的方程。

它描述了IIR滤波器输入信号$x[n]$和输出信号$y[n]$之间的一种递归关系。通过这个方程式，我们可以看到输出信号$y[n]$不仅取决于当前和过去的输入信号$x[n - k]$，由$$\sum_{k = 0}^{M}b_k x[n - k]$$这部分体现，还取决于过去的输出信号$y[n - k]$，由$$\sum_{k = 1}^{N}a_k y[n - k]$$这部分体现。

这种递归关系使得IIR滤波器能够对输入信号进行更复杂的处理，它可以利用过去的输出信息来调整当前的输出，从而实现对不同频率成分的有选择性的增强或衰减等功能。例如，在处理音频信号时，如果想要突出某个频率范围的声音，就可以通过合理设置常系数差异方程式中的系数，让滤波器根据过去的输出情况以及当前和过去的输入情况来实现这样的效果。
# IIR滤波器的响应求解
## 脉冲响应
假设我们有一个具体的IIR滤波器定义为$$y[n]=0.8y[n - 1]+x[n]$$现在来计算它的脉冲响应。

当输入为单位脉冲$\delta[n]$时，输出为$h[n]$，根据给定的IIR滤波器公式，我们来逐步计算脉冲响应。
对于$n = 0$：
因为$\delta[0]=1$，且此时$y[-1]=0$（在初始时刻，过去的输出默认为$0$），所以$$h[0]=y[0]=0.8y[-1]+\delta[0]=0.8\times0 + 1 = 1$$
对于$n = 1$：
$$h[1]=y[1]=0.8y[0]+ \delta[1]$$
由于$\delta[1]=0$（单位脉冲在$n = 1$时为$0$），且$y[0]=1$（前面已计算出），所以$$h[1]=0.8\times1 + 0 = 0.8$$
对于$n = 2$：
$$h[2]=y[2]=0.8y[1]+ \delta[2]$$
同样$\delta[2]=0$，而$y[1]=0.8$（前面已计算出），所以$$h[2]=0.8\times0.8 + 0 = 0.64$$
以此类推，我们可以继续计算出后续时刻的脉冲响应值。

**结果分析：**

从计算结果可以看出，随着$n$的增加，脉冲响应的值在逐渐减小。这是因为在这个IIR滤波器中，每次输出都会受到上一次输出的影响，并且由于系数$0.8$的存在，使得这种影响是逐渐衰减的。

这种衰减的脉冲响应特性是IIR滤波器的一个典型特征，它反映了滤波器对输入脉冲的处理方式以及其内部的反馈机制在起作用。

通过分析脉冲响应，我们可以更好地了解IIR滤波器在不同时刻对输入脉冲的响应情况，进而理解其在处理其他类型输入信号时的特性。
## 阶跃响应
当输入为单位阶跃信号$u[n]$时，IIR滤波器的输出为阶跃响应，其计算公式为$$y[n]=\frac{b_0}{1 - a_1}(1 - a_1^n)u[n]（a_1\neq1）$$
通过一个实例来计算和分析阶跃响应的特点和变化规律:

假设我们有一个IIR滤波器的表达式为$$y[n]=0.5y[n - 1]+x[n]$$
这里$a_1 = 0.5$，$b_0 = 1$（假设）。
对于$n = 0$：
$$y[0]=\frac{b_0}{1 - a_1}(1 - a_1^0)u[0]$$
因为$u[0]=1$，所以$$y[0]=\frac{1}{1 - 0.5}(1 - 0.5^0)=\frac{1}{0.5}(1 - 1)=0$$
对于$n = 1$：
$$y[1]=\frac{b_0}{1 - a_1}(1 - a_1^1)u[1] ，u[1]=1$$，所以$$y[1]=\frac{1}{1 - 0.5}(1 - 0.5)=\frac{1}{0.5}\times0.5 = 1$$
对于$n = 2$：
$$y[2]=\frac{b_0}{1 - a_1}(1 - a_1^2)u[2]，u[2]=1$$，所以$$y[2]=\frac{1}{1 - 0.5}(1 - 0.5^2)=\frac{1}{0.5}\times0.75 = 1.5$$
以此类推，我们可以继续计算出后续时刻的阶跃响应值。

通过图形展示（这里假设可以简单地画出一个坐标图，横轴为$n$，纵轴为$y[n]$），我们可以看到阶跃响应的变化规律。最初，当$n = 0$时，$y[0]=0$，然后随着$n$的增加，阶跃响应的值逐渐上升，并且上升的速度逐渐减慢。这是因为随着$n$的增加，$a_1^n$的值在逐渐减小，但由于分母$1 - a_1$是固定的，所以整体上阶跃响应的值会逐渐趋近于一个极限值，在这个例子中，极限值为$$\frac{b_0}{1 - a_1}=\frac{1}{1 - 0.5}=2$$

**分析阶跃响应的特点：**

阶跃响应能够反映出IIR滤波器在面对持续输入（如单位阶跃信号这种从某个时刻开始一直保持不变的输入）时的动态变化情况。它的变化规律取决于滤波器的系数，不同的系数设置会导致不同的阶跃响应曲线。

例如，当$a_1$的值更接近$1$时，阶跃响应上升的速度会更慢，趋近极限值的过程也会更慢；而当$a_1$的值离$1$较远时，阶跃响应上升的速度会相对较快，趋近极限值的过程也会相对较快。

通过分析阶跃响应，我们可以更好地了解IIR滤波器在处理持续输入信号时的性能和特性。
# IIR滤波器在不同应用场景中的优势：
1. 资源利用效率：在一些对计算资源有限制的应用场景中，如在嵌入式设备上进行音频处理，IIR滤波器可能只需要相对较少的系数就能实现较好的滤波或效果模拟。
2. 对特定效果的模拟：IIR滤波器由于其反馈机制，能够很好地模拟一些具有递归性质的效果，如回音、混响等效果。通过合理设置系数，可以精准地控制这些效果的强度和衰减速度等参数。
3. 频率选择性：IIR滤波器可以通过调整系数来实现对不同频率成分的更精细的选择性处理。例如，在音频处理中，如果想要突出某个频率范围的声音或者抑制某个频率范围的声音，IIR滤波器可以通过设置合适的系数，利用反馈机制来实现这种频率选择性。