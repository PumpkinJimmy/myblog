---
date: 2021-10-18 10:59:39
---
# 关系型数据库设计范式理论

## 关系模式分解

关系模式分解的问题是**如何构造无损的分解***

### Functional Dependencies（函数依赖）

函数依赖是码的概念的推广

考虑关系模式$R$，设$\alpha, \beta \subseteq R$

称$\alpha$*决定*$\beta$：
$$
    \alpha \rightarrow \beta
$$

当且仅当
$$
    \forall r(R), \forall t_1, t_2 \in r(t_1[\alpha] = t_2[\alpha] \rightarrow t_1[\beta] = t_2[\beta])
$$

如上所述的“决定”就是一个*函数依赖*

### 码与函数依赖

可以对比超码的概念。设$K\subseteq R$是关系模式$R$的超码当且仅当：
$$
    \forall r(R), \forall t_1,t_2 \in r(t_1[K] = t_2[K] \rightarrow t_1 = t_2)
$$

或者说，**超码是函数依赖的特例**：
$$
    K \rightarrow R
$$

同理可以使用函数依赖定义候选码的概念：
$$

    \begin{cases}
        K \rightarrow R\\
        \text{for no } \alpha \subset K, \alpha \rightarrow R
    \end{cases}
$$

### 平凡函数依赖
当$\beta \subseteq \alpha$时，$\alpha \rightarrow \beta$是恒成立的。

这样的函数依赖称为*平凡函数依赖*。

### 函数依赖集的闭包

考虑将函数依赖集看作关系，则**函数依赖集$F$上的闭包$F^+$是指这个关系的传递闭包**。

### 1NF（第一范式）
很简单，数据库必须是一个狭义上的二维表。

关于关系型数据库的所有讨论都是基于二维表的，因此所有所有讨论都不涉及违反1NF的模式

### BCNF(Boyce-Codd Normal Form, BC范式)

定义关系模式$R$关于函数依赖集$F$是属于BCNF，当且仅当$\forall f: \alpha \rightarrow \beta \in F^+$，以下两者至少一条成立：
$$

    \beta \in \alpha\\
    \alpha \rightarrow R
$$

通俗地说，**不属于BCNF的关系是可以做分解的**。因为关系中存在一些“码”（属性集）可以唯一确定另一个属性集合的取值，但这码不能唯一确定元组，这表明关系构造中存在冗余。

### 基于BCNF的关系模式分解

设$\alpha \to \beta$是对模式$R$而言违反BCNF的函数依赖。则将$R$分解为两个模式：

$$
    R_1 = \alpha \cup \beta\\
    R_2 = R - (\beta - \alpha)
$$

上述分解的逻辑上，将这个函数依赖所指出的冗余项拎出来另起一个表，新的表只保留一个依赖键$\alpha$

### Dependency Preservation（依赖保持）（待完善）

### 3NF(Third Normal Form)

3NF就是第三范式。

3NF是对第一范式的弱化。

模式$R$关于函数依赖集$F$满足3NF，当且仅当任取$f \in F^+:\alpha \to \beta$，满足如下条件之一：

$$
    \beta \in \alpha\\
    \alpha \to R\\
    \forall A \in (\beta-\alpha)\text{ exist candidate key }C, A \in C
$$

### Normalized（待完善）

### BCNF的局限性

不满足BCNF的模式中数据能继续分解，但**满足BCNF的模式设计不一定没有冗余**

比如说，考虑实体集包含一个ID和两个多值属性。此时将实体转化为一个关系模式，这个关系模式是BCNF，但不是一个合适的设计。

此时需要新的范式，比如第四范式4NF，它是建立在多值依赖理论上的。

## Functional Dependencies Theory

### Closure of a set of functional dependencies

Armstrong's Axioms:
1. 自反律：$\beta \in \alpha \Rightarrow \alpha \to \beta$
2. 增广律：$\alpha \to \beta \Rightarrow \gamma \alpha \to \gamma \beta$
3. 传递律：$\alpha \to \beta \land \beta\to\gamma \Rightarrow \alpha \to\gamma$ 

### 其他定理（待完善）

