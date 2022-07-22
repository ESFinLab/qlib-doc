==================
维纳过程与伊藤引理
==================

本节主要参考文献 [1]_ 中第12章: Wiener process and Ito's lemma 的部分内容。

维纳过程
===========
定义：如果一个变量 :math:`z` 满足以下两个性质，我们就称该变量服从 **维纳过程** （Wiener process）:

**性质一**: 变量在一段微小时间 :math:`\Delta t` 的变化 :math:`\Delta z` 满足：

.. math:: 
    :label: eq_p_1

    \Delta z = \epsilon \sqrt{\Delta t}

其中，:math:`\epsilon \sim \mathcal{N}(0, 1)` .


**性质二**: 对于任意两个不同的微小时间段 :math:`\Delta t`， :math:`\Delta z` 都是相互独立的。

性质二表明一个维纳过程也是一个马尔科夫过程（Markov process [2]_ ）, 也即关于变量未来的情况只与当前情况有关，而与历史信息无关。

根据性质一，维纳过程的均值、标准差和方差分别为 :

.. math:: 
    :label: eq_property_w

    \begin{align}
    \mathbf{E}(\Delta z) &= 0\\
    \mathbf{\sigma}(\Delta z) &= \sqrt{\Delta t}\\
    \mathbf{\sigma^2}(\Delta z) &= \Delta t
    \end{align}


广义维纳过程
==============
变量 :math:`x` 服从广义的维纳过程， 如果

.. math:: 
    :label: generalized_wiener_process

    dx = adt + bdz

其中 :math:`a` , :math:`b` 均为常数。

广义维纳过程下， :math:`\Delta x \sim \mathcal{N}(a\Delta t, b^2\Delta t)` 。

.. image:: /images/wiener_process.png

伊藤过程
===========
从广义维纳过程推广至更一般的情形， 即为伊藤过程，其定义为，

.. math:: 
    :label: ito_process

    dx = a(x, t)dt + b(x, t)dz


伊藤引理
====================
本文主要介绍伊藤第三引理及其应用。

对于满足以下随机偏微分方式的随机过程(即伊藤过程)

.. math:: 
    :label: eq_2

    dX_t = \mu_t dt + \sigma_t dW_t

那么对于二次可导函数 :math:`f(t, X_t)` , 有以下等式成立

.. math:: 
    :label: eq_3

    df(t, X_t) = (\frac{\partial f}{\partial t} + \mu_t\frac{\partial f}{\partial X} + \frac{1}{2}\sigma_t^2\frac{\partial^2f}{\partial X^2})dt + \sigma_t\frac{\partial f}{\partial X}dW_t

.. note:: 
    伊藤引理在随机微分过程的离散化以及Black-Scholes-Merton偏微分方程的推导中有重要作用。

关于伊藤引理的推导请参考附录。


附录：伊藤引理的推导
=======================
本节整理自维基百科相关页面，详见参考文献 [3]_

:math:`f(t, X)`  是关于 :math:`X` 的二次可导函数，将其泰勒展开

.. math:: 
    :label: eq_30

    df(t, X) = \frac{\partial f}{\partial t}dt + \frac{\partial f}{\partial X}dX + \frac{1}{2}\frac{\partial^2 f}{\partial X^2}dX^2+...

其中

.. math:: 
    :label: eq_31
    
    \begin{align}
    dX^2 &= (\mu_tdt + \sigma_t dW_t)^2\\
    &=\mu_t^2dt^2 + 2\mu_t\sigma_t dt dW_t + \sigma_t^2dW_t^2
    \end{align}

当 :math:`dt \rightarrow 0` 时, :math:`dt^2` 项和 :math:`dtdW_t` 项相比 :math:`dW_t^2(\mathcal{O}(dt))` 更快趋于0， 因此将 :math:`dt^2` 项和 :math:`dtdW_t` 项设为0， 将 :math:`dW_t^2` 项设为 :math:`dt` , 可得

.. math:: 
    :label: eq_32
    
    \begin{align}
    df(t, X) &= \frac{\partial f}{\partial t}dt + \frac{\partial f}{\partial X}dX + \frac{1}{2}\frac{\partial^2 f}{\partial X^2}dX^2\\
    &= \frac{\partial f}{\partial t}dt + \frac{\partial f}{\partial X}(\mu_tdt + \sigma_t dW_t) + \frac{1}{2}\frac{\partial^2 f}{\partial X^2}(\sigma_t^2dt)\\
    &= (\frac{\partial f}{\partial t} + \mu_t\frac{\partial f}{\partial X} + \frac{1}{2}\sigma_t^2\frac{\partial^2f}{\partial X^2})dt + \sigma_t\frac{\partial f}{\partial X}dW_t
    \end{align}

参考资料
===========

.. [1] Hull, John C. Options futures and other derivatives, 7th edition. Pearson Education, 2003.

.. [2] Markov process, Wikipedia https://en.wikipedia.org/wiki/Markov_chain

.. [3] Ito's lemma, Wikipedia https://en.wikipedia.org/wiki/It%C3%B4%27s_lemma