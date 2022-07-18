=================
金融工程知识基础
=================

关于几何布朗运动
================

.. math:: 
    :label: eq_1

    dS(t) = \mu S(t) dt + \sigma S(t) dW_t

其中 :math:`W_t` 是一个 `维纳过程 (Wiener process) <https://en.wikipedia.org/wiki/Wiener_process>`_。 :math:`\mu` 表示漂移， :math:`\sigma` 表示随机波动的幅度（即波动率）， 在几何布朗运动的模型假设下，它们都是常数。

伊藤引理 Ito's Lemma
====================
本文主要介绍伊藤第三引理及其应用。
对于满足以下随机偏微分方式的随机过程

.. math:: 
    :label: eq_2

    dX_t = \mu_t dt + \sigma_t dW_t

那么对于二次可导函数 :math:`f(t, X_t)` , 有以下等式成立

.. math:: 
    :label: eq_3

    df(t, X_t) = (\frac{\partial f}{\partial t} + \mu_t\frac{\partial f}{\partial X} + \frac{1}{2}\sigma_t^2\frac{\partial^2f}{\partial X^2})dt + \sigma_t\frac{\partial f}{\partial X}dW_t

关于伊藤引理的推导请参考本节附录

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

注意到 :eq:`eq_9` 式避免了昂贵的幂运算，这是我们在蒙卡模拟算法中推荐的形式。

相关性资产的随机过程
=====================
对于多个具有相关性的资产 :math:`S_i(t)`, 其离散化的路径可以表示为：

.. math:: 
    :label: eq_10

    \ln{S_i(t + \Delta t)} = \ln{S_i(t)} + (\mu_i - \frac{1}{2}\sigma_i^2)\Delta t + \sigma_i \sqrt{\Delta t}\epsilon_i

其中 :math:`\epsilon_i \sim \mathcal{N} (0, \rho_{ij})`. 

对于N维的正态分布随机数，我们可以通过Cholesky分解法获得。

设 :math:`\epsilon = [\epsilon_1, ..., \epsilon_N]^T` 是N个相互独立的正态分布随机数， 矩阵 :math:`M` 满足

.. math:: 
    :label: eq_11

    MM^T = [\rho_{ij}]

那么 

.. math:: 
    :label: eq_12

    \hat{\epsilon} = M\epsilon

是一组服从 :math:`\mathcal{N} (0, \rho_{ij})` 的随机数。

随机波动率模型
===========================


附录A：伊藤引理的推导
=======================


附录B：Cholesky矩阵分解算法
============================


附录C：Crox-Ingersol-Rox过程的QE离散化算法
==========================================


参考资料
=========