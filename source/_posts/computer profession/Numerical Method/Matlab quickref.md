## Matlab Quick Ref
```matlab
% 百分号表示注释

% 向量书写
vec = [1 2 3]

% 矩阵书写
% 写法1：行内
E = [1 0 0; 0 1 0; 0 0 1]
% 写法2：跨行
E2 = [
    1 0 0
    0 1 0
    0 0 1
]

% 矩阵分片索引：
% 注意：矩阵索引从1开始！
A(:,1,3)

% 定义内置函数：
f = inline('1/(1-exp(-x))', 'x')

% 函数求值：
feval(f, x, y, ...) % 用于把函数当参数传的时候用

% linspace
xx =  -1:0.01:1

% 画图
plot()

% 矩阵乘法
A*B

% 数组逐元素运算
A.*B % 逐元素乘
xx.^2 % 逐元素平方

% 常用矩阵
zeros(n) % 全0方阵
ones(n) % 全1方阵
randn(n,m) % 高斯分布随机矩阵
eye(n) % 单位阵

% 常用矩阵操作
size(A) % 矩阵的形状
length(A) % 向量长度
A' % 转置
det(A) % 行列式
max(A) % 最大值
[max_val, max_idx] = max(A) % 顺便求argmax

% **常用算法**
roots(p) % p是多项式系数，求多项式的根
eig(A) % 特征值
inv (A) % 逆
pinv(A) % 伪逆
[L,U] = lu(A) % LU分解
[L,U,P] = lu(A) % LUP分解

% 求零点
fzero(func, span) % 类似二分法，返回数值解

% 解方程（组）
syms x
eq=x^2+2*x+1;
s=solve(eq,x);

% 多项式拟合
coff = polyfit(x,y,n) % n是多项式的次数，返回多项式系数
polyval(coff,x) % 根据多项式的系数和给定的自变量值，给出计算结果

% 最优化：求最小值
fminsearch(func, x0) 

% 线性插值
vq = interp1(x,y,xq) % xq是指对插值多项式的查询点，返回的是查询点的取值
% 样条
vq2 = interp1(x,y,xq,'spline') 

% 数值积分
integral(fun, xmin, xmax)

% 常微分方程
ode45(odefun, tspan, y0) % tspan是计算的范围，y0是初值
ode15s(odefun, tspan, y0) % 刚性问题


% 矩阵范数
norm(A)

% 循环
for i=start:stop
    % body
end

while (cond)
    % body
end

% 遍历数组
for i=arr
    % body
end

% 分支
if cond
    % body1
elseif cond
    % body2
else
    % body3
end

% 二项分布
binopdf(n,p,k) % 概率密度
binocdf(n,p,k) % 累积密度

% 显示格式设置
format long % 长格式（15位）
format short % 短模式（5位）
grid on % 显示网格

% 函数定义
function [ret1, ret2] = func_name(arg1, arg2)
    % someworks

    % set return value
    ret1 = ...
    ret2 = ...

% 其他
display(...) % 显示
```