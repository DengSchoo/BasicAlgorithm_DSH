 # 第一章基础算法

## 快速排序

- 确定分界点：q[l] , q[(l +r ) / 2]，q[r] 这三个分界点 
- ==调整区间==
- 递归处理左右两段

i , j 双指针:

- i指针一直向右找 直到 q[i] > x, 停下切换到 j
- j指针一直向左找 知道q[j] < x, 停下
- 交换q[i] , q[j]
- 交换完毕 i++, j-- 向中间移动一格
- 当i > j 结束递归

```c++
#include <cstdio>
#include <iostream>
using namespace std;

const int N = 1e6 + 10;
int q[N];
int n;

void quick_sort(int q[], int l, int r){
    
    if (l >= r) return ;
	// 选取中间为基准
    int x = q[(l+r) >>1], i = l-1, j = r+1;
    while (i < j)
    {
        while (q[++ i] < x);
        while (q[-- j] > x);
        if (i < j) swap(q[i], q[j]);
    }
    quick_sort(q,l, j), quick_sort(q,j+1, r);
    
    
}

int main() {
    
    scanf("%d", &n);
    
    for (int i = 0; i < n; i++) {
        scanf("%d", &q[i]);
    }
    
    quick_sort(q, 0, n - 1);
    
    for (int i = 0; i < n; i++) {
        printf("%d ", q[i]);
    }
    
    return 0;
}

```



## 归并排序

分治 稳定

- 确定分界点： mid = (l + r) / 2;
- 递归排序左边和右边
- 归并，将两个有序的数组合并为一个数组 ****难点

双路归并

```c++
#include<iostream>
using namespace std;

const int N = 1e6 + 10;
int n;
int q[N], temp[N];

void merge_sort(int q[], int l, int r) {
    if (l >= r) return;
    int mid = (l + r) >> 1;
    
    // 先分层 logn层 每层O(n)
    merge_sort(q, l, mid), merge_sort(q, mid + 1, r);
    
    // i指向左半边起点 j指向右半边起点
    int k = 0, i = l, j = mid + 1;
    
    // 开始合并
    while ( i <= mid && j <= r)
        if (q[i] <= q[j]) temp[k++] = q[i++];
    	else temp[k++] = q[j++];
    // 剩余情况 左半边没有循环完毕继续循环 ..
    while (i <= mid ) temp[k++] = q[i++]; 
    while (j <= r) temp[k++] = q[j++];
    
    // 重新放回到q
    for (i = l, j = 0; i <= r; i++, j++) q[i] = temp[j];
}

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i++) scanf("%d", &q[i]);
    merge_sort(q, 0, n - 1);
    for (int i = 0; i < n; i++) printf("%d ", q[i]);
    return 0;
}



```



## 整数二分

划分区间：选择答案所在的区间，每次将区间划分为一半

```c++
#include <iostream>
using namespace std;
const int N = 1e6 + 10;
int n,m;
int a[N];
int main() {
    
	scanf("%d%d", &n, &m);
    
    for (int i = 0; i < n; i++) scanf("%d", &a[i]);
    
    while (m -- ){
        int x;
        scanf("%d", &x);
        int l = 0, r= n - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (q[mid] >= x) r = mid;  // 边界点在左边 在mid左半边
            else l = mid + 1; //在mid右半边
        }
        
        // 无解 没有找到等于x的
        if (q[l] != x) cout << "-1 -1" <<end;
        
        else {
            cout << l << " ";
            int l = 0, r= n - 1;
            while (l < r) {
                int mid = l + r + 1 >> 1;
                if (q[mid] <= x) l = mid;  // 在mid右半边
                else r = mid - 1; //在mid左半边
            }
            cout << l << endl;
        }
    }
    return 0;
}
```





## 浮点数二分

浮点数至少要比有效小数多两位 ：要求保留5位就是 精度为7位

```c++
#include <iostream>
using namespace std;
int main() {
    double x;
    double l = 0,  r = x;
    // for (int i = 0; i < 100; i++) // 直接迭代一百次
    while (r - l > 1e-8) {
        double mid = ( l + r) / 2;
        if (mid * mid >= x) r = mid;
        else l = mid;
    }
    printf("%lf", l);
    return 0;
}
```

### 三次方根

```c++
#include <iostream>
using namespace std;
int main() {
    double x;
    scanf("%lf", &x);
    
    double l ,r;
    
    if (x > 0) { // 
         l = 0,  r = x;
        while (r - l > 1e-8) {
        
            double mid = ( l + r) / 2;
            
            if (mid * mid * mid >= x) r = mid;
            else l = mid;
            
        }
        
    } else {
         l = x,  r = 0;
        while (r - l > 1e-8) {
        
            double mid = ( l + r) / 2;
            
            if (mid * mid * mid >= x) r = mid;
            else l = mid;
            
        }
    }
    
    printf("%lf", l);
    return 0;
}
```



## 高精度

