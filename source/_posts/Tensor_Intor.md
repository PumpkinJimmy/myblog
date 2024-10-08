---
title: 张量概论学习
abbrlink: 813299cd
---
# 张量概论学习
## 预备知识
- 爱因斯坦求和约定：
  - 在单项式中，我们约定重复出现恰好一次的上/下标（哑指标）充当求和的变量。也即公式$L_{ij} x_i$的实际含义是$\sum_i L_{ij} x_i$
  - 该约定的好处是在多个sigma嵌套的时候可以在不产生歧义的情况下简化书写；且根据求和运算的性质，可以自然地将这种省略求和的单项代入其他公式进行运算
- 以下讨论是从典型的欧氏空间的向量/矩阵之外，从纯线性空间的视角推导并推广这个张量的概念，原则上可以适用于很多非欧式的线性空间
- *形式（form）*：所谓“形式”，就是关于某种数学对象书写的公式，此时将该公式用于表征甚至定义另一种数学对象。
  - 可以不严谨地理解为“运算”。比如说，后面会见到的，使用向量的并矢形式来定义二阶张量
  - “形式”的意义在于，我们可以将一种运算（一种映射）看作是新的数学对象的定义
## 向量
- 不引入“箭头”的几何直观，我们在“点空间”上定义向量：
  
  考虑任意一个点空间，任取两个点$p, q$，我们不加说明地定义点之间的差运算$q - p$，然后将认为向量表征的就是点的差运算的结果。
  
  由此，认为一阶张量（也即*向量*）可以看作一种关于空间中点的映射：
  
  $\bf{v}(p) = q$，当$q - p = \bf v$
  
  虽然这个有点循环定义，但我们摆脱了向量的坐标表示。实际上，上述定义没有摆脱向量的箭头的几何直观、四边形法则、以及平直空间的隐含假设。

  将向量看作是一种“点映射”的观点让我们直接定义出了向量加法运算：

  $\bf{u} + \bf{v} = (\bf{u} + \bf{v})(p) = \bf{v} (\bf{u}(p))$

  由上我们引申出向量的标量运算的定义：$\alpha \bf{v}(p)$看作$\alpha$次迭代应用$\bf{v}$运算

- 内积
  - *内积*是定义在向量上的一种“线性形式”，满足正定性、对称性、双线性的运算，记作$u \cdot v$
    
    - 内积可以看作一种向量上的线性形式
    - 正定性：$v \cdot v \ge 0$，且仅在$v = \vec 0$是取到0
    - 对称性：$u \cdot v = v \cdot u$
  - 内积的重要性在于，依赖内积，我们可以定义向量的模、点的距离，从而构成所谓的“内积空间”
  - 最重要的是，内积可以定义**向量的正交性**：$\bf {u \cdot v} = 0$等价于$\bf u, v$正交

- 将向量的集合记作空间$\mathcal V$
- $\mathcal V$的*基*
  - 便利记号：克罗内克delta可以看作是关于两个下表的一个标量函数，定义如下

    $$
        \delta_{ij} = \begin{cases}1, i = j\\
        0, i \ne j
        \end{cases}
    $$

  - 这里的基特指*单位正交基*
  - 对于有限维线性空间$\mathcal V$，设维度记作$n = \rm dim (\mathcal V)$，则它的单位正交基$\mathcal B = \{\bf e_1, e_2, \dots, e_n\}$是一个包含$n$个向量的集合，且满足：
    - $\bf{e}_i \cdot \bf{e_j} = \delta_{ij}$

- 线性组合与分量表示：
  - 一个线性组合记作：$\alpha_i \bf{u}_i$（注意这里使用了爱因斯坦求和约定，这其实是一个求和式）
  - 基的特点：线性空间$\mathcal V$中的向量可以使用基$\mathcal B$的线性组合表示：$\bf{u} = \alpha_i \bf{e}_i$，且该表示是唯一的，其中$n$个标量$\alpha_i$组成的数组就是向量的分量表示

