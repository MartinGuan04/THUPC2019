### 找树 题解

首先把这道题转化为计数题，即对于 $i\in [0, 2^W-1]$，统计权值为 $i$ 的树有多少种，答案就是最大的方案数非 $0$ 的 $i$。

接下来考虑怎么计数。生成树计数显然是基尔霍夫矩阵，但同时考虑权值就很麻烦了。但是注意到，如果所有操作都是异或，那我们可以将矩阵中的每个权值集合进行 FWT，而 FWT 后数组（集合幂级数）中的每一位都独立了，因此可以分每一位分别计算基尔霍夫矩阵去掉一行一列后的行列式，把行列式的答案组成一个新的数组（集合幂级数），将它 IFWT 后即可得到每一个权值对应的树的方案数。而如果所有操作都是与或者或，都可以进行 FMT 来转化成每一位的问题，从而这样解决。

注意到 FWT 的本质是每一个二进制位分别 FFT（长度为 2 的 FFT，变成 `a[0] + a[1] ` 和 `a[0] - a[1]`），FMT 的本质是每一个二进制位分别前缀和，那我们可以将这两种算法结合起来。如果某一位的操作符是异或，那么我们就将这一位进行 FFT；如果某一位的操作符是与（或），就将这一位求后（前）缀和。这样就转化为按（数组里的）位独立的问题了，从而可以按照上面的方法分别计算行列式，最后再拼起来，按照这样的规则逆变换回去即可。

注意到我们只需要判断 0 和非 0，取几个质数在模意义下计算行列式即可。复杂度 $O((n+W)n^2 2^W )$。
