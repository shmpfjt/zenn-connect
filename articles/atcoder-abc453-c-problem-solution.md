---
title: "【AtCoder】ABC453 C問題 解説"
emoji: "🧩"
type: "tech"
topics: ["atcoder", "競技プログラミング"]
published: false
---

# 問題概要
Sneaking Glances

数直線上の座標 0.5 に高橋君がいます。

高橋君はこれからN回の移動を行います。
i回目の移動では、「正の方向」「負の方向」のいずれかを選び、その方向に$L_i​$進みます。

高橋君は座標 0 を最大で何回通り過ぎることが出来るでしょうか？
なお、この問題の制約上、座標 0 で完了する移動が生じることはありません。

# 制約と考察
制約

$1≤N≤20$

$1≤L_i​≤10^9$

入力はすべて整数

Nが小さいことにより、bit全探索が有効？

# 解法
貪欲法ではダメ。
bit全探索で解く。
0.5をどう扱うか

# 実装
```cpp
#include <iostream>
#include <vector>
using namespace std;

int main()
{
    int n;
    cin >> n;

    int ans = 0;
    vector<int> l(n);
    for(int i = 0; i < n; i++) cin >> l[i];
    for(int bit = 0; bit < (1<<n); bit++)
    {
        long long now = 0;
        int cnt = 0;
        for(int i = 0; i < n; i++)
        {
            bool p = now >= 0;
            if(bit & (1<<i)) now += l[i];
            else now -= l[i];
            if(p && now < 0) cnt++;
            else if(!p && now >= 0) cnt++;
        }
        ans = max(ans, cnt);
    }
    cout << ans << endl;
}