## 二阶张量
### 二阶张量定义
  
  定义二阶张量$\bf L$为满足如下条件的映射：

  $$
    \bf{L}: \mathcal V \to \mathcal V\ |\ \bf{L}(\alpha\bf{u}) = \alpha \bf{L(u)}\ \forall u \in \mathcal V, \alpha \in \mathbb R
  $$

  这个定义过于笼统，它只是说明了二阶张量可以用一个向量空间上关于数乘运算可交换运算顺序的映射来表示。我更倾向于用并矢来定义二阶张量。
  
  注意这里“形式”的运用：$\bf L$它本来是一种映射，我们将这种映射形式看作是张量这个数学对象的定义。
  
  实际上，如果将张量看作是实数的推广，我们可以这样书写省略括号：$\bf L(u) = Lu$。$\bf Lu$的记号中，我们强调张量本身作为一种数的推广的特质，弱化它作为映射的特质，但我们仍然要记住它可以理解为一种线性映射。

  我们记从$\mathcal V$上构造的所有二阶张量构成的线性空间为$Lin(\mathcal V)$。

### 并矢
  
  给定$\bf{u,v} \in \mathcal V$，并矢运算$\bf{u} \otimes \bf{v}$是一个二阶张量，对任意向量$\bf w$满足：

  $$
    \bf (u \otimes v) w = v \cdot w\ u
  $$

  注意，上述公式中，左侧将二阶张量的括号省略了，$\bf (u \otimes v) w$等效$\bf (u \otimes v)(w)$；右侧则是向量的数乘运算，这已经有了良好定义。

  这个看起来奇怪的定义，用矩阵论的视角，将向量看作单列矩阵来看就很自然：$\bf u \otimes v = uv^{\rm T}$，$\bf u \otimes v\ w = uv^{\rm T}w = u(v\cdot w)$

### 二阶张量的线性表示

  并矢运算最重要的意义在于，**我们可以将二阶张量空间跟标准向量空间统一在一起**。比如，考虑$\mathcal V$的基$\mathcal B = \{\bf{e}_i\}$，使用并矢构造如下一组基：

  $$
    \mathcal{B}^2 = \{ \bf{e}_i \otimes \bf{e}_j \}\quad \rm i,j = 1,2,\dots,n
  $$

  则**可以证明$\mathcal{B}^2$构成二阶张量空间$Lin(\mathcal V)$的一组基底**，且可以使用分量表示法表示一个二阶张量，也即：

  $$
    \mathbf L = L_{ij} \bf{e}_i \otimes \bf{e}_j
  $$

  上面书写的线性组合的系数就是二阶张量的分量。我们根据下表将分量组织为一张二维表就得到了我们熟悉的矩阵。

  我们可以给出计算分量值$L_{ij}$的公式（展开即可证明）：

  $$
    L_{ij} = \mathbf e_i \cdot \mathbf L \mathbf e_j
  $$

  我们也将二阶张量表示为单位正交基的线性组合的过程称为*正交分解*

### 张量积
  
  可以自然地导出两个二阶张量的积：将二阶张量看作映射的话，则它们的积就是映射的复合。

  *二阶张量的张量积跟矩阵乘法兼容*，或者说如果认为有限维二阶张量就是矩阵，则二阶张量的法则就跟矩阵乘法完全一致。
  
  从二阶张量的正交分解表示我们可以自然地导出矩阵乘法的表达式。考虑$\mathbf{L = AB}$，则：

  $$
  \begin{aligned}
    L_{ij} &= \mathbf e_i \cdot \mathbf L \mathbf e_j \\
    &= \mathbf e_i\cdot \mathbf{ABe}_j \\
    &= \mathbf e_i\cdot \mathbf{A} (B_{hk} \mathbf e_h \otimes \mathbf e_k \mathbf e_j)\\
    &= （由基向量定义和并矢定义）B_{hk}\delta_{kj} \mathbf e_i \cdot \mathbf A  \mathbf e_h\\
    &= （由基向量定义和并矢定义）B_{hk}\delta_{kj}A_{lm} \delta_{mh} \mathbf e_i \cdot  \mathbf e_l\\
    &= （由基向量定义）A_{lm} B_{hk} \delta_{mh} \delta_{il} \delta_{kj} \\
    &= （有delta定义）A_{lh} B_{hk} \delta_{il} \delta_{kj} = A_{ih}B_{hk}\delta_{kj} = A_{ih}B_{hj}
    \end{aligned}
  $$

  结论：
  $$
    L_{ij} = A_{ih}B_{hj}
  $$

  这就是我们熟悉的矩阵乘法公式

