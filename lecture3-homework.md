# 第3课 课后作业

## 第1题 quadratic nonresidue

a. 如果QR(m,x)=0, 则QR(m,s^2*x)=0。显然如果是s^2=x, QR(m,x)=1。 所以如果双方都遵守协议，证明者每次都能正确返回b的值。

b. 如果QR(m,x)=1，无论证明者选择b是0还是1，QR(m,y)=1。因此证明者无法从计算QR(m,y)得到任何b的信息，只能随机返回0/1,那么就会有1/2的概率失败。

## 第2题 quadratic residue

b*. 同时查询到t&st时，可以算出s，即可以提取出知识。

## 第3题 self-pairing implies failure of DDH

计算e(ag,bg), e(y,g)是否相等。因为e(ag,bg)=e(abg,g)，如果相等，即e(abg,g)=e(y,g)，则abg=y

## 第4题 bls

签名验证成功基于双线性映射的特性。
