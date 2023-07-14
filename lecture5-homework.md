# 第5课 课后作业

## 第1题 如何制作false KZG proof当我们知道s

proof(P(z)=y)可以写作(Commitment-y)/(s-z)，而我们知道Commitment就是P(s),如果我们知道s，对于任意的y1 != y，我们都能计算出(P(s)-y1)/(s-z),这就是一个false proof(P(z)=y1)

## 第2题 用KZG构造向量承诺

如果我们知道一个向量M=(m0, m1, m2,...,mq), 我们可以先利用拉格朗日差值公式求得一个多项式I,满足I(i)=mi  

那么我们的proof(Mi=mi)就可以写作 (I(s) - mi)/(s-i)

## 第3题 同时证明多个点上的估值

同样地，类比第二题，我们首先有Commitment=P(s)，然后我们把要证明的点跟估值用差值公式得到I，那么我们的proof(多个点的估值)=(P(s)-I(s))/(s-Z(s)), 其中Z是多个点估值构成的零多项式Z=(x-eval_1)*...*(x-eval_m)
