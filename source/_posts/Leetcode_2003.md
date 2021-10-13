---
abbrlink: '0'
date: 2021-09-14 16:59:12
---
# Leetcode 2003 DFS, Mex
<https://leetcode-cn.com/problems/smallest-missing-genetic-value-in-each-subtree/>

## 思路
这是一类称为*Mex*的问题，指“求集合中未出现的最小自然数”。

通常这种问题的解决方案是“主席树”，但是本题有特殊条件：
- **数字不重复**
- 值域较小

数字不重复导致我们有如下观察：**若整棵树都不出现1，则所有的子树的结果都是1**

这一观察导出了非常巧妙的解法：
- DFS一遍找到“1”所在的节点
- **只处理“1”节点到根节点的链，更新不到的点的结果都是1**
- 更新链的策略
  - 整个过程中维护一个数字出现表和当前最小未出现数字（称为Mex）
  - 从“1”节点往回走，链上的节点的孩子都要DFS一遍更新表和Mex
  - **由于当前最小未出现数字在从叶子回溯的过程中是单调非减的**，因此更新Mex的时间开销至多是O(结点数+值域大小)，是线性的

## 分析
- 找“1”节点是一遍DFS，$O(n)$
- 更新链上节点的孩子都是DFS，且不重复，因此总的也是$O(n)$

综上，该算法时间复杂度线性（由于使用主席树的通解）。
空间复杂度是值域的线性。

## 代码
```cpp
class Solution {
public:
    vector<vector<int>> sons;
    vector<int> fa;
    vector<int> val;
    int n;
    vector<int> smallestMissingValueSubtree(vector<int>& parents, vector<int>& nums) {
        n = parents.size();
        sons.clear();
        sons.resize(n);
        fa = move(parents);
        val = move(nums);
        for (int i = 0; i < fa.size(); i++){
            if (fa[i] != -1){
                sons[fa[i]].push_back(i);
            }
        }
        int node1 = dfs1(0);
        vector<int> ans(n, 1);
        vector<bool> t(1e5+1);
        if (node1 == -1) return ans;
        else{
            int p = 1;
            dfs2(node1, -1, ans, t, p);
            return ans;
        }
        
    }
    int dfs1(int u){
        if (val[u] == 1){
            return u;
        }
        int res = -1;
        for (auto v: sons[u]){
            res = dfs1(v);
            if (res != -1) return res;
        }
        return -1;
    }
    void update(int u, vector<bool>& t, int& p){
        t[val[u]] = true;
        for(auto v: sons[u]){
            update(v, t, p);
        }
        while (p < t.size() && t[p]) p++;
    }
    void dfs2(int u, int last, vector<int>& ans, vector<bool>& t, int& p){
        t[val[u]] = true;
        while (p < t.size() && t[p]) p++;
        for (auto v: sons[u]){
            if (v != last){
                update(u, t, p);
            }
        }
        ans[u] = p;
        if (u == 0) return;
        else dfs2(fa[u], u, ans, t, p);
    }
    
};
```