# 数学知识

# 数论

## 质数

大于1的自然数，才有质数的概念。

定义：在大于的1的整数中，如果只包含1和本身这两个约数为质数或者叫素数。

### 质数的判定---试除法

```c++
bool is_prime(int n) {
    if (n < 2) return false;
    // i <= n / i 是优化
    for (int i = 2; i <= n / i; i++) {
        if (n % i == 0) return false;
    }
    reutrn true;
}
```

### 分解质因数---试除法

n中最多只包含一个大于sqrt(n)的质因子

```c++
void divide(int n) {
    for (int i = 2; i <= n / i; i++) {
        if (n % i == 0) {
			int s = 0;
            while(n % i == 0) {
                n/=i;
                s++;
            }
            // 质因子i(底数) 对应的质数
            cout << i << s << endl;
        }
    }
    
    if (n > 1) cout << n << 1;
}
```

### 筛质数---埃氏筛O(nloglogn)

按照元素倍数来筛元素。

质数定理: 1 - n 当中有n / lnn个质数

![image-20210218095137442](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210218095137442.png)

```c++

int primes[N], cnt;
bool st[N];

void get_prime(int n) {
    for (int i = 2; i <= n; i++) {
        if (!st[i]) {
            prime[cnt++] = n;
            for (int j = i + i; j <= n; j += i) st[j] = true;
        }
    }
}
```



## 约数

 ### 线性筛法--比埃氏筛法快

n只会被最小质因子筛掉

```c++
const int N = 1000010;
int primes[N], cnt;
bool st[N]; // 记录哪个元素被筛掉了


void get_prime(int n) {
    for (int i = 2; i <= n; i++) {
        if (!st[i])
            prime[cnt++] = i;
        for (int j = 0; primes[j] <= n / i; j++) {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0) break; // primes[j]是i的最小质因子
        }
        
    }
}
```

## 约数

### 试除法求所有约数

```c++

vector<int> get_divisors(int n) {
    vector<int> res;
    for (int i = 1; i <= n / i; i++) {
        if (n % i == 0) 
            res.push_back(i);
        // 一个边界条件
        if (i != n / i) res.push_back(i);
    }
    sort(res.begin(), res.end());
    return res;
}
```



### 求约数个数

 约数个数int范围最多1500+

![image-20210218102146650](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210218102146650.png)

```c++
#include <iostream>
#include <cstring>
#include <unordered_map>
using namespace std;
typedef long long LL;
const int N = 100010, mod = 1e9 + 7;

int main() {
    int n;
    cin >> n;
    unordered_map<int, int> primes;
    while(n --) {
        int n;
        cin >> x;
        for (int i = 2; i <= x / i; i++) {
            while(x % i == 0) {
                x /= i;
                primes[i]++;
            }
        }
        if (x > 1) primes[x] ++;
    }
    LL res = 1;
    for (auto prime : primes) res = res * (prime.second + 1) % mod;
    cout << res;
}
```



### 求约数之和

![image-20210218102903193](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210218102903193.png)

```c++
#include <iostream>
#include <cstring>
#include <unordered_map>
using namespace std;
typedef long long LL;
const int N = 100010, mod = 1e9 + 7;

int main() {
    int n;
    cin >> n;
    unordered_map<int, int> primes;
    while(n --) {
        int n;
        cin >> x;
        for (int i = 2; i <= x / i; i++) {
            while(x % i == 0) {
                x /= i;
                primes[i]++;
            }
        }
        if (x > 1) primes[x] ++;
    }
    LL res = 1;
    for (auto prime : primes) {
        int p = prime.first;
        int a = prime.second;
        
        LL t = 1;
        while (a --) t = (t * p + 1) % mod;
        res = res * t % mod;
    }
    cout << res;
}

```



### 欧几里得算法（辗转相除法）-- 求最大公约数

![image-20210218104811048](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210218104811048.png)

![image-20210218104820189](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210218104820189.png)

```c++
int gcd(int a, int b) {
    return b ? gcd(b, a mod b) : a;
}
```



## 欧拉函数

![image-20210218195150800](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210218195150800.png)

```c++
#include <iostream>
#include <cstring>

using namespace std;

int main () {
    int n;
    cin >> n;
    
    int res = a;
    for (int i = 2; i <= a / i; i++) {
        if (a % i == 0) {
            res = res / i * (i - 1); // res =  res * (1 - 1/ a);
            while(a % i == 0) a/= i;
        }
    }
    if (a > 1) res = res / a * (a - 1);
    cout << res << endl;
}
```

### 筛法求欧拉函数

```c++
#include <iostream>

using namespace std;

typedef long long LL;

const int N = 1000010;


int primes[N], cnt;
int euler[N];
bool st[N];


void get_eulers(int n)
{
    euler[1] = 1;
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i])
        {
            primes[cnt ++ ] = i;
            euler[i] = i - 1;
        }
        for (int j = 0; primes[j] <= n / i; j ++ )
        {
            int t = primes[j] * i;
            st[t] = true;
            if (i % primes[j] == 0)
            {
                euler[t] = euler[i] * primes[j];
                break;
            }
            euler[t] = euler[i] * (primes[j] - 1);
        }
    }
}


int main()
{
    int n;
    cin >> n;

    get_eulers(n);

    LL res = 0;
    for (int i = 1; i <= n; i ++ ) res += euler[i];

    cout << res << endl;

    return 0;
}
```



### 欧拉定理

![image-20210218214351841](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210218214351841.png)





## 快速幂

解决：快速的求出 ==a^k mod p==, O(logk)

![image-20210218220157662](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210218220157662.png)

