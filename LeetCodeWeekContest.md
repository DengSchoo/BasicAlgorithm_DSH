# LeetCode周赛题解

# 225 场周赛 图灵社区

![image-20210124161801764](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210124161801764.png)

```c++
class Solution {
public:

    bool check(string time, char str[]) {
        for (int i = 0; i < 5; i ++ ) {
            if (time[i] == str[i] || time[i] == '?') continue;
            return false;
        }
        return true;
    }

    string maximumTime(string time) {
        for (int i = 23; i >= 0; i -- ) {
            for (int j = 59; j >= 0; j -- ) {
                char str[20];
                sprintf(str, "%02d:%02d", i, j);
                if (check(time, str))
                    return str;
            }
        }
        return "";
    }
};
```



![image-20210124161820668](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210124161820668.png)

```c++
class Solution {
public:

    int work(vector<int> s1, vector<int> s2) {  // 让s1中所有字符小于s2中所有字符
        int res = INT_MAX;
        for (int i = 1; i < 26; i ++ ) {
            int cnt = 0;
            for (int j = i; j < 26; j ++ ) cnt += s1[j];
            for (int j = 0; j < i; j ++ ) cnt += s2[j];
            res = min(res, cnt);
        }
        return res;
    }

    int minCharacters(string a, string b) {
        vector<int> s1(26), s2(26);
        for (auto c: a) s1[c - 'a'] ++ ;
        for (auto c: b) s2[c - 'a'] ++ ;

        int n = a.size(), m = b.size();
        int res = INT_MAX;
        for (int i = 0; i < 26; i ++ )  // 条件3
            res = min(res, n + m - (s1[i] + s2[i]));

        return min(res, min(work(s1, s2), work(s2, s1)));
    }
};

```





![image-20210124161839791](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210124161839791.png)

```c++
const int N = 1000010;

int q[N];

class Solution {
public:
    int kthLargestValue(vector<vector<int>>& w, int k) {
        int n = w.size(), m = w[0].size();
        int cnt = 0;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ ) {
                // s[i][j] = w[i][j] + s[i-1][j] + s[i][j-1] - s[i - 1][j - 1]
                // w[i][j] = w[i][j] ^ s[i-1][j] ^ s[i][j-1] ^ s[i - 1][j - 1]
                if (i) w[i][j] ^= w[i - 1][j];
                if (j) w[i][j] ^= w[i][j - 1];
                if (i && j) w[i][j] ^= w[i - 1][j - 1];
                q[cnt ++ ] = w[i][j];
            }
        k = cnt - k;
        nth_element(q, q + k, q + cnt); // 求第n大的数 默认是从小到大排序的
        return q[k];
    }
};

```





# 230 场周赛

![image-20210228135654314](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210228135654314.png)



```c++
class Solution {
public:
    int countMatches(vector<vector<string>>& items, string ruleKey, string ruleValue) {
        int len = items.size();
        int ans = 0;
        map<string, int> m = {
            {"type", 1},{"color", 2},{"name", 3}
            
        };
        int idx = m[ruleKey] - 1;
        
        for (int i = 0; i < len ;i ++) {
            if (items[i][idx] == ruleValue) ans++;
        }
        return ans;
    }
};
```



![image-20210228135823034](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210228135823034.png)

```c++
class Solution {
public:
    void dfs(int sum,  int next, int &target, vector<int>& baseCosts, vector<int>& toppingCosts, int &ans, int &last) {
        if ((abs(sum - target) < abs(ans - target)) || (abs(sum - target) == abs(ans - target) && ans > sum)) {
            ans = sum;
        }
        
        for (int i = next; i < toppingCosts.size(); i++) {
            dfs(sum + toppingCosts[i],  i + 1 ,target,  baseCosts, toppingCosts, ans, last);
            dfs(sum + 2*toppingCosts[i], i + 1,target,   baseCosts, toppingCosts, ans, last);
        }
    }
    int closestCost(vector<int>& baseCosts, vector<int>& toppingCosts, int target) {
        int n =baseCosts.size(), m = toppingCosts.size();
        int last = 0;
        int ans = 100000;
        for (int i = 0; i < n; i++) {
            
            dfs(baseCosts[i], 0, target, baseCosts, toppingCosts, ans, last);
        }
        return ans;
    }
};
```

![image-20210228141823009](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210228141823009.png)

