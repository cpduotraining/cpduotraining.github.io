---
layout: post
title: Bao đóng đồ thị
tags: [dfs and similar]
---
Trong một đồ thị có n đỉnh và m cạnh, trong đó mỗi định đều nối với n-1 đỉnh còn lại, khi này đồ thị có cạnh đầy đủ nhất. Dễ thấy số cạnh lúc này là C2n: (n*(n-1))/2.

**Đề bài**: cho trước đồ thị có n đỉnh và m cạnh, hãy bổ sung số cạnh còn thiếu để các thành phần liên thông trong đồ thị thành bao đóng.

- Nhận xét 1: dễ thấy rằng để tìm số cạnh bổ sung ta cần biết số cạnh của đồ thị đầy đủ theo như công thức đã cho.
- Nhận xét 2: ta cần phải xác định trong từng thành phân liên thông của đồ thị có bao nhiêu cạnh, ta thấy rằng với mỗi đỉnh kể u trong danh sách kề tương ứng là số cạnh, để tính tổng số cạnh trong thành phân liên thông đó ta chỉ việc duyệt hết tất cả các đỉnh trong thành phần liên thông đó và cộng lại tất cả chia cho 2 (  chia 2 khử đi số cạnh bị lặp lại ).

Từ nhận xét (1) và (2) ta có thể giải được bài toán.

**Tham khảo code:**
```c++
#include <bits/stdc++.h>
#define fastIO ios::sync_with_stdio(NULL); cin.tie(NULL);
#define fi "BAODONG.INP"
#define fo "BAODONG.OUT"
 
using namespace std;
const int N = 1E6+7;
 
 
vector<int> adj[N], comp[N];
int n,m;
int comp_cnt = 0;
int edge[N];
deque<int> Q;
bitset<N> visited;
int res = 0;
 
void BFS(int s)
{
    Q.push_back(s);
    visited[s] = true;
 
 while (!Q.empty())
 {
 int tmp = Q.front();
        Q.pop_front();
 
 for (int i = 0;i < (int)adj[tmp].size(); ++i)
 {
 int tmp_2 = adj[tmp][i];
 if (visited[tmp_2] == false)
 {
                visited[tmp_2] = true;
                Q.push_back(tmp_2);
                comp[comp_cnt].push_back(tmp_2);
 }
 }
 }
}
int main()
{
 //freopen(fi,"r",stdin);
 //freopen(fo,"w",stdout);
        fastIO
 
        cin>>n>>m;
 
 for (int i = 1; i<= m;++i)
 {
 int u,v;
            cin>>u>>v;
            adj[u].push_back(v);
            adj[v].push_back(u);
 }
 
 for (int i = 1;i <= n;++i)
 if (visited[i] == false) 
 {
 ++comp_cnt;
 //cout<<i<<"\n";
                comp[comp_cnt].push_back(i);
                BFS(i);
 }
 
 for (int i = 1;i <= comp_cnt;++i) 
 {
 int sum = 0;
 for (int j = 0;j < (int)comp[i].size(); ++j)
 {
 int v = comp[i][j];
                sum = sum + adj[v].size();
 }
            sum = sum / 2;
            edge[i] = sum;
 }
 
 for (int i = 1;i <= comp_cnt; ++i)
 {
 int n_comp = comp[i].size();
 int sz = ((n_comp-1)*n_comp)/2;
 
            res = res + (sz-edge[i]);
 }
 
        cout<<res;
 
}
```
