# 搜索与图论算法

# DFS

关键概念：回溯、剪枝

![image-20210213203818314](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210213203818314.png)

不具有最短性

### 全排列数

```c++
#include <iostream>

using namespace std;

const int N = 110;


int n;
int path[N];
bool st[N];


void dfs(int u) {
	if (u == n) {
        // cout
        return ;
    }
    
    for(int i = 0; i < n; i++) {
        if (!st[i]) {
            path[u]  = i;
            st[i] = 1;
            dfs(u + 1);
            //path[u] = 0;
            st[i] = 0; // 恢复现场
        }
    }
    
}


int main () {
    
}
```



### n皇后问题

可以是来全排列然后计算合法性。

```c++
#include <iostream>

using namespace std;

const int N = 20;


int n;
char g[N][N];
int path[N];
bool col[N], dg[N], udg[N];
void dfs(int u) {
	if (u == n) {
        // cout 输出方案
        for (int i = 0; i < n; i++) puts(g[i]);
        return ;
    }
    
    for(int i = 0; i < n; i++) {
        // 二维坐标计算对角线
        if (!col[i] && !dg[u + i] && !udg[n - u + i]) {
            g[u][i]  = 'Q';
            col[i] = dg[u + i] = udg[n - u + i] = true;
            st[i] = 1;
            dfs(u + 1);
            //path[u] = 0;
            col[i] = dg[u + i] = udg[n - u + i] = false; // 恢复现场
            g[u][i]  = '.';
        }
    }
    
}




```

方法二 ：

![image-20210213210550239](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210213210550239.png)



# BFS

![](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210213203818314.png)

好处：第一次搜索到的点就为最近的点。最短路。

![image-20210213210920035](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210213210920035.png)

### 走迷宫--最少步数

```c++
#include <iostream>
#include <queue>
using namespace std;

typedef pair<int, int> PII;

 int n, m;

const int N = 20;

int g[N][N]; // 存放地图

int d[N][N]; // 存放每个点到起点的距离

int bfs() {
    int hh = 0
}



```

![image-20210213211322714](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210213211322714.png)

![image-20210213211538121](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210213211538121.png)

记录路径：记录每一个点的前一个点过来的。



# 图（树是特殊的图）的存储

![image-20210213211826605](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210213211826605.png)

节点存放：该点能走到的点的集合，次序无关

![image-20210213212012221](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210213212012221.png)



# 树与图的DFS

![image-20210213212723427](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210213212723427.png)



可以计算每个子树的大小





# 树与图的BFS

![image-20210214080037184](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210214080037184.png)

# 拓扑排序

有向无环图，一定会存在一个拓扑序列。

拓扑排序得到的序列，其在有向图的指向（任意两个方向）为同一个方向。

每次入度为0都可以作为起点。

![image-20210214080910807](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210214080910807.png)

![image-20210214080933991](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210214080933991.png)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

const int N = 100010;
int head[N], e[N], ne[N], idx;

void add(int a, int b) {
    e[idx] = b, ne[idx] = head[a], head[a] = idx++;
}

bool topsort() {
    int hh = 0, tt = -1;
    for(int i = 1; i<= n; i++) {
        if (!d[i])
            q[++tt] = i;
        
    }
    while(hh <= tt) {
        int t = q[hh++];
        
        for (int i = h[t]; i != -1; i= ne[i]) {
            int j = e[i];
            d[j]--;
            if (d[j] == 0) q[++tt] = j
        }
    }
    return tt == n  - 1;
}

int main() {
    cin >> n >> m;
    
    memset(h , -1, sizeof h);
    
    for (int i = 0; i < m; i++) {
        int a, b;
        cin >> a >> b;
        add(a, b);
        d[b]++; //更新入度
    }
    if (topsort()) { // 可以判断是否有拓扑排序
        
    } else puts("-1");
}
```





# 最短路

![image-20210214194118119](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210214194118119.png)



## Dijkstra O(n ^ 2) 掌握

### 朴素版

![image-20210214200102089](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210214200102089.png)

S集合存放已经使用过的点

![image-20210214200556299](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210214200556299.png)

稠密图使用邻接矩阵，稀疏图使用邻接表

```c++
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 510;