```c++
class Solution {
public:
    int work(vector<int>& h, int s) {
        int sum = 0;
        for (int i = 1; i <= 6; i ++ ) sum += i * h[i];

        if (sum > s) {
            int x = sum - s, res = 0;
            for (int i = 6; i > 1; i -- ) {
                int t = i - 1;
                if (h[i] * t >= x) return res + (x + t - 1) / t;
                res += h[i];
                x -= h[i] * t;
            }
            return res;
        } else {
            int x = s - sum, res = 0;
            for (int i = 1; i < 6; i ++ ) {
                int t = 6 - i;
                if (h[i] * t >= x) return res + (x + t - 1) / t;
                res += h[i];
                x -= h[i] * t;
            }
            return res;
        }
    }

    int minOperations(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size(), m = nums2.size();
        if (n > m) return minOperations(nums2, nums1);
        if (m > n * 6) return -1;
        vector<int> h1(7), h2(7);
        for (auto x: nums1) h1[x] ++ ;
        for (auto x: nums2) h2[x] ++ ;
        int res = 1e9;
        for (int i = m; i <= n * 6; i ++ )
            res = min(res, work(h1, i) + work(h2, i));
        return res;
    }
};

作者：yxc
链接：https://www.acwing.com/activity/content/code/content/922444/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

# 232场周赛

![image-20210315211022491](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210315211022491.png)

```c++
class Solution { // 暴力
public:
    bool areAlmostEqual(string s1, string s2) {
        int len = s1.length();
        int cnt = 0;
        int idx1 = 0, idx2 = 0;
        string temp;
        if (s1 == s2) return true;
        for (int i = 0; i < len; i++) {
            for (int j = i; j < len; j++) {
                temp = s1;
                char c = s1[i];
                temp[i] = temp[j];
                temp[j] = c;
                if (temp == s2) return true;
            }
        }
        
        return false;
    }
};
```



![image-20210315211034859](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210315211034859.png)

```c++
class Solution { // 最大度数的点
public:
    int findCenter(vector<vector<int>>& edges) {
        int n = edges.size();
        map<int, int> m;
        int max = 0, idx = -1;
        for (int i = 0; i < n; i++) {
            int a = edges[i][0], b = edges[i][1];
            if (m.count(a))
                m[a]++;
            else {
                m[a] = 1;
            }
            if (max < m[a]) max = m[a], idx = a;
            if (m.count(b))
                m[b]++;
            else {
                m[b] = 1;
            }
            if (max < m[b]) max = m[b], idx = b;
            
        }
        return idx;
        
    }
};
```



![image-20210315211043952](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210315211043952.png)

```c++
class Solution { // 自定义结构体的排序 + 优先队列
    typedef struct Class{
        double pass, total;
        
        bool operator < (const Class &x) const{
            
            double a = (pass + 1) / (total + 1) - pass/total;
            double b = (x.pass + 1) / (x.total + 1) - x.pass / x.total;
           return a < b;
      }
    }C;
public:
    double maxAverageRatio(vector<vector<int>>& classes, int extraStudents) {
        int n = classes.size();
        priority_queue<C> pq;;
        for (int i = 0; i < n; i++) {
            pq.push(C{(double)classes[i][0],(double)classes[i][1]});
        }
        while (extraStudents) {
            C c = pq.top();
            pq.pop();
            c.pass = c.pass + 1;
            c.total = c.total + 1;
            pq.push(c);
            extraStudents--;
        }
        double ans = 0;
        while (pq.size()) {
            C x = pq.top();
            pq.pop();
            //cout << x.pass << x.total << endl;
            ans += (x.pass) / (x.total);
        }
        return ans / n;
    }
};
```



![image-20210315211052478](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210315211052478.png)

```c++
// 双向优先队列
// 类似的题目：直方图中最大的矩形
class Solution {
public:
    int maximumScore(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> h(n + 2, - 1), l(n + 2), r(n + 2), stk(n + 2);
        for (int i = 1; i <= n; i++) h[i] = nums[i - 1];
        
        int tt = 0;
        stk[0] = 0;
        for (int i = 1; i <= n; i++) {
            while (h[stk[tt]] >= h[i] ) tt--; // 栈顶当前元素比h[i]大就先弹栈
            l[i] = stk[tt]; // s记录i的左向 第一个比h[i]小的元素下标
            stk[++tt] = i; // 将当前元素压栈
        }
        
        // 相当于逆向来了一遍求左向的过程
        tt = 0;
        stk[0] = n + 1;
        for (int i = n; i; i--) {
            while (h[stk[tt]] >= h[i] ) tt--; // 栈顶当前元素比h[i]大就先弹栈
            r[i] = stk[tt]; // s记录i的左向 第一个比h[i]小的元素下标
            stk[++tt] = i; // 将当前元素压栈
        }
        k++; // 因为下标都被扩大了1 所以+1
        int res = 0;
        for (int i = 1; i <= n; i++) {
            if (l[i] < k && r[i] > k)
                res = max(res, (r[i] - l[i] + 1 - 2) * h[i]);
        }
        return res;
    }
};
```

