---
date: 2021-09-24 15:59:16
title: GDCPC2021 A An easy problem

---
# GDCPC2021 A An easy problem
突破口：
- 网格和搜索空间非常庞大，但第K大的寻找中K取值有限，缩小了搜索空间
知识点：
- BFS
- Hash去重
```c++
#include <iostream>
#include <cstdio>
#include <queue>
#include <unordered_set>
using namespace std;
typedef long long ll;
struct PairHash
{
    ll operator()(const pair<ll,ll>& p)const{
        return p.first * 1000001 + p.second;
    }
};
int main()
{
    int n, m, k; scanf("%d%d%d", &n, &m, &k);
    queue<pair<ll,ll>> q;
    priority_queue<ll, vector<ll>, greater<ll>> heap;
    unordered_set<pair<ll,ll>, PairHash> used;
    q.push(make_pair(n, m));
    heap.push(ll(m)*n);
    used.insert(make_pair(n,m));
    while (!q.empty()){
        auto p = q.front(); q.pop();
//           cout << p.first << ' ' << p.second << endl;
        if (p.first > 1){
            auto p_nxt = p;
            p_nxt.first--;
            if (!used.count(p_nxt)) {
                if (heap.size() < k || p_nxt.first * p_nxt.second > heap.top()){
                    if (heap.size() == k) heap.pop();
                    heap.push(p_nxt.first * p_nxt.second);
                    q.push(p_nxt);
                    used.insert(p_nxt);
                    
                }
            }
        }
        if (p.second > 1){
            auto p_nxt = p;
            p_nxt.second--;
            if (!used.count(p_nxt)){
                if (heap.size() < k || p_nxt.first * p_nxt.second > heap.top()){
                    if (heap.size() == k) heap.pop();
                    heap.push(p_nxt.first * p_nxt.second);
                    q.push(p_nxt);
                    used.insert(p_nxt);
                }
            }
        }
//           cout << heap.top() << endl;
    }
    printf("%lld\n", heap.top());
    return 0;
}
```