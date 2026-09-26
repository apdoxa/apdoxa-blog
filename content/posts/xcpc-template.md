+++
date = '2026-08-09T09:50:22+08:00'
draft = false
title = 'XCPC自用模板库'
+++

> 声明: 自用！非本人原创，仅做整理归档

# 一、杂

## 1.二分（单调函数）

### 整数域

- 前驱：第一个满足 check(m) 的位置（最小值）

```cpp
int l=1,int r=n+1;
while(r>l){
    int mid=((r-l>>1)+l);
    if(chk(mid))r=mid;
    else l=mid+1;
}
```

- 后继: 最后一个满足 check(m) 的位置（最大值）

```cpp
int l=0,r=n;
while(r>l){
    int mid=((r-l+1>>1)+l);//向上取整
    if(chk(mid))l=mid;
    else r=mid-1;
  
}
```

### 实数域

```cpp
using ld=long double;
auto chk=[&](double t)->bool{
  
};
double lo=0,hi=1e12;
while(hi-lo>max(1.0,lo)*EPS){//误差控制
    double mid=(lo+hi)/2;
    if(chk(mid))hi=x;
    else lo=x;
}
```

## 2.三分(单峰函数)

```cpp
using ld=long double;
const ld EPS=1e-7;
auto get=[&](const auto&f){
    ld lo=-1e4,hi=1e4;
    while(hi-lo>3*EPS){
        ld x1=(lo+hi-EPS)/2;
        ld x2=(lo+hi+EPS)/2;
        if(f(x1)>f(x2))lo=x1;//极小
        else hi=x2;
    }
    return f((lo+hi)/2);
}
```

# 二、数据结构

## 树状数组

```cpp
//1索引，注意索引0会卡循环
struct BIT{
    int n;
    vector<int> bit;
    BIT(int n):bit(n+1){this->n=n;}
    void add(int pos,int v){
        for(int i=pos;i<=n;i+=i&-i){
            bit[i]+=v;
        };
    };
    int sum(int pos){
        int ans=0;
        for(int i=pos;i>0;i-=i&-i){
            ans+=bit[i];
        }
        return ans;
    }
    int rngsum(int l,int r){
        if(l>r)return 0;
        return sum(r)-sum(l-1);
    }
};
```

# 三、图论

# 四、字符串

## KMP及前缀数组

```cpp
auto kmp_prefix=[&](string s){
    int n=s.size();
    vector<int> f(n+1);
    for(int i=1,j=0;i<n;i++){
        while(j and s[i]!=s[j])j=f[j];
        j+=(s[i]==s[j]);
        f[i+1]=j;
    }
};
auto kmp=[&](string s,string t){
    int n=s.size(),m=t.size();
    vector<int> pos;
    auto pre=kmp_prefix(t);
    for(int i=1,j=0;i<=n;i++){
        while(j and s[i]!=t[j+1])j=kmp[j];
        if(s[i]==t[j+1])j++;
        if(j==m)pos.push_back(i-m+1);
    }
    return pos; 
}
```

# 五、数学

## 1. 卢卡斯定理 nlogn 求解组合数

```cpp
int C[MO][MO];
int Lk(int n,int m,int p){
    if(m>n)return 0;
    if(m==0)return 1;
    return (C[n%p][m%p]*Lk(n/p,m/p,p)%p);
}
```

## 2.杨辉三角求解组合数

```cpp
int C[N][N];
void init(){
    C[0][0]=1;
    rrp(i,1,n){
        C[i][0]=1;
        rrp(j,1,i){
            C[i][j]=(C[i-1][j-1]+C[i-1][j])%MO;
        }
    }
}
```

参考📚
1.[jiangly算法模板收集]https://www.cnblogs.com/WIDA/p/17633758.html#%E5%A3%B0%E6%98%8E
2.[github hh2048/XCPC](https://github.com/hh2048/XCPC)
3.[OIWiki](https://oi-wiki.org/)
4.[CF提交](https://codeforces.com/)
5.[牛客提交](https://ac.nowcoder.com/)
