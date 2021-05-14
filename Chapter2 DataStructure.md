# 第二章 数据结构

# 链表

> 数组模拟链表

```c++
struct node {
    int e, l, r;
}nodes[N];
```



## 单向链表

![image-20210206183314156](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210206183314156.png)

```c++
#include <iostream>
using namespace std;

const int N = 1000010;

// head 为头指针
// e[i],ne[i]为链表中的元素的值和下一个元素的索引
// idx存储已经用到了哪个点
int head, e[N], ne[N], idx;

void init() {
    head = -1;
    idx = 0;
}

// 将x插入到头结点
void add_head(int x) {
    // 存下要存的数
    e[idx] = x, ne[idx] = head;
    
    head = idx++;
    
}
// 插入结点
void add(int k, int x) {
    e[idx] = x;
    ne[idx] = ne[k];
    
    ne[k] = idx++;
    
}

//删除下标为k的结点后删掉
void remove(int k) {
    ne[k] = ne[ne[k]];
}


int main () {
    int m;
    cin >> m;
    init();
    while (m --) {
        int k, x;
        char op;
        cin >> op;
        if (op == 'H') {
            cin >> x;
            add_head(x);
        }else if (op == 'D') {
            cin >> k;
            if (!k) head = ne[head];
            remove(k - 1);
        } else   {
            cin >> k >> x;
            add(k - 1, x);
        }
    }
    for (int i = head; i != -1; i = ne[i]) cout << e[i] << endl;
    return 0;
}
```



## 双向链表

```c++
#include <iostream>
using namespace std;

const int N = 1000010;

// head 为头指针
// e[i],ne[i]为链表中的元素的值和下一个元素的索引
// idx存储已经用到了哪个点
int head, l[N], r[N], ne[N], idx;

void init() {
    r[0] = 1;
    r[1] = 0;
    idx = 2;
}

// 在右边插入一个x 如果要在左边插入则调用 add(l[k], x);
void add(int k, int x) {
    e[idx] = x;
    r[idx] = r[k];
    l[idx] = k;
    
    l[r[k]] = idx;
    r[k] = x;
}

// 删除第k个
void remove(int k) {
    r[l[k]] = r[k];
    l[r[k]] = l[k];
}

int main() {
    
}
```





# 栈

用idx模拟进栈和退栈

进栈：stack[idx++] = x

退栈：idx--

访问栈顶元素 stack[idx - 1]

# 队列



# 单调栈

![image-20210206194359795](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210206194359795.png)

## 求滑动窗口的最大值或者最小值

![image-20210206195501991](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210206195501991.png)

```c++
#include <iostream>
using namespace std;

const int N = 1e6 + 10;

int n;
int a[N], q[N];

int main () {
    cin >> n;
    for (int i =0; i < n;i++) cin >> a[i];
    
    int hh = 0, tt = 0;
   
    for(int i = 0; i < n; i++) {
        // 栈不为空 并且头
        if (hh <= tt && i - k + 1 > q[hh]) hh++;
        
        q[tt++] = i;// 将当前的数插入到队列中
        
        // a[q[tt]] <= a[i]即为最小值
        while(hh <= tt && a[q[tt]] >= a[i]) tt--; 
    	if (i >= k - 1) cout << a[q[hh]];
    }
}
```

# KMP

```c++
#include<iostream>

using namespace std;

const int N = 100010, M = 100010;
int n, m;
char p[N], s[M];
int ne[N];

int main() {
    cin >> n >> p + 1 >> m >> s + 1;
    // 求next过程
    for (int i = 2, j = 0; i <= n; i++) {
        while(j && p[i] != p[j + 1]) j = ne[j];
        if (p[i] == p[j + 1]) j++;
        ne[i] = j;
    }
    // kmp匹配过程
    for (int i = 1, j= 0; i <= m; i++) {
        while(j && s[i] != p[j + 1]) j = ne[j];
        if (s[i] == p[j + 1]) j++;
        if (j == n) {
            // 匹配成功
            
            
            j = ne[j]; // 为下次继续做准备
        }
    }
}
```

