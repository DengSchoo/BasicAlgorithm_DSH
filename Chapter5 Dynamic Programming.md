# 动态规划

动态规划是一种分析思想，可以总结套路，但是套模板没用

![image-20210221191605174](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210221191605174.png)

![image-20210221192224994](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210221192224994.png)





# 背包问题

## 01背包问题

==每个物品只能用 0 | 1次==

![image-20210220214942550](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210220214942550.png)

![image-20210221192916877](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210221192916877.png)

F(i, j)表示从 1~i选取 体积不超过j的最大值。

```c++
#include<iostream>
#include <algorithm>

using namespace std;
int n, m;
int v[N], w[N];
int f[N][N];
int main() {
	cin >> n >> m;
	for (int i = 1; i <= n; i++) cin >> v[i] >> w[i];
    // f[0][0 ~m] = 0， 0件物品
    for(int i = 1; i <= n; i++) {
        for (int j = m; j >= v[j]; j--) {// 倒着循环
            
            f[j] = max(f[j], f[j - v[i]] + w[i]);
        }
    }
    cout << f[m];
}                                       
```

优化：只用到了上一层 f[i - 1]，并且 j 和 j - v[i] 都<= j





## 完全背包问题

==物品可以使用无限次==

![image-20210220215423158](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210220215423158.png)

![image-20210222144007912](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210222144007912.png)



![image-20210222144016116](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210222144016116.png)

状态集合划分：选0个 i和选k个 i。

`f[i][j] =`

![image-20210222144306455](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210222144306455.png)

### 朴素做法

```c++
#include <iostream>
using namespace std;

const int N = 1010;

int v[N], w[N];
int dp[N][N];

int main() {
    
    int n, m; // n是物品种类 m是体积
    cin >> n >> m;
    for (int i = 1; i <= n; i++) cin >> v[i] >> w[i];
    
    for (int i = 1; i <= n; i++) { // 前i个物品
		for (int j = 0; j <= m; j++) { // 体积不超过j
            for (int k = 0; k * v[i] <= j; k++) { // 选取0 ~ k个
                // 状态转移
                f[i][j] = max(f[i - 1][j], f[i - 1][j - k * v[i]] + k * w[i]);
            }
        }
    }
    cout << f[n][m] << endl;
    return 0;
}

```

![image-20210222145609726](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210222145609726.png)

```c++
#include <iostream>
using namespace std;

const int N = 1010;

int v[N], w[N];
int dp[N][N];

int main() {
    
    int n, m; // n是物品种类 m是体积
    cin >> n >> m;
    for (int i = 1; i <= n; i++) cin >> v[i] >> w[i];
    
    for (int i = 1; i <= n; i++) { // 前i个物品
		for (int j = 0; j <= m; j++) { // 体积不超过j
            f[i][j] = f[i - 1][j];
            if (v[i] <= j)f[i][j] = max(f[i][j], f[i][j - v[i]] + w[i]);
        }
    }
    cout << f[n][m] << endl;
    return 0;
}

```



滚动数组优化

```c++
#include <iostream>
using namespace std;

const int N = 1010;

int v[N], w[N];
int f[N][N];

int main() {
    
    int n, m; // n是物品种类 m是体积
    cin >> n >> m;
    for (int i = 1; i <= n; i++) cin >> v[i] >> w[i];
    
    for (int i = 1; i <= n; i++) { // 前i个物品
		for (int j = 0; j <= m; j++) { // 体积不超过j
            // 正着循环
            f[j] = max(f[j], f[j - v[i]] + w[i]);
        }
    }
    cout << f[n][m] << endl;
    return 0;
}

```



## 多重背包问题 -- 数据较小

==每个物品有特定的数量==

![image-20210220215657258](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210220215657258.png)