1. Union: $\alpha \to \beta, \alpha \to \gamma \Rightarrow \alpha \to \beta\gamma$
2. Decomposition: $\alpha \to \beta\gamma\Rightarrow \alpha \to \beta, \alpha \to \gamma 

### 属性闭包（待完善）

属性闭包是定义在某个属性集（键）上的。

对给定键求属性闭包的方法：
```
aplus = key
for f:a,b in F do
    if a is a subset of aplus do
        aplus += b
    end
end
```


### 无关属性与正则覆盖（待完善）

#### 无关属性

  无关属性$A$定义在某个依赖集$F$中的某个依赖$\alpha \to \beta$的左端$\alpha$或右端$\beta$上

  若$F$的闭包等于$F-(\alpha \to \beta) \cup ((\alpha-A)\to\beta$的闭包，则称$A$是无关属性

- 检测是否无关
  - 定义法
  - 属性闭包法
    
    考虑检测属性$A$在$\alpha \to \beta$中是否无关
    - 若$A \in \beta$：则检验简化集$F'$中$\alpha^+$是否仍然包含$A$，若仍然包含则$A$是无关属性。（其中$F'=F-{\alpha\to\beta}\cup(\alpha\to(\beta-A))$
    - 若$A \in \alpha$：设$\gamma = \alpha - \{A\}$，若$\gamma\to\beta \in F^+$，也即$\beta\subseteq\gamma^+$，则$A$是无关属性

#### 正则覆盖
  
$F_c$是$F$的正则覆盖其同时满足：
- $F_c$等效$F$
- $F_c$中的依赖都不含无关属性
- $F_c$中的依赖各不相同

求解方法：
```
repeat
    合并同键依赖
    去除无关属性
until(F_c不变)
```

### 无损分解
$(R_1,R_2)$是模式$R$的一个无损分解当以下两个依赖至少有一个在$F^+$中：

$$
    R_1 \cap R_2 \to R_1\\
    R_1 \cap R_2 \to R_2
$$

### 依赖保持

若$F$中的依赖可以分别在分解后的模式$R_1, R_2$中验证，而不必联表再验证，则称为依赖保持的分解。

形式化定义：
$$
    (F_1 \cup F_2 \cup \cdots F_n)^+ = F^+
$$

检验依赖保持（待完善）

## 函数依赖理论的运用

### BCNF分解算法

检测是否违反BCNF范式：
1. 计算属性闭包
2. 根据属性闭包检测是否是超码

### 3NF分解算法（待完善）
#### Why not BCNF Decomposition
BCNF分解的缺陷：**不满足依赖保持**

考虑$F$如下：
$$
    JK \to L\\
    L \to K
$$

不存在无损分解同时满足BCNF和依赖保持。

但是，存在无损分解同时满足3NF和依赖保持。

#### 3NF缺陷
作为代价：
- 3NF是存在冗余的。
- 此外，3NF允许了空值。

3NF测试是*NP-hard*，但是3NF分解是P的

### 分解方法
设$F_c$是$F$的正则覆盖，将$F_c$中

## 工程中的函数依赖理论
**还没有商业级DBMS实现在一个关系上检测函数依赖关系是否满足的特性**。原因是检测函数依赖的代价太高昂了。

商业级DBMS实现的特性是函数依赖的弱化：主码


## *多值依赖与4NF（第四范式）

### 多值依赖
称$Y$多值决定$Z$当且仅当：
$$
    \langle y_1, z_1, w_1 \rangle \in r, \langle y_1, z_2, w_2\rangle \in r \rightarrow \langle y_1, z_1, w_2\rangle \in r, \langle y_1, z_2, w_1\rangle \in r
$$

### 对比通常的函数依赖
函数依赖其实“禁止了某些元组的存在”；而多值依赖却是“要求了某些元组的存在“

### 4NF（第四范式）
4NF是BCNF在多值依赖上的推广。简单将BCNF定义中的要求从单值依赖更换成多值依赖即可

4NF要求闭包里的多值依赖要么是平凡依赖，要么是超码依赖。

4NF是BCNF的进一步收紧。

### 4NF分解算法（待完善）
基本跟BCNF分解算法一致。