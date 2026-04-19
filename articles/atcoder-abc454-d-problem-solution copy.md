---
title: "【AtCoder】ABC454 C問題 解説"
emoji: "🧩"
type: "tech"
topics: ["atcoder", "競技プログラミング"]
published: false
---

# 問題概要
[問題はこちら](https://atcoder.jp/contests/abc453/tasks/abc454_c)

アイテムを1から順番に物々交換していき、持っているアイテムとしてあり得る種類数の最大値を求める問題。

# 制約と考察

n < 3 × 10^5, m < 3 × 10^5 のため、n×mの計算量では間に合わない。

アイテムを交換するので、交換できるアイテムでグラフを構築して解くことが考えられる。
「手に入れることのできる」なので、実際に物々交換をするのではなく、
物々交換をすることで手に入れられるものをカウントしていけばよい。

# 解法
アイテムを頂点としてグラフを考える。
アイテムiとアイテムjを交換できるときiからjにグラフの辺を張る。
アイテム1から行ける頂点の個数が答えとなる。

実装にはdfs(深さ優先探索)とbfs(幅優先探索)どちらでもよいが、
今回はdfsを用いて実装した。

# 実装
```cpp
#include <iostream>
#include <vector>
#include <stack>
using namespace std;

int main()
{
    // 入力
    int n,m;
    cin >> n >> m;

    // グラフを構成
    vector<vector<int>> g(n);
    for(int i = 0; i < m; i++)
    {
        int a,b;
        cin >> a >> b;
        a--,b--;

        g[a].push_back(b);
    }

    stack<int> st; // dfs用stack
    vector<bool> visited(n, false); // 頂点1から行ける頂点
    st.push(0);
    visited[0] = true;

    // dfs
    while(!st.empty())
    {
        int v = st.top(); st.pop();

        for(int nv : g[v])
        {
            if(visited[nv]) continue;
            visited[nv] = true;
            st.push(nv);
        }
    }
    int ans = 0;
    for(int i = 0; i < n; i++) if(visited[i]) ans++;

    // 出力
    cout << ans << endl;
}
```

# まとめ

グラフを考える。