```c++
#include <iostream>

#include <algorithm>

using namespace std;

typedef long long LL;
int n;

// a ^ k % p
int qmi(int a, int k, int p) {
    int res = 1;
    while(k) {
        
        if (k & 1) res = (LL) res * a % p;
        k >>= 1;
        a = (LL)a * a % p;
    }
    return res;
}

int main (){
    scanf("%d", &n);
    while(n --) {
        int a, k, p;
	    scanf("%d%d%d", &a, &k, &p);
        printf("%d", qmi(a,k,p));
    }
    return 0;
}
```

### 快速幂求逆元

![image-20210219113019336](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210219113019336.png)

费马定理：

![image-20210219113113462](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210219113113462.png)



## 扩展欧几里得算法

![image-20210219113801078](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210219113801078.png)

### 裴蜀定理

![image-20210219113942754](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210219113942754.png)

(a, b)最大公约数

![image-20210219115413452](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210219115413452.png)

```c++
#include <iostream>
#include <algorithm>

using namespace std;

int exgcd(int a, int b, int &x, int &y)
{
    if (!b)
    {
        x = 1, y = 0;
        return a;
    }
    int d = exgcd(b, a % b, y, x);
    y -= a / b * x;
    return d;
}

int main()
{
    int n;
    scanf("%d", &n);

    while (n -- )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        int x, y;
        exgcd(a, b, x, y);
        printf("%d %d\n", x, y);
    }

    return 0;
}

```

### 线性同余方程

![image-20210219115734946](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210219115734946.png)

![image-20210219115949574](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210219115949574.png)





 gcd(a, m)能整除b

```c++
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;


int exgcd(int a, int b, int &x, int &y)
{
    if (!b)
    {
        x = 1, y = 0;
        return a;
    }
    int d = exgcd(b, a % b, y, x);
    y -= a / b * x;
    return d;
}


int main()
{
    int n;
    scanf("%d", &n);
    while (n -- )
    {
        int a, b, m;
        scanf("%d%d%d", &a, &b, &m);

        int x, y;
        int d = exgcd(a, m, x, y);
        if (b % d) puts("impossible");
        else printf("%d\n", (LL)b / d * x % m);
    }

    return 0;
}

```





## 中国剩余定理

![image-20210219121358734](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210219121358734.png)

![image-20210219121409303](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210219121409303.png)

## 高斯消元

来解方程的。

n^3 次方内，求解n元 , n个方程

![image-20210219170721424](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210219170721424.png)

![image-20210219170811990](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210219170811990.png)





# 组合计数

## 求组合数I -- 递推

![image-20210220161642967](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210220161642967.png)

![image-20210220162601321](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210220162601321.png)

```c++
for(int i = 0; i <= Max_N; i ++)
        for(int j = 0; j <= i; j ++)
            if(j == 0)
                C[i][j] = 1;
            else
                C[i][j] = (C[i - 1][j] + C[i - 1][j - 1]) % Mod;
```

## 求组合数II -- 预处理

中心思想：C(a,b)=a!÷b!÷(a−b)!(由于求逆元时用了Fermat小定理，所以要求Mod为质数)

```c++
int fact[N], infact[N]; // 阶乘和阶乘的逆

int quick_pow(int a, int b, int p){
    int res = 1;
    while(b){
        if(b & 1)
            res = (LL)res * a % p;
        b >>= 1;
        a = (LL)a * a % p;
    }
    return res;
}

void init(){
    fact[0] = infact[0] = 1;
    for(int i = 1; i <= 1e5; i ++){
        fact[i] = (LL)fact[i - 1] * i % Mod;
        infact[i] = (LL)infact[i - 1] * quick_pow(i, Mod - 2, Mod) % Mod;
    }
}

int C(int a, int b){
    return (LL)fact[a] * infact[b] % Mod * infact[a - b] % Mod;
}

```



## 求组合数III -- Lucas定理

![image-20210220163638077](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210220163638077.png)

```c++
LL quick_pow(int a, int k, int p) {
    LL res = 1;
    while(k) {
        if (k & 1) res = res * a % p;
        b >> 1;
        a = a * a % p;
    }
    return res;
}

LL C(LL a, LL b, LL p){
    LL res = 1;
    for(int i = 1, j = a; i <= b; i ++, j --){
        res = res * j % p;
        res = res * quick_pow(i, p - 2, p) % p;
    }
    return res;
}

LL lucas(LL a, LL b, LL p){
    if(a < p && b < p)
        return C(a, b, p);
    else
        return C(a % p, b % p, p) * lucas(a / p, b / p, p) % p;
}


```

## 容斥原理

![image-20210220165616052](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210220165616052.png)

```c++
for(int i = 1; i < 1 << m; i ++){
    int s = 0;
    long long t = 1;
    for(int j = 0; j < m; j ++)
        if(i >> j & 1){
            s ++;
            if((long long) t * a[j] > n){
                t = -1;
                break;
            }
            t *= a[j];
        }
    if(t != -1)
        if(s % 2)
            res += n / t;
        else
            res -= n / t;
}

```



# 高斯消元

蓝桥杯可能不涉及到。

# 简单博弈论--ICG公平组合游戏

一个游戏满足：

- 两个玩家交替进行
- 任意时刻，可以执行的合法行动与轮到哪位玩家无关
- 不能行动的玩家判负

围棋和象棋就不一样了。

## Nim游戏

![image-20210220170519051](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210220170519051.png)

![image-20210220171341971](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210220171341971.png)



## 台阶-Nim游戏

```c++
#include <iostream>

using namespace std;

int main()
{
    int res = 0;
    int n;
    cin >> n;

    for(int i = 1 ; i <= n ; i++)
    {
        int x;
        cin >> x;
        if(i % 2) res ^= x;
    }

    if(res) puts("Yes");
    else puts("No");
    return 0;
}
```



## 集合-Nim游戏

## 拆分Nim游戏