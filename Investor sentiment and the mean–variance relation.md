# 1. 研究假设
本文基于3个假设：
1. 市场中存在积极和消极的情绪投资者；
2. 情绪投资者不愿意做空；
3. 情绪投资者错误的估计方差。

上述三点假说，会导致两个推论：
1. 情绪投资者会错误估计方差，因此市场中情绪投资者的存在会削弱均值-方差关系；
2. 由于情绪投资者不愿做空股票，因此他们持有更多的股票，当市场情绪高涨时影响更大；

基于上述的推论，形成了文章的假说：
>The heavy presence of sentiment investors during high-sentiment periods should undermine an otherwise positive mean–variance tradeoff in the stock market.
>在情绪高涨时期，情绪投资者的大量出现会破坏股市中原本的正向均值-方差关系

# 2.波动率模型

本文选用4个模型来计算波动率
1. Rolling-Window-mode

$$Var_t(R_{t+1})=\sigma^2_t=\frac{22}{N_t}\sum_{d=1}^{N_t}r^2_{t-d}$$
	所以这里为什么要乘以系数22呢？
用方差的差值作为volatility innovation的代理变量
$$Var(R_{t+1})^u=\sigma^2_{t+1}-Var_t(R_{t+1})=\sigma_{t+1}^2-\sigma_t^2$$
2. Mixed data sampling approach
相比于RWm，这种方法能够更加灵活的调整系数
$$Var_t(R_{t+1})=22\sum_{d=0}^{250}w_dr^2_{t-d}$$
	其中$$w_d(\kappa_1,\kappa_2)=\frac{exp(\kappa_1d+\kappa_2d^2)}{\sum_{i=0}^{250}exp(\kappa_1i+\kappa_2i^2)}$$
在这种方法下，用与 RWm 同样的方式计算volatility innovation
3. GARCH and asymmetric GARCH
GARCH模型和非对称的GARCH模型类似，只是后者考虑波动不同方向带来的影响，下面仅以GARCH为例
$$r_{t+1}^{raw}=\mu+\epsilon_{daily,t+1}$$
$$h_{t+1}=\omega+\beta h_t+\alpha \epsilon_{daily,t}^2$$
于是，我们可以计算出月度的波动：
$$Var_t(R_{t+1})=E_t(\sum_{d=1}^{22}h_{t+d})$$
接着计算出：
$$Var(R_{t+1})^u=Var_{t+1}(R_{t+2})-Var_t(R_{t+2})$$
# 3.实证部分
## 3.1 投资者情绪指数
Baker and Wurgler(2006)形成的一个综合情绪指数
>closed-end fund discount, the NYSE share turnover, the number of IPOs, the average first-day return of IPOs, the equity share in new issues, and the dividend premium.
>首先对宏观经济变量进行回归，取出残差后选用主成分回归的第一个主成分作为投资者情绪
## 3.2 描述性统计
![image](https://github.com/FuHuakang/Paper_Read/assets/110003295/47c9dd65-8fdb-4a66-a8a0-a3132d593463)

从上图中可以观察以下几点：
1. 对于Equal-weighted index而言，低情绪时期的收益率明显高于高情绪时期
>在高情绪时期，股票被高估从而收益降低
2. 过往的研究发现股票的收益率的偏度是负数的，但是在低情绪时期则是正数（0.958）
>理论和实证结果认为情绪是均值回归的，高情绪倾向于在右端有长尾，这种情绪推高了价格，从而导致收益率在左端出现长尾（偏度为负数）
3. 此外还可以注意到方差的不同，在High sentiment时期的方差明显高于Low sentiment时期，这符合情绪交易者在该时期发挥更重要作用的假说
## 3.3 均值方差关系
下面终于到了文章的核心部分，检验均值方差关系
首先设定模型：
$$R_{t+1}=a+bVar_t(R_{t+1})+\epsilon_{t+1}$$
为了考虑情绪的影响，加入情绪的虚拟变量：
$$R_{t+1}=a_1+b_1Var_t(R_{t+1})+a_2D_t+b_2D_tVar_t(R_{t+1})+\epsilon_{t+1}$$
基于假设，我们认为 $b_1$应当是正的，而 $b_2$是负的，因为高情绪会削弱均值方差关系

![image](https://github.com/FuHuakang/Paper_Read/assets/110003295/7e3fd9e9-7f1c-45f6-885f-178dc2e82ae1)

在One-regime方程中发现均值-方差关系是模糊的，R方很小，同时统计上不显著
在Two-regime方程中，正如我们所预料的那样，而且在统计上同样显著
>上述的结果基于RWm计算，在文中还报告了剩余三种模型，结果同样支持假设
后续还加入了 $Var_t(R_{t+1})^u$
## 3.4 机制
本文研究的重点在于投资者情绪如何影响均值方差关系，研究的方式使用交互项回归的方式，文中包括两个：
$$D_tVar_t(R_{t+1})$$
$$D_tVar_t(R_{t+1})^u$$
这对应文章所研究的两个机制，即mean–variance relation和mean–innovation relation
在过去的研究中，人们直接观察情绪与股价水平之间的关系
文章的创新之处在于提出新的机制：**情绪——方差补偿——股价水平**
这意味着在市场情绪高涨时期，情绪交易者大量参与交易会破坏原有的均值-方差关系，从而导致风险补偿降低
此外该研究也为风险补偿提供了实证证据
