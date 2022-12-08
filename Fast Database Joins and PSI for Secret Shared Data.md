# Fast Database Joins and PSI for Secret Shared Data

>- *[MRR20]Fast Database Joins and PSI for Secret Shared Data*
>  - 论文发表在CCS 2020，论文链接见[ACM CCS](https://dl.acm.org/doi/10.1145/3372297.3423358)，[eprint](https://eprint.iacr.org/2019/518)



## Consatructaion

### Overview

***Database Joins***

+ Inner Join (PSI)
+ Full Join(Union)
+ Left Join(PSI)
+ Outer Join(Union)

***Setting***

+ Semi-honest 
+ 3 party
+ Honest majority
+ Outsourced MPC

  + $n$ clients can secret shared their data between the 3 parties



==The core data structure：**Cuckoo Hash Tables**==



**Overview of join Algorithm(no security)**:

首先，在不涉及秘密份额时，连接操作如下：

1. 假设 $Y_1,X_1$  是连接键，  利用布谷鸟哈希将 $Y_1[i]$ 映射到哈希表 $T$ 中
2. 对 $X[i]$  进行布谷鸟哈希，如果匹配，构建 $\hat{Y}^0[i]、\hat{Y}^1[i]$



 

1. **<u>Revealing hashes</u>**

​	How to reveal $h_1(x),h_2(x)$ without revealing $X$ (similarly)?

2. **<u>Cuckoo hashing</u>** 

​	How to rearrange $[X]$ into $[T_X]$ without revealing $X$?

3. **<u>Selectaing from $T_X$</u>**

​	How to rearrange $[T_X]$ into $[T_{X,i}^*]$ without revealing $X,Y$?

4. **<u>How to compare?</u>**

​	How do we compare secret shared values?



### Randomized encodings

***Oblivious Encodings***  

+ Define cuckoo hash function as `PRF` output

​		$h_1(x)=F_k(x,1)$

​		$h_2(x)=F_k(x,2)$

+ Within MPC, compute

​		$[k]\leftarrow \{0,1\}^\kappa$

​		for $i=1,...,n:$ 

​			$[h_1(x_i)]=F_{[k]}([x_i],1)$

​			$[h_2(x_i)]=F_{[k]}([x_i],2)$

​		Reveal $h_1(x_i), h_2(x_i)$ to party $P_1$

+ Similarly, reveal $h_1(y_i),h_2(y_i)$ to party $P_1$
+ Since all $x_i(resp. y_i)$ are unique, revealing PRF outputs does not leak information.



 takes as input several tuples $([\![B_i]\!], [\![X_i]\!],P_i,)$ $\$

+ $[\![B_i]\!]:$ 其中 $B_i\in \{0,1\}^d$，表示 $d$ 比特的数组
+  $[\![X_i]\!]:$ 其中 $X_i\in (\{0,1\}^d)^\sigma$，表示 $d$ 个长度为 $\sigma$ 的字符串
  + $P_i:$ 表示 $P_i$ 输出加密后的三元组																																																				

### Oblivious switching network

+ inputs:

  + $P_1$ inputs an arbitrary function $\pi(i):[n]\rightarrow [m]$
  + $P_1,P_2$ inputs $[A]=[a_1],...,[a_n]$
  + $P_3$ has no input

+ Output:

  + $P_1,P_3$ output $[D]=[d_1],...,[d_m]$

    ​	where $[d_i]=[a_{\pi(i)}]$

### 3pc Oblivious Permutation $\prod_{perm}$

+ Restriction, $\pi(i):[n]\rightarrow[n]$ is apermutaion.
+ $P_1$ samples random oermutations $\pi_2,\pi_3$ st.

​			$\pi(i)=\pi_3(\pi_2(i))$



### Oblivious switching network Overview $\prod_{Switch}$

1. If $a_i$ appears $k$ times in output then $a_i$ should have $k-1$ dummies after it...
2. If $a_i$ appears $k$ times in output then duplicate $a_i$ into the next $k-1$ dummies after it...
3. Reorder the array as desired. 



### Duplication Layer $\prod_{dup}$

+ $P_1$ has $s\in \{0,1\}$ and $P_2$ has $a_0,a_1\in\{0,1 \}^\sigma$
+ $P_2$ samples $r,w_0,w_1\leftarrow\{0,1\}^{\sigma}$ and $\phi\leftarrow \{0,1\}$
+ $P_2$ sends $m_0,m_1,\phi$ to $P_1$ where
  + $m_1=a_2\oplus r\oplus w_\phi$
  + $m_2=a_2\oplus r\oplus w_{\phi\oplus1}$
+ $P_2$ sends $w_0,w_1$ to $P_3$
+ $P_1$ sends $\rho=\phi \oplus s$ to $P_3$
+ $P_3$ send $w_\rho$ to $P_1$
+ $P_2$ outputs $[d_1]_2=r$ and $P_1$ outs $[d_1]_1=m_s\oplus w_p$



### Putting it together

1. $P_1$ learns $h_1(x_i),h_2(x_i)$
2. $P_1$ uses Obiv.permutation to construct $T_X$
3. $P_2$ learns $h_1(y_i),h_2(y_i)$
4. $P_2$ uses Obiv.Switching Network to construct $T^*_{X,1},T^*_{X,2}$
5. Compare $T^*_{X,1},T^*_{X,2}$ and $Y$ using generic 3-party MPC.

### Join Protocols

***Join Protocol*** 可以被划分为 $4$ 个阶段：

1. 计算连接键的 ***randomized encoding***
2. 参与方 $P_1$ 根据表 $Y$ 构建布谷鸟哈希表 $T$ ，之后利用 ***permutation protocols*** 重排秘密份额
3. 对于表 $X$ 的每行 $x$ , $P_0$ 使用 ***oblivious switching network*** 将其映射到布谷鸟哈希表相应的位置 $i_1,i_2$ 根据秘密共享三元组 $(x,T[i_1],T[i_2])$
4. $x$ 连接键与 $T[i_1],T[i_2]$ 进行比较，只要其中有一个匹配上了，则填充相应的 $Y^{'}$ 行，否则相应的 $Y^{'}$ 行设置为 *NULL*
5. 然后可以通过比较 $X$ 和 $Y^{'}$ 的第 $i$ 行来构造各种类型的连接。





### Non-unique Join on Column





























### Revealing Results

