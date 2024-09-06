---
title: N步相移算法的推导
date: 2024-09-05 23:35:07
tags: 
- 算法推导 
- 条纹投影
categories: 
- 结构光
cover: https://s3.bmp.ovh/imgs/2024/09/05/66839ea5f1df1c3f.jpg
---
相机采集的N步相移图案可以表示为
$$
\begin{equation}
\begin{split}   I_n &=a+b \cos(\varphi+\Delta\varphi_n)\\
      &=a+b\cos\varphi\cos\Delta\varphi_n-b\sin\varphi\sin\Delta\varphi_n
\end{split}
\end{equation}
$$
将$b\cos\varphi$记作$c$，将$b\sin\varphi$记作$s$，那么
$$
\begin{equation}
I_n=a+c\cos\Delta\varphi_n-s\sin\Delta\varphi_n
\end{equation}
$$
我们需要从N个公式（2）中求取未知量$a,c$和$s$,很自然地采用最小二乘法求解，最优化以下偏差：
$$
\begin{equation}
\varepsilon=\sum_{n=1}^N \big(I_n-(a+c\cos\Delta\varphi_n-s\sin\Delta\varphi_n)\big)^2
\end{equation}
$$
由偏导$\frac{\partial\varepsilon}{\partial a}=0$可得
$$
\begin{equation}
\sum_{n=1}^{N}I_n=aN+c\sum_{n=1}^{N}\cos\Delta\varphi_n-s\sum_{n=1}^{N}\sin\Delta\varphi_n
\end{equation}
$$
由偏导$\frac{\partial\varepsilon}{\partial c}=0$可得
$$
\begin{equation}
\sum_{n=1}^{N}I_n\cos\Delta\varphi_n=a\sum_{n=1}^{N}\cos\Delta\varphi_n+c\sum_{n=1}^{N}\cos^2\Delta\varphi_n-s\sum_{n=1}^{N}\sin\Delta\varphi_n\cos\Delta\varphi_n
\end{equation}
$$
由偏导$\frac{\partial\varepsilon}{\partial s}=0$可得
$$
\begin{equation}
\sum_{n=1}^{N}I_n\sin\Delta\varphi_n=a\sum_{n=1}^{N}\sin\Delta\varphi_n+c\sum_{n=1}^{N}\sin\Delta\varphi_n\cos\Delta\varphi_n-s\sum_{n=1}^{N}\sin^2\Delta\varphi_n
\end{equation}
$$
综合公式（4），公式（5）和公式（6），整理成矩阵形式，为
$$
\begin{equation}
\begin{pmatrix}N&\sum_{n=1}^{N}\cos\Delta\varphi_n&-\sum_{n=1}^{N}\sin\Delta\varphi_n & \\\sum_{n=1}^{N}\cos\Delta\varphi_n & \sum_{n=1}^{N}\cos^2\Delta\varphi_n & -\sum_{n=1}^{N}\sin\Delta\varphi_n\cos\Delta\varphi_n \\ \sum_{n=1}^{N}\sin\Delta\varphi_n &  \sum_{n=1}^{N}\sin\Delta\varphi_n\cos\Delta\varphi_n & -\sum_{n=1}^{N}\sin^2\Delta\varphi_n  \end{pmatrix}\begin{pmatrix}a\\c\\s\end{pmatrix}=\begin{pmatrix}\sum_{n=1}^{N}I_n\\\sum_{n=1}^{N}I_n\cos\Delta\varphi_n\\\sum_{n=1}^{N}I_n\sin\Delta\varphi_n\end{pmatrix}
\end{equation}
$$
由于$\sum_{n=1}^{N}\cos\Delta\varphi_n=0$, $\sum_{n=1}^{N}\sin\Delta\varphi_n=0$,$\sum_{n=1}^{N}\sin\Delta\varphi_n\cos\Delta\varphi_n=0$, $\sum_{n=1}^{N}\cos^2\Delta\varphi_n=\frac{N}{2}$, $\sum_{n=1}^{N}\sin^2\Delta\varphi_n=\frac{N}{2}$, 代入公式（7）中可得，
$$
\begin{equation}
\begin{pmatrix}N&0&0 \\0&\frac{N}{2}&0 \\ 0&0&-\frac{N}{2}\end{pmatrix}\begin{pmatrix}a \\ c \\ s \end{pmatrix}=\begin{pmatrix}\sum_{n=1}^{N}I_n\\\sum_{n=1}^{N}I_n\cos\Delta\varphi_n\\\sum_{n=1}^{N}I_n\sin\Delta\varphi_n\end{pmatrix}
\end{equation}
$$
此时，易得
$$
\begin{equation}
\begin{split}
a&=\frac1N\sum_{n=1}^{N}I_n\\
c&=\frac2N\sum_{n=1}^{N}I_n\cos\Delta\varphi_n\\
s&=-\frac2N\sum_{n=1}^{N}I_n\sin\Delta\varphi_n
\end{split}
\end{equation}
$$
由于$b\cos\varphi=c$, $b\sin\varphi=s$,可得
$$
\begin{equation}
\begin{split}
\tan\varphi&=-\frac{\sum_{n=1}^{N}I_n\sin\Delta\varphi_n}{\sum_{n=1}^{N}I_n\cos\Delta\varphi_n}\\
b&=\frac2N\sqrt{(\sum_{n=1}^{N}I_n\sin\Delta\varphi_n)^2+(\sum_{n=1}^{N}I_n\cos\Delta\varphi_n)^2}
\end{split}
\end{equation}
$$
推导完毕。
