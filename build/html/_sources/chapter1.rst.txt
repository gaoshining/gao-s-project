（二）统计分析准备步骤
==========================

1.业务理解
----------------------
分析目的
    时间变量与市场规模之间的关系（并依据时间预测市场规模）。
分析数据
    给出了不同年份和季度，四个不同市场的2G、3G、4G以及总体的市场规模数据。


2.数据读入
----------------------
将原始数据进行加减运算后，得出5G市场规模数据，首先读入5G数据。

.. code::

    import numpy as np
    import pandas as pd
    df_data = pd.read_excel("fiveG.xlsx")


3.数据理解
----------------------

(1)查看形状
++++++++++++++++++++

.. code::

    print(df_data.shape)

out:

 .. image:: xingzhuang.jpg   

(2)查看列名
++++++++++++++++++++

.. code::

    print(df_data.columns)

out:

.. image:: lieming.jpg   

(3)查看描述性统计信息
+++++++++++++++++++++++

.. code::

    df_data.describe()

out:

 .. image:: miaoshu.jpg   

(4)数据可视化
+++++++++++++++++++++++

.. code::

    import matplotlib.pyplot as plt
    plt.scatter(df_data['time_series'],df_data['5GNorth'])

.. image:: xiangguantu.jpg

4.数据准备
----------------------
从可视化结果看出，可以进行线性回归分析。再计算相关系数进行检验。

.. code::

    df_data['time_series'].corr(df_data['5GNorth'])

.. image:: xiangguan.jpg

发现时间序列与5G市场规模的确有强烈的线性相关关系。所以可以采用线性回归的方式建立模型。

为此，需要准备线性回归分析所需的特征矩阵（X）和目标向量（y)。

.. code::

    X=df_data['time_series']
    y=df_data['5GNorth']

在多项式分析中，特征矩阵X根据多项式次数的高低，由多部分组成。比如三次多项式回归，X由三部分组成。

.. code::

    X=np.column_stack((X,np.power(X,2),np.power(X,3)))