int n, m;
int g[N][N];
int dist[N];
bool st[N];

int dijkstra()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    for (int i = 0; i < n - 1; i ++ )
    {
        int t = -1;
        for (int j = 1; j <= n; j ++ )
            if (!st[j] && (t == -1 || dist[t] > dist[j]))
                t = j;

        for (int j = 1; j <= n; j ++ )
            dist[j] = min(dist[j], dist[t] + g[t][j]);

        st[t] = true;
    }

    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}

int main()
{
    scanf("%d%d", &n, &m);

    memset(g, 0x3f, sizeof g);
    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);

        g[a][b] = min(g[a][b], c);
    }

    printf("%d\n", dijkstra());

    return 0;
}

```





### 堆优化

```c++
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

typedef pair<int, int> PII;

const int N = 1e6 + 10;

int n, m;
int h[N], w[N], e[N], ne[N], idx;
int dist[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

int dijkstra()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    priority_queue<PII, vector<PII>, greater<PII>> heap;
    heap.push({0, 1});

    while (heap.size())
    {
        auto t = heap.top();
        heap.pop();

        int ver = t.second, distance = t.first;

        if (st[ver]) continue;
        st[ver] = true;

        for (int i = h[ver]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > dist[ver] + w[i])
            {
                dist[j] = dist[ver] + w[i];
                heap.push({dist[j], j});
            }
        }
    }

    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}

int main()
{
    scanf("%d%d", &n, &m);

    memset(h, -1, sizeof h);
    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c);
    }

    cout << dijkstra() << endl;

    return 0;
}

```





## bellman-ford O(nm)

存在负权回路的话，最短路不一定存在。

可以找负环。

![image-20210216214611128](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210216214611128.png)

![image-20210216214714357](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210216214714357.png)

更新过程：松弛操作。

![image-20210216214840237](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210216214840237.png)

对于所有的边：三角不等式

![image-20210216214803920](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210216214803920.png)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 510, M = 100010;

int n, m, k;

int dist[N], backup[N];
struct Edge{
    int a, b, w;
    
}edges[M];

int bellman-ford() {
    memset(dist, 0x3f, sizeof dist);
    for (int i = 0; i < k; i++) {
        // 可能存在串联 所有只用上一次的结果
        memcpy(backup, dist, sizeof dist);
        for (int j = 0; j < m; j++) {
            int a = edges[j].a, b = edges[i].b, w = edges[j].w;
            dist[b] = min (dist[b], backup[a] + w);
        }
    }
    // 可能到不了 但是可能减去了一些数
    if (dist[n] > 0x3f3f3f3f / 2) return -1;
    return dist[n];
}


int main () {
    scanf("%d%d%d", &n, &m, &k);
    
     for (int i = 0; i < m; i++) {
         int a, b, w;
         scanf("%d%d%d", &a, &b, &w);
         edges[i] = {a, b , w};
     }
    
    int t = bellman-ford();
    
    if (t == -1) puts("impossible");
    else cout << t << endl;
    
    
    return 0;
}
```





## spfa 一般O(m), 最坏O(n, m)重点掌握

没有负环都可以用spfa， %99没有负环.

判断负环一般使用spfa。

对bellman-ford算法做了优化。

![image-20210216221317390](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210216221317390.png)

判断dist[a]是否变小了。

新增队列：里面存放减小的dist[a];

![image-20210216221906752](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210216221906752.png)



![image-20210216221919744](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210216221919744.png)



```c++
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

typedef pair<int, int> PII;

const int N = 1e6 + 10;

int n, m;
int h[N], w[N], e[N], ne[N], idx;
int dist[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

int spfa()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    st[1] = true; // 存储当前数组是否在队列当中
    
    while(q.size()) {
        int q = q.fornt();
        q,pop();
        st[t] = false;
        
        for (int i = h[t]; i!= -1; i = ne[i]) {
            int j = e[i];
            if (dist[j] > dist[t] + w[i]) {
                dist[j] = dist[t] + w[i];
                
                if (!s[t]) { // 加入到数组当中
                    q.push(t);
                    st[j] = true;
                }
            }
        }
        
    }
    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}

int main()
{
    scanf("%d%d", &n, &m);

    memset(h, -1, sizeof h);
    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c);
    }

    int t = spfa();
    

    return 0;
}

```