- 由二阶张量的并矢分解定义，我们可以自然地导出矩阵论的大部分定义：转置、对称、反对称、迹、行列式、矩阵的逆、特征值
### 转置
- 转置：满足如下等式的运算$\mathbf L^T$：$\mathbf{u\cdot L v} = \mathbf{v \cdot L^\mathnormal{T} u}$
- 上述转置定义的好处是，不依赖向量的正交分解形式，不会将转置运算限定在具体的基底上，说明*给定张量的转置是唯一确定的，与选取的基无关*
- 当然，如果是笛卡尔坐标系，则容易证明二阶张量的转置运算等效下标交换

### 轨迹
- 迹：线性形式$tr: Lin(\mathcal V) \to \mathbb R$，满足$tr(\mathbf a \otimes \mathbf b) = \mathbf{a \cdot b}$
  - 这个定义自然限制了取值是方阵，且容易证明迹等效方阵主对角线的分量之和
  - 且可以证明满足要求的形式是唯一存在的
  
  迹是一个张量的本质属性，它的取值与具体选取的基无关

- 迹的重要意义是**在二阶张量上定义内积，从而给出二阶张量空间上的模（距离）**
- 推导迹的分量计算法则：
  
  $$
  \begin{aligned}
    \mathrm tr \mathbf L &= （由二阶张量的正交分解）\mathrm tr (L_{ij} \mathbf e_i \otimes \mathbf e_j)\\
    &= （由线性形式的定义）L_{ij}\mathrm tr ( \mathbf e_i \otimes \mathbf e_j)\\
    &= （由迹的定义）L_{ij} \mathbf e_i \cdot \mathbf e_j\\
    &= （由正交基定义）L_{ij} \delta_{ij}\\
    &= L_{ii}
  \end{aligned}
  $$

  与矩阵的迹是主对角线元素之和的法则吻合

- 标量积/内积
  - 两个词这里同一个意思，都是指一个满足$\omega(\mathbf A, \mathbf B): Lin(\mathcal V) \times Lin(\mathcal V) \to \mathbb R$的*映射到标量的正定、可交换、对称双线性形式*（可以发现，这个要求跟向量内积几乎一致）
  - 从迹诱导的**二阶张量内积定义**：
    
    $$
        \mathbf A \cdot \mathbf B = \mathrm tr(\mathbf A^T \mathbf B)
    $$
  
  - 容易验证上述定义的内积确实是正定、可交换、对称双线性的
  - 更妙的是，这个内积跟向量内积是兼容的，也即可以将二阶张量替换成向量的并矢形式后直接运算
  - 该内积的分量计算式：
    
    $$
        \begin{aligned}
            \mathbf A \cdot \mathbf B &= \mathrm tr(\mathbf A^T \mathbf B) \\
            &= (\mathbf A^T \mathbf B)_{ii}\\
            &= A_{ij} B_{ij}
        \end{aligned}
    $$

    文字描述就是，对两个矩阵执行“按元素乘法”然后求和。可以发现这个算法可以理解为把矩阵“展平”成向量后执行向量内积的做法
  - 有了上述的定义，容易导出二阶张量欧式模的定义以及两个二阶张量的欧式距离的定义
  - **上述定义可以无缝迁移到任意阶张量用于定义任意阶张量空间的欧式距离**

### 行列式
  - 考虑反对称三线性形式$\omega: \mathcal{V \times V \times V} \to \mathbb R$，反对称三线性意味着$\omega(\mathbf{u, v, \cdot}), \omega(\mathbf{u, \cdot, w}), \omega(\mathbf{\cdot, v, w})$都是双线性形式，且满足：

    $$
      \omega(\mathbf{u, v, w}) = -\omega(\mathbf{v, u, w}) = -\omega(\mathbf{u,w,v}) = -\omega(\mathbf{w,v,u})
    $$
  - 给定二阶张量$\mathbf L$和某三线性对称形式，可以构造一个新的形式$\omega_{\mathbf L}(\mathbf{u,v,w}) = \omega(\mathbf{Lu,Lv,Lw})$
  - 容易证明向量空间的反对称三线性形式也构成一个向量空间，记作$\Omega$，可以证明$\mathrm dim(\Omega) = 1$，则对于$\mathbf L$总存在$\lambda_{\mathbf L} \in \mathbb R$满足有如下等式成立：

    $$
      \omega_{\mathbf L}(\mathbf{u,v,w}) = \omega(\mathbf{Lu,Lv,Lw}) = \lambda_{\mathbf L} \omega(\mathbf{u,v,w})
    $$ 
  
  - 上述等式中的实数$\lambda_\mathbf{L}$就是$\mathbf L$的*行列式*
  - 行列式$\mathrm det \mathbf L$的重要意义在于它是张量的*本质属性*，取值只与张量相关，与张量选取的基无关、与上述定义中选取的线性形式$\omega$也无关
  - 行列式也跟判断向量组线性无关、矩阵奇异性、可逆性等密切相关
