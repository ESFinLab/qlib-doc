=================
金融工程知识基础
=================

.. toctree::
   :maxdepth: 2
   :caption: 主要内容:
   
   wiener_process_and_ito_lemma


关于几何布朗运动
================

.. math:: 
    :label: eq_1

    dS(t) = \mu S(t) dt + \sigma S(t) dW_t

其中 :math:`W_t` 是一个 `维纳过程 (Wiener process) <https://en.wikipedia.org/wiki/Wiener_process>`_。 :math:`\mu` 表示漂移， :math:`\sigma` 表示随机波动的幅度（即波动率）， 在Black-Scholes-Merton的模型假设下，它们都是常数。

.. note:: 
    在风险中性假定下，对于权益类资产 :math:`\mu = r_f - q`, 其中 :math:`r_f` 表示无风险利率，:math:`q` 表示连续年化分红率。

.. note:: 
    对于挂钩标的为指数，但实际对冲为相应股指期货的衍生品，可以将贴水作为分红处理，严格的证明请参考附录。


随机过程的离散化
================
我们利用伊藤引理 :eq:`eq_3` 可以得到资产价格几何布朗运动的解析形式，进而得到按观察点离散的价格路径模拟算法。

设 :math:`f(S(t)) = \ln{S(t)}`,那么

.. math:: 
    :label: eq_4

    \frac{\partial f}{\partial S} = \frac{1}{S}

.. math:: 
    :label: eq_5
    
    \frac{\partial^2 f}{\partial S^2} = -\frac{1}{S^2}

将 :eq:`eq_4`， :eq:`eq_5` 以及 :math:`\mu_t = \mu S(t)`, :math:`\sigma_t = \sigma S(t)` 代入 :eq:`eq_3` 式可以得到

.. math:: 
    :label: eq_6
    
    d\ln{S(t)} = (\mu - \frac{1}{2}\sigma^2)dt + \sigma dW_t


:eq:`eq_6` 式两边积分，即可求得 :eq:`eq_1` 式从 :math:`t` 到 :math:`t+\Delta t` 区间的解析形式：


.. math:: 
    :label: eq_7
    
    S(t + \Delta t) = S(t)\exp((\mu - \frac{1}{2}\sigma^2)\Delta t + \sigma W_{\Delta t})

其中 :math:`W_{\Delta t} \sim \mathcal{N} (0, \sqrt{\Delta t})`.

因此，资产价格从 :math:`t` 到 :math:`t+\Delta t` 的离散化方法可表示为

.. math:: 
    :label: eq_8
    
    S(t + \Delta t) = S(t)\exp((\mu - \frac{1}{2}\sigma^2)\Delta t + \sigma \sqrt{\Delta t}\epsilon)

或者更简单的，对数形式:

.. math:: 
    :label: eq_9
    
    \ln{S(t + \Delta t)} = \ln{S(t)} + (\mu - \frac{1}{2}\sigma^2)\Delta t + \sigma \sqrt{\Delta t}\epsilon

其中 :math:`\epsilon \sim \mathcal{N} (0, 1)`。

.. note::
    注意到 :eq:`eq_9` 式避免了昂贵的幂运算，这是我们在蒙卡模拟算法中推荐的形式。

相关性资产的随机过程
=====================
对于多个具有相关性的资产 :math:`S_i(t)`, 其离散化的路径可以表示为：

.. math:: 
    :label: eq_10

    \ln{S_i(t + \Delta t)} = \ln{S_i(t)} + (\mu_i - \frac{1}{2}\sigma_i^2)\Delta t + \sigma_i \sqrt{\Delta t}\epsilon_i

其中 :math:`\epsilon_i \sim \mathcal{N} (0, \rho_{ij})`. 

对于N维的正态分布随机数，我们可以通过Cholesky分解法获得。

