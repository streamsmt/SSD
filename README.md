# SSD工具包说明
by renesmt

##压缩感知问题简介
作为一个MATLAB工具包的说明,只对压缩感知问题的形式作一个简要的介绍.
在信息科学中会碰到这样的问题:我们要观测的信号是*稀疏*的,也就是说在某个基的表示中,信号$x_0$的大部分分量都等于0.此时,如果再用一个满秩方阵$D_{n\times n}$对其进行观测:$D_{n\times n}x_{0_{n\times 1}}=y_{n\times 1}$,不免显得有些笨重.
陶哲轩和Candes等人的工作指出,可以用非完备矩阵进行观测,即$D_{m\times n}x_{0_{n\times 1}}=y_{m\times 1}$($m<n$),在矩阵满足RIP性质,并且信号满足稀疏性时,可以由$D$和$y$完整地反推出$x_0$.
当然,RIP性质较难满足;这也是我们设计SSD算法的原因.我们相信它在实际的应用场景中具有更强的*鲁棒性*(robustness).



## 代码说明
SSD算法的详细介绍在我们的[论文][1]中.
`ssd.m`是主程序,`guidance_convex.m`使用*Dual Ascent*优化方法求得欧几里得最短解:目前来看是最快的一种方法.
将这两者分开是为了应对未来可能的改动.
Shortest-Solution-guided Decimation算法,以欠定线性方程组$Dx=y$解空间中的欧几里得最短解作为索引,由$D$和$y$反推出原始稀疏信号$x_0$.
作为例子,可以在MATLAB中产生2000\*10000的矩阵`D`以及10000\*1的原始稀疏(稀疏度=0.05)信号`x_0`:

    D = randn(2000,10000);
    x_0 = random_vector(10000,0.05);

函数`random_vector`是容易实现的.`x_0`包含500个非零元素.之后运行代码:

    y = D*x_0;
    result = ssd(D,y,10);

之后可以比较`result`和`x_0`,发现它们van♂全一致.


  [1]: https://arxiv.org/abs/1709.08388
  
