# Fast Database Joins and PSI for Secret Shared Data

>- *[MRR20]Fast Database Joins and PSI for Secret Shared Data*
>  - 论文发表在CCS 2020，论文链接见[ACM CCS](https://dl.acm.org/doi/10.1145/3372297.3423358)，[eprint](https://eprint.iacr.org/2019/518)



## Consatructaion

### Overview

***Setting***

+ Semi-honest 
+ 3 party
+ Honest majority
+ Outsourced MPC

  + $n$ clients can secret shared their data between the 3 parties




The core data structure：**Cuckoo Hash Tables**

首先，在不涉及秘密份额时，连接操作如下：