# Trie树

> 快速(高效的)存储和查找字符串集合的数据结构
>
> 要点：字母的个数不是特别多

![image-20210207182606695](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210207182606695.png)

![image-20210207182643846](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210207182643846.png)

```c++
#include <iostream>

using namespace std;

const int N = 100010;
int son[N][26], cnt[N], idx; // 小标为0的点，既是根节点又是空节点

void insert(char str[]) {
    int p = 0;
    for ( int i = 0; str[i]; i++) {
        int u = str[i] - 'a';
        if (!son[p][u]) son[p][u] = ++idx;
        p = son[p][u];
    }
    cnt[p] ++;
}


int query(char str[]) {
    int p = 0;
    for (int i = 0; str[i]; i++) {
        int u = str[i]- 'a';
        if (!son[p][u]) return 0;
        p = son[p][u];
    }
    return cnt[p];
}

char str[N];

int main () {
    int n;
    scanf("%d", &n);
    while(n --) {
        char op[2];
        scanf("%s%s", op, str);
        if (op[0] == '1') insert(str);
        else printf("%d", query(str));
    }
    return 0;
}
```



# 并查集

![image-20210209194507787](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210209194507787.png)

支持的操作

暴力：

- 将两个集合合并 `belong[x] = a`
- 询问两个元素是否在一个集合当中`if (belong[x] == belong[y]) `

并查集：

近乎O(1)的时间复杂度快速的支持上述两个操作。

基本原理：树的形式维护每一个集合。树根的编号就是整个集合的编号，每个节点存储它的父节点，P[x]表示x的父节点

- 表示是否是树根`if (p[x] == x)`

- 求x的集合编号：`while(p[x] != x) x = p[x];`，这一步仍然存在着复杂度，在这里可以做并查集的优化，路径压缩。

- 合并两个集合：`px 是x的编号， py是y的编号。p[x]= y, 将x插入到y`

```c++
#include <iostream>
using namespace std;

const int N = 100010;

int p[N], size[N];
int n, m;
int find(int x) { // 返回x的祖宗节点 + 路径压缩
    if (p[x] != x) p[x] = find(p[x]);
    
    // 经过路径压缩后已经是父节点
    return p[x];
}

// 累加两个集合的个数 将x插入y
void add(int x, int y) {
    // 
    if (x == y) return ;
    size[find(y)] += size[find(x)];
}

// 合并两个集合
void uion(int x, int y) {
    int px = find(x);
    int py = find(y);
    if (px == py) return;
    
    p[px] = py; // px插入到py中
    add(x, y);
}

int root_size(int x) {
    return size[find(x)];
}

// 判断是否在同一个连通块
bool connect(int x, int y) {
    if (find(x) == find(y)) return true;
    return false;
}



int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n;i++) {
        p[i] = i;
        size[i] = 1;
    }
    
}
```



# 堆

基本操作（小顶堆）：

1. 插入一个数`head[++size] = x; up(size)`
2. 求集合当中的最小值`heap[1]`
3. 删除最小值`heap[1] = heap[size]; size --,down(1)`
4. 删除任意一个元素`heap[k]=heap[size]; size--;down(k); up(k)`
5. 修改任意一个元素`heap[k] = x; up(k); down(k);`

堆是完全二叉树：

根节点要比左右孩子结点都要小。



存储：一维数组, 下标要从1开始 ，不然2 * x就是0。

x的左儿子 ==2x==

x的右儿子==2x+1==



