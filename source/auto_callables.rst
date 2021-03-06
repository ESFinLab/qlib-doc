================================
定价引擎应用案例：自动赎回结构
================================

本节介绍流行的四大类双障碍自动赎回结构。

凤凰结构
===========

.. list-table:: 
   :widths: 75 120 200
   :header-rows: 1

   * - 结构单元
     - 结构要素
     - 说明
   * - 敲出条款
     - | 1. 敲出方向
       | 2. 敲出障碍价格
       | 3. 敲出观察日
     - 
   * - 敲入条款
     - | 1. 敲入方向
       | 2. 敲入障碍价格
       | 3. 敲入观察日
       | 4. 敲入到期后的赔付类型
     - | 1. 敲入方向与敲出方向是成对出现的，**向上敲出向下敲入** 或者 **向下敲出向上敲入**
       | 2. 敲入且未敲出到期以后，凤凰结构持有者须按敲入到期后的赔付类型产生赔付，一般是 :math:`- \mathbf{Max}(K-S_T, 0)`
   * - 票息条款
     - | 1. 票息支付日期
       | 2. 票息形式
     - | 1. 票息支付日期一般与敲出观察日相同， 但是也可以不同；
       | 2. 凤凰结构的票息一般是一笔年化票息收益，可以额外指定票息障碍为票息支付设置条件。