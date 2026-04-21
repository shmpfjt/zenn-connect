---
title: "【AtCoder】ABC454 C問題 解説"
emoji: "🧩"
type: "tech"
topics: ["atcoder", "競技プログラミング"]
published: false
---

この記事では、ABC454 C問題をグラフとして捉え、到達可能な頂点数を数えることで解く方法を解説します。

# 問題概要
[問題はこちら](https://atcoder.jp/contests/abc454/tasks/abc454_c)

アイテム1を持った状態からスタートし、与えられた交換ルールに従って、他のアイテムと交換していく。

このとき、最終的に「手に入れることができるアイテムの種類数」の最大値を求める問題。

# 制約と考察

n, m ≤ 3 × 10^5 と大きいため、すべての交換パターンを試すことはできない。

ここで、「アイテムを交換できる関係」は、
「あるアイテムから別のアイテムへ移動できる」と考えられる。

したがって、アイテムを頂点、交換可能な関係を辺とするグラフとして扱うことができる。
交換は単方向（Aを渡したときのみBが得られる）であるため、グラフは有向グラフとして扱う。

このとき、「アイテム1から到達可能な頂点の数」が、手に入れることのできるアイテム数に対応する。

したがって、頂点1から到達可能な頂点数を数えればよい。

計算量は O(N + M) であり、制約内で十分高速に実行できる。

# 解法

実装にはDFS（深さ優先探索）やBFS（幅優先探索）が使える。

どちらも「到達可能な頂点を列挙する」という目的は同じであり、
今回は一例としてDFSを用いる。

# 実装
以下にDFSを用いた実装例を示す。

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

- 交換関係はグラフとして扱うことができる
- 有向グラフとして「到達可能な頂点数」を数える問題に帰着できる
- DFSやBFSで効率よく解くことができる

問題をグラフとして捉えることで、大規模データでも効率的に処理できる。