```c++
#include <iostream>
using namespace std;

const int N = 1010;
int v[N],w[N],s[N];
int f[N][N];



int main() {
    
    int n, m;
    cin >> n >> m;
    
    for (int i = 1; i<= n; i++) cin >> v[i] >> w[i] >> s[i];
    
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j <= m; j++) {
            for (int k = 0; k <= s[i] && j <= k * v[i]; k++) {
                f[i][j] = max(f[i][j], f[i - 1][j - k * v[i]] + k * w[i]);
            }
        }
    }
    cout << f[n][m];
    return 0;
}
```





## 多重背包问题II 是 -- 数据较大

==物品分为多组，每组中有若干水果==

![image-20210222152637975](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210222152637975.png)

不能像完全背包那样优化。

二进制优化

```c++
#include <bits/stdc++.h>

using namespace std;

const int N = 25000, M = 2010;

int n, m;
int v[N], w[N];
int f[N];



int main() {
    cin >> n >> m;
    int cnt = 0;
    for (int i = 1; i <= n; i++) {
        int a, b, s;
        cin >> a >> b >>s;
        int k = 1;
        while(k <= s) {
            cnt++;
            v[cnt] = a * k;
            w[cnt] = b * k;
            s -= k;
            k *= 2;
            
        }
        if (s > 0) {
            cnt++;
            v[cnt] = a * s;
            w[cnt] = b * s;
        }
    }
    n = cnt;
    for (int i = 1; i <= n; i++) {
        for (int j = m; j >= v[i]; j--) {
            f[j] = max (f[j], f[j - v[i]] + w[i]);
        }
    }
    cout << f[m];
}
```



## 分组背包

![image-20210220215632919](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210220215632919.png)

![image-20210222204332714](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210222204332714.png)

```c++
#include <iostream>

using namespace std;

const int N = 110;

int v[N][N], w[N][N], s[N];
int f[N];

int main() {
    
    int n , m;
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> s[i];
        for (int j = 0; j < s[i]; j++) 
            cin >> v[i][j] >>w[i][j];
    }
    for (int i = 1; i <= n; i++) {
        for (int j = m; j>= 0; j--) {
            for (int k = 0; k < s[i]; k++) {
                if (v[i][k] <= j)
                	f[j] = max(f[j], f[j - v[i][k]] + w[i][k]);
                
            }
        }
    }
    cout << f[m] << endl;
    return 0;
}

```





# 线性DP

## 数字三角形

![image-20210225113335911](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210225113335911.png)

![image-20210225114254930](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210225114254930.png)



![image-20210225115017767](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210225115017767.png)

```c++
#include <iostream>
using namespace std;
int n;
const int N = 510, INF = 1e9;
int f[N][N],a[N][N];
int main() {
    cin >> n;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <=  i; i++) {
            cin >> a[i][j];
            
        }
    for (int i = 0; i <= n; i++) {
        for (int j = 0; j <= i; j++) {
            f[i][j] = -INF;
        }
    }
    f[1][1] = a[1][1];
    for (int i = 2; i <= n; i++) {
        for (int j = 1; j <= i + 1; j++) {
            f[i][j] = max(f[i - 1][j - 1] + a[i][j], f[i - 1][j] + a[i][j]);
        }
    }
    int ans = 0;
    for (int i = 0; i < n; i++) {
        ans = max(ans, f[n][i + 1]);
    }
    cout << ans;
    retrurn 0;
}
```



## 最长上升子序列问题

![image-20210225121043261](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210225121043261.png)



```c++
#include <iostream>

using namespace std;
const int N = 1e3 +1;
int f[N];
int a[N];
int main(){
    int n; cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        
    }
    
    for (int i = 1; i <= n; i++) {
        f[i] = 1; // 只有a[i]一个数就是1
        for (int j = 1; j < i; i++) {
            if (a[i] > a[j])
            	f[i] = max(f[i], f[j] + 1);
        }
    }
    
    int ans  = 0;
    for(int i = 1; i <= n; i++) {
        ans = max(ans, f[i]);
    }
    cout << ans;
}
```

### 记录路径