### 特征值
  - 跟矩阵论的定义没有出入，就是满足如下方程的实数和向量：

    $$
      \mathbf{Lv = \mathnormal{\lambda}v}
    $$ 
  
  - 结合二阶张量基只有最有意思的结论之一，对于对称二阶张量$\mathbf L$和它的特征值/特征向量有如下分解：

    $$
      \mathbf L = \lambda_i \mathbf e_i \otimes \mathbf e_j
    $$

  - 另外一个有意思的结论：张量积交换的充要条件：coaxial，意思是共享同一组正交基

### 叉积
- 构建反对称向量和反对称矩阵的同构
  - 在三维向量空间中，我们可以证明任意一个向量可以与一个三维二阶的反对称张量一一对应，从而构成了一个反对称二阶张量和向量的同构。反对称二阶张量称为*skew tensor*，它对应的向量就称为*skew vector*

- 定义叉积
  
  三维向量空间中任取向量$\mathbf{a, b}$，定义叉积运算
  $$
    \mathbf{a \times b = W_a b}
  $$

  其中$\mathbf W_a$是向量$\mathbf a$的对应的skew tensor

  容易说明叉积是反对称双线性形式

- 叉积导出的重要结论
  - 向量平行条件：$\mathbf{a \times b = o}$
  - 叉积向量$\mathbf{a \times b}$是张量$\mathbf{b \otimes a - a \otimes b}$的轴向向量

## 张量分析简述
- 考虑到n阶张量上我们都定义了内积和模，所以这些张量空间自然地满足度量空间的要求，从而自然有了距离的定义并使用距离来将实数上的分析学定义迁移到张量空间上
- 场论
  - 场：定义域是点空间的子集（而不是实数的子集）的标量/向量/张量映射
  - 典型的场：梯度场是对给定标量场构造的一个向量场
- （待完善）自变量和因变量都是高阶张量的情况
  - 仅就欧式空间笛卡尔坐标系来说，这种求导运算会得到高阶的张量
- 在机器学习中，我们试图对张量的张量函数做求导的动机基本上都是获得梯度，所以这里实际上关心的是场的全微分形式，如考虑$\mathbf f: \mathcal V \to \mathcal V$，它的全微分形式：

$$
    \mathbf{f(v) = f(v_0) + Df(v_0)\Delta v + o(\Delta v)}
$$

其中我们关心的量其实是$\mathbf {Df(v_0)}$其实就是一个张量场$grad \mathbf f$在$\mathbf v_0$的取值，也即此时的“导数”其实是一个二阶张量

## 关于张量、线性、微分、函数之间的关系的思考（胡扯）
考虑二元函数的微分表达式：

$$
    f(x,y) = f(x_0, y_0) + A\Delta x + B\Delta y + o(\sqrt{\Delta x^2 + \Delta y^2})
$$

把自变量改写成二维向量$\mathbf v$：

$$
    f(\mathbf v) = f(\mathbf v_0) + \mathbf{df(v_0)}\ \Delta \mathbf v + o(|\Delta \mathbf v|)
$$

其中：
$$
  \mathbf{df(v_0)} = grad \mathbf f(\mathbf v_0)\\
  grad \mathbf f(\mathbf v_0) = [A, B]^T
$$

因此可以认为标量场的微分$\mathbf{df(v_0)}$是一阶张量场（梯度场）。其中，梯度场中的向量可以认为存在这样的结构：

$$
    \mathbf{df} = A\mathbf{dx} + B\mathbf{dy}
$$

也即，**对于标量场，给定点的一阶微分其实是一个二维向量空间中的向量**，该空间的基底$\mathbf{dx}$和$\mathbf{dy}$是二维空间中单位正交的向量。

另外，由此可以确定微分和自变量变化量之间的乘法用的是*张量积*

考虑向量场$\mathbf f(\mathbf v)$，类比写出微分形式：

$$
    \mathbf{f( v) = f(\mathbf v_0) + \mathbf{Df(v_0)}\ \Delta \mathbf v + o(|\Delta \mathbf v|)}
$$

