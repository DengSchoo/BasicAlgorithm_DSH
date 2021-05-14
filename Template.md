# 优先队列--排序构造

![image-20210213104656883](https://gitee.com/DengSchoo374/img/raw/master/images/image-20210213104656883.png)



`priority_queue<Type, Container<Type>, Functional<Type>>`

其中Type 为数据类型，Container为保存数据的容器，Functional 为元素比较方式。

升序排列：`priority_queue<int, vector<int>, greater<int> > q;`

```c++
#include<queue>
#include<iostream>
 
 
using namespace std;
 
 
struct mytype{
        int x;
};
 
 
struct comp{
        bool operator()(mytype a, mytype b)
        {
                return a.x > b.x;
        }
};
 
 
bool operator < (mytype a, mytype b){
        return a.x < b.x;
}
 
 
int main() {
        //基本数据类型
        priority_queue<int> pq; //使用默认模板参数（最大堆）
        priority_queue<int,vector<int>,greater<int> > pq1; //最小堆
 
 
        //自定义数据类型
        priority_queue<mytype,vector<mytype>,comp> pq2; //指定三个参数
        priority_queue<mytype> pq3;//重载operator<后，声明对象时就可以只带一个模板参数
}
```

