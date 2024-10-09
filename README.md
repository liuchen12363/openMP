# gmres
PETSC – решить СЛАУ GMRES и sparse matrix
广义最小残量方法(一般简称GMRES)是一个求解线性方程组 数值解的迭代方法。这个方法利用在Krylov子空间中有着最小残量的向量来逼近解。
需要求解的线性方程组记为
。
假设矩阵A是nn阶的可逆的。进一步，假设b是标准化的，即||b|| = 1 (在这篇文章中，||·||是Euclidean范数)。
这个问题的m阶Krylov子空间是
。
GMRES通过使得残量Axm − b最小的向量xm ∈ Km来逼近Ax = b的精确解。
但是，向量b, Ab, …, Am−1b几乎是线性相关的。因此，用Arnoldi迭代方法求得的这组Km的标准正交基

来取代上面的那组基。所以，向量xm ∈ Km写成xm = Qmym，其中ym ∈ Rm且Qm是由q1, …, qm组成的nm矩阵。
Arnoldi过程也产生一个 (m+1)m阶上Hessenberg矩阵满足
。
因为是正交的，我们有
，
其中

是Rm+1的标准基的第一个向量，并且
 ，
其中是初始向量(通常是零向量)。因此，求使得残量
。
的范数最小的。这是一个m阶线性最小二乘问题。
这就是GMRES方法。在迭代的每一步中：
1.做一步Arnoldi迭代方法；
2.寻找使得||rm||最小的 ；
3.计算 ；
4.如果残量不够小，重复以上过程。
在每一步迭代中，必须计算一次矩阵向量积Aqm。对于一般的n阶稠密矩阵，这要计算复杂度大约2n2次浮点运算。但是对于稀疏矩阵，这个计算复杂度能减少到O(n)。进一步，关于矩阵向量积，在m次迭代中能进行O(m n)次浮点运算。

稀疏矩阵（英语：sparse matrix），在数值分析中，是其元素大部分为零的矩阵。反之，如果大部分元素都非零，则这个矩阵是稠密(dense)的。在科学与工程学领域中求解线性模型时经常出现大型的稀疏矩阵。
