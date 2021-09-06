---
title: LeetCode 5865 线性DP
date: 2021-9-6 8:40:0
categories:
- 算法
  - ACM习题

tags:
- DP

mathjax: true
---
# LeetCode 5865 访问完房间的第一天

<https://leetcode-cn.com/problems/first-day-where-you-have-been-in-all-the-rooms/>

## 思路

**DP + 前缀和**

观察到关键结论：**每个nextVisit访问都是“往回走”的，因此前进只能是因为偶数次到达，因此首次访问位置i时前i-1个位置都是偶数次访问**

从而有了子问题划分的可能。

令`dp[i]`表示第奇数次到达（包括首次到达）第`i`号房间到到达`i+1`号房间所需要的天数，则：
$$
    dp_i = \sum_{k=nextVisit_j}^{i-1} dp_k
$$

由于涉及片段和，考虑`dp2`维护`dp`的前缀，然后利用前缀和转移即可。

注意：由于存在取模，前缀和可能会减出负数，而C++求模是保留符号的，因此要处理符号。

## 代码

```cpp
#define mod 1000000007
class Solution {
public:
    typedef long long ll;
    ll dp[100011];
    int firstDayBeenInAllRooms(vector<int>& nextVisit) {
        dp[0] = 0;
        dp[1] = 2;
        for (int i = 2; i <= nextVisit.size(); i++){
            dp[i] = ((1 + dp[i-1] - dp[nextVisit[i-1]] + 1 + mod) % mod + dp[i-1])%mod;
        }
        int n = nextVisit.size();
        return dp[n-1];
    }
};
```