```c++
#include <iostream>

using namespace std;
const int N = 1e3 +1;
int f[N];
int a[N], g[N];
int main(){
    int n; cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        
    }
    
    for (int i = 1; i <= n; i++) {
        f[i] = 1; // 只有a[i]一个数就是1
        for (int j = 1; j < i; i++) {
            if (a[i] > a[j]) {
                if (f[i] < f[j] + 1) {
                    f[i] = f[j] + 1;
                    g[i] = j;
                }
            }
            	
        }
    }
    
    int k  = 1;
    for(int i = 1; i <= n; i++)
        if(f[k] < f[i]) 
            k = i;
    
    
    for (int i = 0, len = f[k]; i < len; i++) {
        cout << a[k];
        k = g[k];
    }
    cout << ans;
}
```







## 最长上升子序列



## 最长公共子序列问题

![image-20210226195549311](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210226195549311.png)

![image-20210226200258388](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210226200258388.png)

![image-20210226201106492](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210226201106492.png)

求最大值问题状态集合划分可以有重叠的部分



```c++
#include <iostream>
using namespace std;
const int N = 1e3 + 3;
int f[N][N];
char a[N], char b[N];
int main(){
    int n, m;
    scanf("%d%d", &n, &m);
    scanf("%s%s", a + 1, b + 1);
    
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            
            f[i][j] = max(f[i][j], f[i - 1, j], f[i][j - 1]);
            if (a[i] == b[j]) { // 第四种情况出现
                f[i][j] = max(f[i][j], f[i - 1][j - 1] + 1);
            }
        }
    }
    cout << f[n][m];
    return 0;
}
```

## 最长上升子序列II -- 优化

![image-20210227194537859](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210227194537859.png)

优化：存放不同长度的最小值。

```c++
#include <iostream>
using namespace std;

const int N = 100010;

int n ;
int a[N];
int q[N];

int main() {
    cin >> n;
    for (int i = 0; i < n; i++) cin >> a[i];
    
    q[0] = -2e9;
    
    int len = 0;
    for (int i = 0; i < n; i++) {
        
        int l = 0, r = len;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (q[mid] < a[i]) l = mid;
            else r = mid - 1;
        }
        
        len = max(len, r + 1);
        q[r + 1] = a[i];
    }
    cout << len;
    return 0;
}
```



## 编辑距离

![image-20210227202427415](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210227202427415.png)



## 编辑距离II



# 区间DP

## 石子合并

![image-20210226203148187](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210226203148187.png)

![image-20210226213003993](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210226213003993.png)

![image-20210226212955184](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210226212955184.png)

将左边k个合并到右边r - l + 1 - k

```c++
#include <iostream>
using namespace std;
const int N = 1000;
int f[N][N];
int s[i];
int main() {
    cin >> n;
    for (int i = 1; i <= n; i++) cin >> s[i];
    
    for (int i = 1; i <= n; i++) s[i] += s[i - 1];
    
    for (int len = 2; len <= n; len++) {
        for (int i = 1; i + len - 1 <= n; i++) {
            int l = i ,r = i + len - 1;
            f[l][r] = 1e8;
            for (int k = l; k < r; k++) {
                f[l][r] = min(f[l][r], f[l][k] + f[k + 1][r] + s[r] - s[l - 1]);
            }
        }
    }
    cout << f[1][n];
    return 0;
}
```







# 计数类DP





# 数位统计DP



# 状态压缩DP



# 树状DP



# 记忆化搜索

```c++
逆序
```

```c++
#include <iostream>
using namespace std;
const int N = 1e5+10;
int a[N];
int v[N];
int main() {
    int n = 0;
    cin >> n;
    
    
    for (int i = 1; i < n - 1; i++) {
        cin >> a[i];
    }
    for (int i = 1; i < n - 1; i++) {
        if(a[i] && a[i - 1]) {
            ans
        }
    }
    
    
}
```

```c++
n用二进制表示
```