```c++
#include <iostream>
#include <string>
#include <vector>

using namespace std;

// 两个高精度整数相加
vector<int> add (vector<int> &A, vector<int> &B) {
    vector<int> ans;
    int t = 0;
    for (int i = 0; i < A.size() || i < B.size(); i++) {
        if (i < A.size()) t += A[i];
        if (i < B.size()) t += B[i];
        ans.push_back(t % 10);
        t /= 10;
    }
    if (t) ans.push_back(1);

    return ans;
}


// A, B非负 并且 A.size() > B.size()
vector<int> sub (vector<int> &A, vector<int> &B) {
    vector<int> ans;

    int t = 0;
    for (int i = 0; i < A.size(); i++) {
        t = A[i] - t;
        if(i < B.size()) t -= B[i];
        ans.push_back((t + 10) % 10);
        if (t < 0) t = 1; //判断借位
        else t = 0;
    }
    while(ans.size() > 1 && ans.back() == 0){
        ans.pop_back();
    }

    return ans;
}

// A / b,  商 是c 余数是 r
vector<int> div(vector<int> A, int b, int &r) {
    vector<int> ans;
    r = 0;
    for (int i = A.size() - 1; i >= 0; i--) {
        r = r * 10 + A[i];
        ans.push_back(r / b);
        r %= b;
    }
    reverse(ans.begin(), ans.end());
    while (ans.size() > 1 && ans.back() == 0) ans.pop_back();
    return ans;
}



int main () {
    string a;
    string b;
    vector<int> A, B, ans;
    cin >> a >> b;
    for (int i = a.size() - 1; i >= 0; i--) A.push_back(a[i] - '0');
    for (int i = b.size() - 1; i >= 0; i--) B.push_back(b[i] - '0');
    ans = add(A, B);
    for (int i = ans.size() - 1; i >= 0; i--) {
        printf("%d", ans[i]);
    }
    return 0;
}
```

## 前缀和



```c++
#include <iostream>

using namespace std;

const int N = 100010;

int n, m;
int a[N], s[N];

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &a[i]);

    for (int i = 1; i <= n; i ++ ) s[i] = s[i - 1] + a[i]; // 前缀和的初始化

    while (m -- )
    {
        int l, r;
        scanf("%d%d", &l, &r);
        printf("%d\n", s[r] - s[l - 1]); // 区间和的计算
    }

    return 0;
}

```



### 二维子矩阵的和

```c++
#include <iostream>

using namespace std;

const int N = 1010;

int n, m, q;
int s[N][N];

int main()
{
    scanf("%d%d%d", &n, &m, &q);

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            scanf("%d", &s[i][j]);

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            s[i][j] += s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1];

    while (q -- )
    {
        int x1, y1, x2, y2;
        scanf("%d%d%d%d", &x1, &y1, &x2, &y2);
        printf("%d\n", s[x2][y2] - s[x1 - 1][y2] - s[x2][y1 - 1] + s[x1 - 1][y1 - 1]);
    }

    return 0;
}
```

## 差分

对于数组a，构造数组b，使得数组`bn = an - an-1` 即 `ai = b1+b2+..+bi`。前缀和数组的逆运算

有b数组就可以有O(n)的时间得到a数组。

应用：在a数组的 [l, r]之间的数全部 + C, 

```c++
#include <iostream>

using namespace std;

const int N = 100010;

int n, m;
int a[N], b[N];

void insert(int l, int r, int c)
{
    b[l] += c;
    b[r + 1] -= c;
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &a[i]);

    for (int i = 1; i <= n; i ++ ) insert(i, i, a[i]);

    while (m -- )
    {
        int l, r, c;
        scanf("%d%d%d", &l, &r, &c);
        insert(l, r, c);
    }

    for (int i = 1; i <= n; i ++ ) b[i] += b[i - 1];

    for (int i = 1; i <= n; i ++ ) printf("%d ", b[i]);

    return 0;
}

```

### 二维差分

```c++
#include <iostream>

using namespace std;

const int N = 1010;

int n, m, q;
int a[N][N], b[N][N];

void insert(int x1, int y1, int x2, int y2, int c)
{
    b[x1][y1] += c;
    b[x2 + 1][y1] -= c;
    b[x1][y2 + 1] -= c;
    b[x2 + 1][y2 + 1] += c;
}

int main()
{
    scanf("%d%d%d", &n, &m, &q);

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            scanf("%d", &a[i][j]);

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            insert(i, j, i, j, a[i][j]);

    while (q -- )
    {
        int x1, y1, x2, y2, c;
        cin >> x1 >> y1 >> x2 >> y2 >> c;
        insert(x1, y1, x2, y2, c);
    }

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            b[i][j] += b[i - 1][j] + b[i][j - 1] - b[i - 1][j - 1];

    for (int i = 1; i <= n; i ++ )
    {
        for (int j = 1; j <= m; j ++ ) printf("%d ", b[i][j]);
        puts("");
    }

    return 0;
}

```

















































