网格图可能卡nm，一般没事。



### 求负环：抽屉原理

新增cnt数组，表示到达该点的边数

![image-20210216223405276](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210216223405276.png)

更新：

![image-20210216223436389](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210216223436389.png)

==cnt[x] >= n==表示至少经过了n条边，经过了n+ 1个点，因为抽屉原理，所有至少两个点是相同的，并且一定是负环。

```c++
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

typedef pair<int, int> PII;

const int N = 1e6 + 10;

int n, m;
int h[N], w[N], e[N], ne[N], idx;
int dist[N];
bool st[N];
int cnt[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

int spfa()
{
    queue<int> q;
    // 判断是否存在负环 需要把所有点加入而不是单独一个点。
    for (int i = 1; i <= n; i++) {
        st[i] = true;
        q.push(i);
    }
    
    while(q.size()) {
        int q = q.fornt();
        q,pop();
        
        st[t] = false;
        
        for (int i = h[t]; i!= -1; i = ne[i]) {
            int j = e[i];
            if (dist[j] > dist[t] + w[i]) {
                dist[j] = dist[t] + w[i];
                cnt[j] = cnt[t] + 1;
                if (cnt[j] >= n) return true;
                if (!s[t]) { // 加入到数组当中
                    q.push(t);
                    st[j] = true;
                }
            }
        }
        
    }
    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}

int main()
{
    scanf("%d%d", &n, &m);

    memset(h, -1, sizeof h);
    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c);
    }

    if (spfa()) puts("yes");
    else 
    

    return 0;
}

```



## Floyed O(n ^ 3)

基于动态规划。

可以处理负权，但是不能处理负环。

![image-20210217195618625](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210217195618625.png)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

int n, m, Q;
const int N = 210, INF = 1e9;


int d[N][N];

void floyd() {
    for (int k = 1; k <= n; k++) {
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
            }
        }
    }
}


int main () {
    cin >> n >> m >> Q;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            if (i == j) d[i][j] = 0;
            else d[i][j] = INF;
        }
    }
    while(m --) {
        int a, b ,w;
        cin >> a >> b >> w;
        d[a][b] = min(d[a][b], w);
    }
    
    floyd();
    
    
    // 对d进行处理
    if (d[a][b] > INF / 2) puts("impossible")
    return 0;
}
```





# 最小生成树

![image-20210217202002417](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210217202002417.png)

## Prim

不分正负权。

![image-20210217203853155](C:\Users\联想\AppData\Roaming\Typora\typora-user-images\image-20210217203853155.png)



```c++
#include <iostream>
#include <cstring>

using namespace std;

const int N = 510, INF = 0x3f3f3f3f;
int n , m;
int g[N][N];
int dist[N];
bool st[N];

int prim() {
    memset(dist, ox3f, sizeof dist);
    int res = 0;
    for (int i = 0; i < n; i++) {
        int t = -1;
        for (int j = 1; j <= n; j++) {
            if (!st[j] == - 1 || dist[t] > dist[j])
                t = j;
        }
        if (i && dist[t] == INF) return -1;
        
        // 防止在自环前 先累加再更新
        if (i) res += dist[t];
        st[t] = true;
        
        // 更新其它结点
        for (int j = 1; j <= n; j++) {
            // dist[t]表示该点到集合的距离
            dist[j] = min (dist[j], g[t][j]);
        }
        
        
    }
    return res;
}

int main () {
    cin >> n >> m;
    memset(g, 0x3f, sizeof g);
    
    
    while(m --) {
        int a, b, c;
        cin >> a >> b >> c;
        g[a][b] = g[b][a] = min(g[a][b], c);
        
    }
    
    int t = prim();
    if (t == INF) puts("impossible"); // 所有点不联通
}
```







## Kruskal



# 二分图

## 染色法 - DFS O(n + m)

## 匈牙利算法 - 求二分图的最大匹配 bad:O(mn)