```c++
#include <iostream>
#include <algorithm>

using namespace std;
const int N = 100010;
int n, m;
// 
int h[N],ph[N],hp[N] size;

// up 操作 O(log(n))
void up(int u) {
    while(u / 2 && h[u / 2] > h[u]) {
        swap(h[u / 2], h[u]);
        u /= 2;
    }
}

// down 操作 O(log(n))
void down(int u) {
    int t =  u;
    if (u * 2 <= size && h[u * 2] < h[t]) t = u * 2;
    if (u * 2 + 1 <= size && h[u * 2 + 1] < h[t]) t = u * 2 + 1;
    if (u != t) {
        swap(u , t);
        donw(t);
    }
}

int top() {
    return h[1];
}



void pop() {
    h[1] = h[size];
    size--;
    down(1);
}

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &h[i + 1]);
    }
    size = n;
    //有序数组 O(n)构建堆
    for (int i = n / 2; i; i--) {
        down(i);
    }
    
}

```

![image-20210209205913788](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210209205913788.png)

```c++
#include <iostream>
#include <algorithm>
#include <string.h>

using namespace std;

const int N = 100010;

int h[N], ph[N], hp[N], cnt;

void heap_swap(int a, int b)
{
    swap(ph[hp[a]],ph[hp[b]]);
    swap(hp[a], hp[b]);
    swap(h[a], h[b]);
}

void down(int u)
{
    int t = u;
    if (u * 2 <= cnt && h[u * 2] < h[t]) t = u * 2;
    if (u * 2 + 1 <= cnt && h[u * 2 + 1] < h[t]) t = u * 2 + 1;
    if (u != t)
    {
        heap_swap(u, t);
        down(t);
    }
}

void up(int u)
{
    while (u / 2 && h[u] < h[u / 2])
    {
        heap_swap(u, u / 2);
        u >>= 1;
    }
}

int main()
{
    int n, m = 0;
    scanf("%d", &n);
    while (n -- )
    {
        char op[5];
        int k, x;
        scanf("%s", op);
        if (!strcmp(op, "I"))
        {
            scanf("%d", &x);
            cnt ++ ;
            m ++ ;
            ph[m] = cnt, hp[cnt] = m;
            h[cnt] = x;
            up(cnt);
        }
        else if (!strcmp(op, "PM")) printf("%d\n", h[1]);
        else if (!strcmp(op, "DM"))
        {
            heap_swap(1, cnt);
            cnt -- ;
            down(1);
        }
        else if (!strcmp(op, "D"))
        {
            scanf("%d", &k);
            k = ph[k];
            heap_swap(k, cnt);
            cnt -- ;
            up(k);
            down(k);
        }
        else
        {
            scanf("%d%d", &k, &x);
            k = ph[k];
            h[k] = x;
            up(k);
            down(k);
        }
    }

    return 0;
}

```



# 哈希表

## 存储结构

### 开放寻址法

![image-20210210225547882](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210210225547882.png)

```c++
#include <iostream>

using namespace std;

// 经验 INT_MAX = 0x3f3f3f3f

const int N = 200003, null = 0x3f3f3f;

int h[N];

//返回下标或者要存储的位置
int find(int x) {
    int k = (x % N + N) % N;
    while(h[k] != null && h[k] != x) {
        k++;
        if (k == N) k = 0;
    }
    return k;
}
int main() {
    memset(h 0x3f, sizeof h);
    return 0;
}
```



### 拉链法

![image-20210210222936949](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210210222936949.png)

```c++
#include <iostream>

using namespace std;

const int N = 100003;

int h[N],e[N],ne[N], idx;
void insert(int x) {
    int k = (x % N + N) % N;
    e[idx] = x;
   	ne[idx] = h[k];
    h[k] = idx++;
}

bool find(int x) {
    int k = (x % N + N) % N;
    for (int i = h[k]; i!= -1; i = ne[i])
        if (e[i] == x)
            return true;
    return false;
}

int main() {
    memset(h, -1, sizeof h)
    return 0;
}
```



## 字符串哈希方法--字符串前缀哈希法

![image-20210210230214655](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210210230214655.png)

![image-20210210230549936](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210210230549936.png)

- 不能将某个字母映射为0。

- 假定不存在冲突

  p = 131 或者 13331

  Q = 2 ^ 64次方

  99.99%不会出现冲突。

可以由任意的前缀hash求得任意子串的hash