此时$\mathbf{Df(v_0)}$只能是一个二阶张量。虽然对于向量表达式我们似乎没法使用“除法”和极限来计算无穷小，但考虑到微分和二阶张量的定义，微分这个“映射”只能是等价一个二阶张量。- 其实，考虑到我们建立分析学利用的是模和度量（比如说无穷小用的是向量的模的无穷小），所以两端除以向量的模长然后求极限就完事了

只考虑笛卡尔坐标系，根据矩乘法则并将向量场拆解为多个标量场，容易得到$\mathbf{Df(v_0)}$的矩阵表达式。其中$Df_{ij} = \frac{\partial f_i}{\partial v_j}$，如果是经典的列布局，则相当于把向量场拆成多个标量场的线性组合后，将每个标量场的梯度当作行向量拼起来

（猜测）写作爱因斯坦求和就是：

$$
    \mathbf{Df} = f_{i,j} dx_i\otimes dx_j
$$

进一步考虑二阶张量函数对二阶张量求梯度，对张量函数$\mathbf{f(X)}$写出微分表达式：

$$
    \mathbf{f( X) = f(\mathbf X_0) + \mathbf{Df(X_0)}\ \Delta \mathbf X + o(|\Delta \mathbf X|)}
$$

根据微分的定义（线性算子）和四阶张量的定义，$\mathbf{Df(X_0)}$会是一个四阶张量，且立刻有如下的表达式：
$$
    Df_{ijkl} = \frac{\partial f_{ij}}{\partial X_{kl}}
$$

（猜测）写作爱因斯坦求和就是：

$$
    \mathbf{Df} = f_{ij, kl} dx_i\otimes dx_j \otimes dx_k \otimes dx_l
$$

事实上，*四阶张量不能像二阶张量那样组织成矩阵书写*，所以矩阵对矩阵的求导才不好书写。当然，现在也折腾出了所谓“分子布局”“分母布局”这些东西强行把它写出来。对于计算机，则没有这些问题，因为n阶张量对应的n维数组在计算机里都被存储为了一维数组。

一个有意思的猜测：*对于n阶微分所在的张量空间，空间的基底恰好是底层的点空间的基底的并矢构成的，就像我们前面看的教程里那样构造，对于二维空间就是类似$dx \otimes dy$的底作为Heissian矩阵的基*

有了知其然也知其所以然的张量求导术，我们可以回头看一些经典的矩阵表达式求导。

首先可以证明如下结论：*任意阶张量的内积、叉积、张量积、并矢运算的求导法则都有类似标量求导的乘积法则*

然后将微分记号d看作一个运算，按照它的线性性质，可以正常做很多四则运算。

考虑如下例子：

张量场$\mathbf{f(X) = X^\mathrm{T}AX}$关于$\mathbf X$求微分，有：

$$
    \mathbf{Df = D(X^\mathrm{T}AX) = D(X^\mathrm{T}A)X+X^\mathrm{T}A\ DX = 2AX\ DX}
$$

（可以说明$\mathbf{D(X^T) = D(X)}$）

上述符号的理解：
- $\mathbf{Df, DX}$都是四阶张量，且笛卡尔坐标系下$\mathbf{DX}$是四阶单位张量$\mathbb I$。
- $\mathbf{ 2AX\ DX}$看起来并不是良定义的，因为我们没有对四阶张量关于二阶张量的“右乘”做定义。但考虑到$\mathbf{Df}$总是将一个二阶张量$\mathbf \Delta X$映射到一个二阶张量，所以这个表达式可以这样理解：$\mathbf{DX}$是所谓的“度量张量”，对于平直时空，这个映射等效$\mathbf{2AX\Delta X}$，这样就是良定义的了。

例2（最小二乘法正规方程）：对$\mathbf{|y - AX|}$关于$\mathbf X$求梯度。

等价与求标量场$\mathbf{(y- AX)\cdot (y-AX)}$的梯度

根据张量数量积的乘积法则，可知$\mathbf{D[(y- AX)\cdot (y-AX)] = [-A\cdot (y-AX)-(y-AX)\cdot A]DX = (2A^TAX-2A^Ty)\ DX}$

可知梯度为：$\mathbf {2A^TAX-2A^Ty}$，令梯度为0得到：$\mathbf{X=(A^TA)^{-1}A^Ty}$，就是线性模型的OLS正规方程解，由此也容易导出非方阵矩阵的彭罗斯伪逆公式