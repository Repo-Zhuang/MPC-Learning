# Falcon: Honest-Majority Maliciously Secure Framework for Private Deep Learning

## 1. Abstract
FALCON是一个恶意模型下三方的深度学习框架，支持训练和推理。它有四个主要的优势:  
1. 对于高容量的神经网络比如VGG16的支持, 有很好的表现
2. 支持批量归一化
3. 对于恶意敌手，能保证安全中止  

## 2. Contributions
+ Malicious Security
FALCON提出了一种新的协议保证计算总是正确的，或者在检测到恶意行动时中止协议。FALCON通过设计一种新的协议来计算非线性函数来实现的。    MPC协议在计算线性函数时通常是高效的，但在计算非线性函数时比如ReLU函数时非常困难。  

+ Improved Protocols
FALCON结合了SecureNN和ABY3的技术达到了协议的有效性。协议改善了中心构建模块的理论复杂度。
协议演示了如何通过算术秘密共享实现对非线性操作的恶意安全协议，并且避免使用协议之间的转换（算术、布尔、混淆电路）  

+ Expressiveness
FALCON完全支持批量归一化层，包括前向和后向传播。换句话说，FALCON支持隐私训练和推理。即使是模型是在训练，所用的数据集也可能是隐私的，之前的工作都假设训练是在信任的环境下进行的。对训练设计安全协议是困难的由于需要反向传播。

## 3. FALCON Overview

### 3.1 A 3-Party Machine Learning Service

一共有三个服务器。数据持有方以复制秘密共享的方式将其数据分享给三个服务器，三个服务器利用这些数据开始训练，之后，询问方可以以份额的方式询问这三个服务器基于训练后以分享形式存在的模型。

### 3.2 Motivating Application: Detection of Child Exploitative Images Online

是