设 :math:`\epsilon = [\epsilon_1, ..., \epsilon_N]^T` 是N个相互独立的正态分布随机数， 矩阵 :math:`L` 满足

.. math:: 
    :label: eq_11

    LL^T = [\rho_{ij}]

其中矩阵 :math:`L` 是下三角矩阵。

那么 

.. math:: 
    :label: eq_12

    \hat{\epsilon} = L\epsilon

是一组服从 :math:`\mathcal{N} (0, \rho_{ij})` 的随机数（请证明）。

随机波动率模型
===========================

除BS模型（常数波动率模型）以外，还有一类随机波动率模型(stochastic volatility model)也被广泛应用，其中比较典型的就是Heston模型 [1]_。

Heston模型中，方差具有均值回归的特性：

.. math:: 
    :label: eq_13

    dV(t) = \kappa (\theta - V(t))dt + \xi\sqrt{V(t)}dW_t

因此上述模型也称为均值回归模型或Cox-Ingersoll-Ross模型，其中，:math:`V(t)` 是方差，

- :math:`\kappa` :用以表征均值回归的强度；
- :math:`\theta` :用以表征方差的长期均值；
- :math:`\xi`: 用以表征方差的随机波动的强度。

:eq:`eq_13` 式 与 :eq:`eq_1` 式结合，再增加一个参数 :math:`<W_SW_V> = \rho` 表示资产价格的随机过程与波动的随机过程的相关性，就形成了所谓的Heston模型。

.. note:: 
    Cox-Ingersoll-Ross 模型也常常用来为利率的动态过程建模。




附录B：期货对冲股指期权下的Black-Scholes偏微分方程
==================================================
本节改编自镒链科技微信公众号原创学术文章，详见参考文献 [3]_

期现联动关系
---------------
设期货价格 :math:`F(t)` 与指数（现货价格） :math:`S(t)` 之间的关系为

.. math:: 
    :label: eq_14

    F(t) = e^{-q(T-t)}S(t)

其中 :math:`q` 表示年化贴水率（ :math:`q>0` 表示贴水） 

Black-Scholes 偏微分方程
--------------------------
一份期权的价值依然由其挂钩标的S决定，也即 :math:`V=V(S, t)`。 根据伊藤引理，以及BS模型，我们有

.. math:: 
    :label: eq_15

    dV = (\mu S\frac{\partial V}{\partial S} + \frac{1}{2}\sigma^2 S^2\frac{\partial^2V}{\partial S^2} + \frac{\partial V}{\partial t})dt + \sigma S\frac{\partial V}{\partial S}dW_t

到这一步为止，与传统的Black-Scholes偏微分方程的推导并无二致。

下一步，需构建一个瞬时无风险投资组合 :math:`\Pi(t)`。由于股指并不能直接进行交易，我们以股指期货来构建该组合， 即

.. math:: 
    :label: eq_16

    \Pi = V - \Delta F

需要注意，此处的 :math:`\Delta` 希腊字母Delta, 是在Delta对冲中我们需要对冲的股指期货的相应数量。

那么该投资组合在极短的时间dt里的价值变化就是

.. math:: 
    :label: eq_17

    d\Pi = dV - \Delta dF

由 :eq:`eq_14` 式，我们有

.. math:: 
    :label: eq_18

    dF = e^{-q(T-t)}dS + qSe^{-q(T-t)}dt

再将 :eq:`eq_1` 式代入 :eq:`eq_18` 式中，进而可得

.. math:: 
    :label: eq_19

    dF = e^{-q(T-t)}(\mu + q)Sdt + e^{-q(T-t)}\sigma SdW_t

将 :eq:`eq_19` ， :eq:`eq_15` 代入 :eq:`eq_17` ， 可以得到

.. math:: 
    :label: eq_20

    d\Pi = (\mu S\frac{dS}{dt} + \frac{1}{2}\sigma^2S^2\frac{\partial^2 V}{\partial S^2} + \frac{\partial V}{\partial t}-\Delta e^{-q(T-t)}(\mu + q)S)dt + (\sigma S\frac{\partial V}{\partial S}-\Delta e^{-q(T-t)} S)dW_t