![image-20210210231537135](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210210231537135.png)

```c++
#include <iostream>

using namespace std;

// 131 进制
const int N = 100010, p = 131;

typedef unsigned long long ULL;

char str[N];
UUL h[N], p[N];

// 获取l - r之间的hash值
UUL get(int l, int r) {
    return h[r] - h[l] * p[r - l + 1]; 
}


int main () {
    p[0] = 1;
    for (int i = 1; i <= n; i++) {
        p[i] = p[i - 1] * p;
        h[i] = h[i - 1] * p + str[i];
    }
}

```

# STL

容器共有的：

- size()返回元素个数
- empty()返回是否为空

都可以使用C++ 11语法循环遍历

for (auto a:  A)  cout << a;



## vector 变长数组

> 倍增的思想：自动变长，尽量减少申请空间的次数（大概logn）
>
> 系统为某一程序分配空间的时候，所需的时间与空间大小无关，与申请次数有关。

### 初始化

```c++
vector<int> a(10, 1); // 长度为10的，初始值为1

```

- clear()清空
- front()/back() 访问队首和队尾
- push_back()/pop_back() 向队尾压入一个元素/队尾弹出
- 支持比较运算，可以判断两个vector的大小， 按照字典序

## pair<int , int  > 二元组

-  first是第一个元素

- second是第二个元素

- 支持比较运算，按照字典序来比较，以first为第一关键字，second为第二关键字

- C++ 11 构造一个pari

  `p = {20, "abc"}`

  `p = make_pair(10, "abc")`;

  pair<int , pair<string , string>>三元组

  

## string  字符串

- c_str()将string转换为字符数组
- substr(int start, int len)子串起始下标和长度
- length()
- `printf("%s", s.c_str())`;





## queue 队列

- push()入队
- front()返回队首
- back()返回队尾
- pop()出队
- size()
- empty()

没有clear函数

如果要清空：

```
queue<int> q;
q = queue<int>();
```



## priority_queue 堆

默认大顶堆

- push() 插入一个元素
- top() 返回堆顶元素
- pop()弹出堆顶元素

```c++
priority_queue<int> heap;
```

构造小顶堆

- 插入相反数
- 直接定义为小顶堆`priority_queue<int , vector<int>, greater<int>>heap`



## stack

- push()入栈

- top()返回栈顶元素

- pop()弹栈
- empty
- size



## deque 双端队列 

加强版的vector, 速度比较慢, 效率比较低

- size()
- empty()
- clear()
- front()
- back()
- push_back();
- pop_back();
- push_front
- pop_front()
- []
- begin/end

## set, multiset ,map, multimap

基于（平衡二叉树）红黑树， 动态维护有序序列, 时间复杂度logn

set不可以有重复元素，multiset可以有重复元素

- insert() 插入一个数
- find() 查找一个数
- count()返回某一个数的个数
- erase(int x)删除所有x O(k + log n)
- erase(迭代器) 删除这个迭代器
- lower_bound/upper_bound()
  - lower(x) 返回大于等于x的最小的数
  - upper_bound返回大于x最小的数



### map / multimap

- insert()  插入的数是一个pair<>二元组 , k ,v

- erase() pair或者迭代器
- find()
- [] 时间复杂度是logn
- lower_bound/upper_bound()

```c++
map<string, int> a;
a["dsh"] = 1;
cout << a["dsh"] << endl;
```







## unorder_set ,unorder_map, unorder_multiset, unorder_multimao

内部无序，基于hash表实现的。绝大部分操作的时间复杂度为O(1)

不支持lower_bound/upper_bound()

```c++
unordered_map<string, int> um;
```



## bitset 压位

- `bitset<100000> s;`
- ~ , &, | , ^
- `>>`
- `<<`
- ==
- !=
- []
- count() 返回多少个1
- any() 是否至少有一个1
- none判断是否全为0
- set() 把所有位置1
- set(k, v) 把k位变为v
- reset()所有位置0
- flip()所有位取反
- flio(int k)，把k取反





