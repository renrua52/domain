---
title: Educational Codeforces Round 188 题解
date: 2026-03-17
mathjax: true
categories:
  - Math
---

纪念 mer 第一次做出三题，Ziri 决定为此次 [Codeforces Round](https://codeforces.com/contest/2204) 写题解，顺便回忆青春。

## A. Passing The Ball

**题面**. 给定一个由 L 和 R 组成的字符串，开头和结尾分别一定是 L 和 R。从第一个人开始传球，当前持球人如果是 L 就往左传，否则往右传。求有多少人可以拿到球。

**题解**. 签到题。观察到答案是第一个 L 的序号，因为球不可能传给 L 右边的人。

## B. Right Maximum

**题面**. 给定一个整数序列，每步找到其中最大元素（如有多个则取最右边的），删去它及其右边所有数。求序列变空需要的步数。

**题解**. 观察到如下事实：若一个数左侧的数都不大于它，则一定存在某一步选中的数是它；同时，若一个数左侧存在大于它的数，则该数的存在不影响答案（因为它会被左侧的这个数一同删去）。因此只要统计「左侧的数都不大于它」的数的个数即可。

**代码**.

~~~
void solve(){
    int n;
    cin >> n;
    int ans = 0, maxn = 0;
    for(int i = 1; i <= n; ++ i) cin >> a[i];
    for(int i = 1; i <= n; ++ i){
        if(a[i] >= maxn){
            ans ++;
            maxn = a[i];
        }
    }
    cout << ans << "\n";
    return;
}
~~~

## C. Spring

**题面**. 给定整数 $a,b,c,m$。一共有 $m$ 天，在 $a$ 的倍数天 A 去打水（B、C 类似）。每天有 $6$ 桶水，如果该天无人打水则无事发生，若有 $x$ 人打水则打水的人每人打 $6/x$ 桶。求 A、B、C 各自打了多少桶水。

**题解**. 利用容斥原理计算每种打水情况的天数即可。涉及公约数的计算应当使用[欧几里得算法](https://oi-wiki.org/math/number-theory/gcd/#%E6%AC%A7%E5%87%A0%E9%87%8C%E5%BE%97%E7%AE%97%E6%B3%95)。开 `long long`。

**代码**. 

~~~
ll gcd(ll x, ll y){
    if(x < y){
        ll t = x;
        x = y;
        y = t;
    }
    if(x % y == 0) return y;
    return gcd(y, x % y);
}
 
void solve(){
    ll a, b, c, m;
    cin >> a >> b >> c >> m;
    ll ab = a * b / gcd(a, b);
    ll ac = a * c / gcd(a, c);
    ll bc = b * c / gcd(b, c);
    ll abc = ab * c / gcd(ab, c);
    ll cnt_abc = m / abc;
    ll cnt_ab = m / ab - cnt_abc;
    ll cnt_bc = m / bc - cnt_abc;
    ll cnt_ac = m / ac - cnt_abc;
    ll cnt_a = m / a - cnt_ab - cnt_ac - cnt_abc;
    ll cnt_b = m / b - cnt_ab - cnt_bc - cnt_abc;
    ll cnt_c = m / c - cnt_ac - cnt_bc - cnt_abc;
    ll ans_a = cnt_a * 6 + cnt_ab * 3 + cnt_ac * 3 + cnt_abc * 2;
    ll ans_b = cnt_b * 6 + cnt_ab * 3 + cnt_bc * 3 + cnt_abc * 2;
    ll ans_c = cnt_c * 6 + cnt_ac * 3 + cnt_bc * 3 + cnt_abc * 2;
    cout << ans_a << " " << ans_b << " " << ans_c << "\n";
    return;
}
~~~

## D. Alternating Path

**题面**. 给定一无向图，没有自环和重边。给它的每条边赋予方向，可以使它变为有向图。对于这样一个有向图，称它的一个节点 $v$ 为 beautiful，若果它满足如下性质：

> 对于原无向图中从 $v$ 开始的任意一条道路（可以包含环）$(v, v_1, \dots, v_k)$，总是满足：$(v,v_1),(v_2,v_1),(v_2,v_3),(v_4,v_3),\dots$ 都是新的有向图中的边（也称这样的道路为 alternating path）。

考虑所有给原图赋方向的方法，求最多能够得到多少 beautiful 的点。

**题解**. 由于刻意的定义、晦涩的描述，该题在赛后被一致评价为屎题。事实上此题的实质是简单的。首先注意到，原图的各个连通分量是独立的，可以分别计算答案并加和。

我们断言：

> 对于每个连通分量，其中存在 beautiful 点，当且仅当它是二分图。

一个无向图是[二分图](https://oi-wiki.org/graph/bi-graph/)，如果可以将其节点分为两个子集 $X,Y$，使得边仅存在于两子集之间（而不是子集内部）。等价地，可以将图的节点染成两种颜色（$2$-着色），使得相邻节点的颜色不同。可以证明，这也等价于图中不存在长度为奇数的环（[证明](https://oi-wiki.org/graph/bi-graph/#%E5%88%BB%E7%94%BB)）。一个例子如下图：

![](https://oi-wiki.org/graph/images/bi-graph-1.svg)

> **断言的证明**
> 
> 如果连通分量是二分图，只要把所有边的方向都定为从 $X$ 到 $Y$，那么 $X$ 中的点显然全是 beautiful 的。
> 
> 如果不是二分图，则它内部存在长度为奇数的环。考虑图中任一节点 $v$，由于考虑的是连通分量，总可以找到「从 $v$ 进入奇数环，而后绕奇数环两圈」的道路，但「绕奇数环两圈」无论怎么赋方向怎么也不可能是 alternating path。因此任一节点都不是 beautiful 的。

至此，本题的解法就很显然了：只需要对每个连通分量判断它是否是二分图，如果不是则其答案为 $0$；如果是，则其答案为「$2$-着色后，两种颜色的点的个数中较多者」。判断二分图的标准方法之一正是尝试 $2$-着色（使用 bfs 或 dfs 均可），因此可以一边判断二分图，一边尝试着色并累加答案。复杂度 $O(n+m)$。

**代码**. 

~~~
vector<int> g[N]; // 邻接表
bool vis[N];
int color[N] = {0}; // 染色：0为未染色，颜色可以为1或2

int bfs(){
    queue<int> q;
    int ans = 0;
    for(int i = 1; i <= n; ++ i){ // 通过 i 枚举连通分量
        if(vis[i]) continue; // i 属于之前的连通分量（已经考虑过）
        
        int cnt = 0, cnt1 = 0;
        // 新连通分量，从节点 i 开始搜索
        q.push(i);
        color[i] = 1; // 起点染色为 1，尝试交替染色
        bool flag = true; // 该分量是否为二分图

        while(!q.empty()){
            int now = q.front();
            q.pop();
            if(vis[now]) continue;
            vis[now] = 1;

            if(color[now] == 1) cnt1 ++; // 计数颜色 1 的个数
            cnt ++; // 计数连通分量的点的总数

            for(int i = 0; i < g[now].size(); ++ i){ // 遍历 i 的邻居
                int nxt = g[now][i];
                if(color[nxt] && color[nxt] == color[now]) // 染色冲突：nxt 本应染与 now 不同的颜色，但已经被染上了相同颜色
                    flag = false;
                
                if(!vis[nxt]){ // 搜索新的节点
                    q.push(nxt);
                    color[nxt] = 3-color[now]; // 染另一种颜色
                }
            }
        }
        if(flag) ans += max(cnt1, cnt-cnt1); // 是二分图才累加答案
    }
    return ans;
}
 
void solve(){
    cin >> n >> m;
    for(int i = 1; i <= n; ++ i){ // 初始化（因为多测）
        vis[i] = 0;
        color[i] = 0;
        g[i].clear();
    }
    for(int i = 1; i <= m; ++ i){ // 建邻接表
        int u, v;
        cin >> u >> v;
        g[u].push_back(v);
        g[v].push_back(u);
    }
    cout << bfs() << "\n";
}
~~~

## E. Sum of Digits (and Again)

**题面**. 考虑正整数 $x$，定义 $S(x)$ 是如下伪代码构造的字符串：
~~~
S(x) = EmptyStr()
while x >= 10:
    S(x).append(str(x))
    x = SumOfDigits(x)
~~~
其中 `SumOfDigits` 计算正整数的数位和。一个例子是 $S(75)$ 是 `'75123'`。

现在给定由 0~9 数码构成的字符串 $s$（$1\leq |s| \leq 10^5$），保证：通过重排 $s$ 的字符，可以使得对于某个正整数 $x$，$S(x)=s$。求出这个重排。

**题解**. 感觉本题比 D 简单。注意到，由于长度是 $10^5$ 量级，即便是这么多个 $9$ 加在一起，产生的数位和也不过是一个 $6$ 位数。换言之，只要初始的 $x\geq 10$，第一次求数位和的结果（记为 $k$）就至多有 $6$ 位。

这启发我们枚举 $k$，从 $1$ 枚举到 $999999$ 即可。对于给定的 $k$，$S(x)$ 中位于 $k$ 之后的字符串内容也是确定的。剩余没用过的数码都应该位于 $k$ 之前（顺序无所谓），如果它们加起来恰好等于 $k$，则找到了满足要求的重排。复杂度为 $k$ 的量级。

$x<10$ 的情形需要特判（因为不需要计算第一次数位和，也就没有 $k$）。

**代码**. 

~~~
void solve(){
    cin >> s;
    int n = s.size();
    for(int i = 0; i <= 9; ++ i) cnt[i] = 0;

    if(n == 1){
        cout << s << "\n";
        return;
    }

    int tot = 0;
    for(int i = 0; i < n; ++ i){
        int x = s[i] - '0';
        cnt[x] ++;
        tot += x;
    }

    for(int k = 1; k <= 999999; ++ k){
        int r[15] = {0};
        for(int i = 0; i <= 9; ++ i){
            r[i] = cnt[i];
        }

        int now = k, sum = tot;
        bool flag = true; // 是否为可行的重排
        vector<int> ans; // S(x) 中从 k 往后的字符串内容

        while(now > 9){ // 从 now=k 开始尝试循环
            int tmp = now, add = 0;
            while(tmp){
                int dgt = tmp % 10;
                add += dgt;
                if(r[dgt] == 0){ // 要用的数码不够了，已经证明当前 k 不可行
                    flag = false;
                    break;
                }
                sum -= dgt; // 记录没用过的数的和，这里使用了 dgt，所以减去
                r[dgt] --; // 记录每个数码剩多少
                tmp /= 10;
            }
            now = add; // 将 now 更新为 now 的数位和
            ans.push_back(now);
            if(!flag) break;
        }
        if(!flag) continue;

        if(r[now] == 0) continue;
        sum -= now;
        r[now] --;

        if(sum != k) continue;

        // k 可行（把 k 及之后的字符串处理完后，没用过的数加起来确实是 k）

        for(int i = 9; i >= 0; -- i) while(r[i] --) cout << i; // 先输出剩余的数码（都应该在 k 前面，顺序无关）
        cout << k;
        for(int i = 0; i < ans.size(); ++ i) cout << ans[i]; 
        cout << "\n";

        break;
    }
}
~~~