可以看到当且仅当 :math:`\Delta = e^{q(T-t)}\frac{\partial V}{\partial S}` 时，上式中的随机项才能被完全消除。我们将其代回 :eq:`eq_20` ，可以得到

.. math:: 
    :label: eq_21

    d\Pi = (\frac{1}{2}\sigma^2S^2\frac{\partial^2 V}{\partial S^2} + \frac{\partial V}{\partial t} - qS\frac{\partial V}{\partial S})dt

另外在无套利假设下，有 

.. math:: 
    :label: eq_22

    d\Pi = r\Pi dt 

将 :eq:`eq_16` , :eq:`eq_21` 以及 :eq:`eq_14` 代入 :eq:`eq_22`， 经整理最终可以得到

.. math:: 
    :label: eq_23

    \frac{\partial V}{\partial t} + \frac{1}{2}\sigma^2S^2\frac{\partial^2 V}{\partial S^2} + (r-q)S\frac{\partial V}{\partial S} - rV = 0

不难发现 :eq:`eq_23` 与连续分红股票期权的Black-Scholes偏微分方程具有同样的形式。因此只需要把贴水率当分红处理即可。而在对冲操作中，我们有

.. math:: 
    :label: eq_24

    \Delta_F = e^{q(T-t)}\frac{\partial V}{\partial S} = S(t)/F(t)\Delta_S

.. note:: 
    注意到，对于挂钩股指而采用期货对冲的衍生品，其Delta仍由股指点位来计算得到，但对冲的实际头寸 :math:`\Delta_F` 需要被修正，最终是保持Delta金额不变。

附录C：Cholesky矩阵分解算法
============================
本节整理自维基百科相关页面, 详见参考文献 [4]_

设正定对称矩阵

.. math:: 
    :label: eq_27
    
    \begin{align}
    \mathbf{A} = \mathbf{L}\mathbf{L}^T &= \begin{pmatrix}   L_{11} & 0 & 0 \\
    L_{21} & L_{22} & 0 \\
    L_{31} & L_{32} & L_{33}\\
    \end{pmatrix}
    \begin{pmatrix}   L_{11} & L_{21} & L_{31} \\
    0 & L_{22} & L_{32} \\
    0 & 0 & L_{33}
    \end{pmatrix} \\
    & =\begin{pmatrix}   L_{11}^2 &   &(\text{symmetric})   \\
    L_{21}L_{11} & L_{21}^2 + L_{22}^2& \\
    L_{31}L_{11} & L_{31}L_{21}+L_{32}L_{22} & L_{31}^2 + L_{32}^2+L_{33}^2
    \end{pmatrix}
    \end{align}

可以得到：

.. math:: 
    :label: eq_28

    \mathbf{L} = 
    \begin{pmatrix} \sqrt{A_{11}} &  0 & 0  \\
    A_{21}/L_{11} & \sqrt{A_{22} - L_{21}^2} & 0 \\
    A_{31}/L_{11} &  \left( A_{32} - L_{31}L_{21} \right) /L_{22}  &\sqrt{A_{33}- L_{31}^2 - L_{32}^2}
    \end{pmatrix}

该结果不难推至更高维，从而得到分解后的下三角矩阵元素的表示式：

.. math:: 
    :label: eq_29
    
    \begin{align}
    L_{j,j} &= (\pm)\sqrt{ A_{j,j} - \sum_{k=1}^{j-1} L_{j,k}^2 }\\
    L_{i,j} &= \frac{1}{L_{j,j}} \left( A_{i,j} - \sum_{k=1}^{j-1} L_{i,k} L_{j,k} \right) \quad \text{for } i>j
    \end{align}



附录D：Heston过程的离散化
=============================================
本节整理自参考文献 [5]_

标的资产价格的离散化算法
------------------------
设 :math:`X(t) = \ln(S(t))`, 那么

.. math:: 
    :label: eq_25
    
    X(t+\Delta t) =X(t)  + r\Delta t + K_0 + K_1V(t) + K_2V(t+\Delta t) + \sqrt{K_3 V(t) + K_4V(t+\Delta t)}Z 

其中  :math:`Z \sim \mathcal{N}(0, 1)`, 

- :math:`K_0 = -\frac{\rho\kappa\theta}{\xi}\Delta t`
- :math:`K_1 = \gamma_1(\frac{\kappa\rho}{\xi}- \frac{1}{2})\Delta t - \frac{\rho}{\xi}`
- :math:`K_2 = \gamma_2(\frac{\kappa\rho}{\xi}- \frac{1}{2})\Delta t + \frac{\rho}{\xi}`
- :math:`K_3 = \gamma_1(1- \rho^2)\Delta t`
- :math:`K_4 = \gamma_2(1- \rho^2)\Delta t`

一般，我们取 :math:`\gamma_1 = \gamma_2 = 0.5`

方差均值回归过程的离散化算法-QE算法
-------------------------------------
任意选择一个关键值 :math:`\psi_c \in [1, 2]`，例如1.5，

- 1. 计算 :math:`m` 和 :math:`s^2` ;
- 2. 计算 :math:`\psi = s^2/m^2` ;
- 3. 取一个均匀分布的随机数 :math:`U_v` ;
- 4. 如果 :math:`\psi \leq \psi_c`  ，则
   
   - 1. 计算 a 和 b;
   - 2. 计算 :math:`Z_v = \Phi^{-1}(U_v)` ;
   - 3. 设定 :math:`V(t+\Delta t) = a(b+ Z_v)^2` .
- 5. 否则:
  
   - 1. 计算 :math:`\beta`  和 :math:`p` ;
   - 2. 设定 :math:`V(t+\Delta t) = \Psi^{-1}(U_v;p, \beta)`.

上述算法中所涉及到的计算公式如下：

.. math::
    :label: eq_26

    \begin{aligned}
    m &= \theta + (V(t) - \theta)e^{-\kappa\Delta t}\\
    s^2 &= \frac{V(t)\xi^2e^{-\kappa\Delta t}}{\kappa}(1-e^{-\kappa\Delta t}) + \frac{\theta \xi^2}{2\kappa}(1-e^{-\kappa\Delta t})^2\\
    b^2 &= 2\psi^{-1} - 1 + \sqrt{2\psi^{-1}}\sqrt{2\psi^{-1}-1}\\
    a &= \frac{m}{1+b^2}\\
    p &= \frac{\psi -1}{\psi +1}\\
    \beta &= \frac{1-p}{m}\\
    \Psi^{-1}(u;p,\beta) &= \begin{cases}
    0, & 0\leq u\leq p,\\
    \beta^{-1}\ln\frac{1-p}{1-u},& p<u\leq 1.
    \end{cases}
    \end{aligned}

参考资料
=========

.. [1] Heston, Steven. “A Closed-Form Solution for Options with Stochastic Volatility with Applications to Bond and Currency Options.” Review of Financial Studies 6 (1993): 327-343.

.. [3] 期货对冲股指期权下的Black-Scholes偏微分方程， 镒链科技微信公众号 https://mp.weixin.qq.com/s/iFmrWS8JNAROoIG7D5n-mg

.. [4] Cholesky decomposition, Wikipedia https://en.wikipedia.org/wiki/Cholesky_decomposition

.. [5] Andersen, Leif B.G., Efficient Simulation of the Heston Stochastic Volatility Model (January 23, 2007). Available at SSRN: https://ssrn.com/abstract=946405 or http://dx.doi.org/10.2139/ssrn.946405


