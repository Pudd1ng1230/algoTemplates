# 环境配置

### 火车头
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    
}

signed main() {
    freopen("in.txt", "r", stdin);
    ios::sync_with_stdio(false), cin.tie(nullptr);
    int qaq = 1; 
    // cin >> qaq;
    while (qaq--) {
        solve();
    }
    return 0;
}
```

### CodeRunner配置
`Ctrl + Shift + P`后输入`Preferences: Open User Settings (JSON)`，添加如下内容：
```cpp
{
    // 让代码在集成终端里运行，方便进行输入交互
    "code-runner.runInTerminal": true,
    // 运行前自动保存文件，避免忘记保存
    "code-runner.saveFileBeforeRun": true,
    // 运行前清空上一次的输出，结果更清晰
    "code-runner.clearPreviousOutput": true,
    // 【核心配置】自定义 C++ 的编译和运行命令
    "code-runner.executorMap": {
        "cpp": "cd $dir && g++ -std=gnu++23 -O2 -Wall $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt"
    }
}
```
### 命令行编译
```cmd
g++ a.cpp -o a -std=c++17 -O2 -Wall -static && ./a
g++ a.cpp -o a -std=gnu++23 -O2 -Wall -static && ./a
```


# STL

### string
```cpp
s.substr(pos) // 从pos截取到末尾
s.substr(pos, len)
s.insert(pos, str_to_insert)
s.erase(pos, len)
s.find(str_to_find, start_pos) // 返回索引，没找到返回string::opos
for (auto it = s.begin(); it != s.end(); ++it) cout << *it; // 迭代器遍历
```

### vector
```cpp
v.front()
v.back()
v.emplace_back()
v.pop_back()
v.resize(new_size)
v.erase(iterator) // O(N)
	v.erase(v.begin()); // 删除第一个元素
	v.erase(v.begin() + 5); // 删除第六个元素
	v.erase(v.begin(), v.begin() + 3); // 删除前三个元素
v.insert(iterator, item) // O(N)
find(v.begin(), v.end(), value)
	//返回一个指向第一个等于 value 的元素的迭代器。如果没找到，返回 v.end()。
accumulate(v.begin(), v.end(), 0LL); // 求和, 0LL 是初始值
```

### sqrt
```cpp
long double 版本的 sqrt 为 sqrtl，可提高精度
```
### map
```cpp
map: 永远按 Key 排序（红黑树）
unordered_map：永远是乱序（哈希表）
给 Value 排序 一定要 p.second push 进 vector 然后 sort vector
```
### next_permutation
```
把给定的序列（数组、vector、string）变成字典序中的下一个排列，先排序，再 do-while
```

### 二分
lower 表示 >=， upper 表示 >。使用前记得先进行排序。
```cpp
//返回a数组区间中第一个>=x的地址【地址！！！】
cout << lower_bound(a.begin(), a.end(), x);
cout << lower_bound(a.begin(), a.end(), x) - a.begin(); //在a数组中查找第一个>=x的元素下标
upper_bound(a.begin(), a.end(), x) - lower_bound(a.begin(), a.end(), x) //查找k在a中出现了几次
```

### accumulate
```cpp
//将a数组区间的元素进行累加，并输出累加和+x的值
cout << accumulate(a.begin(), a.end(), x);
```

### priority_queue
默认升序（大根堆）
```cpp
priority_queue<int, vector<int>, greater<int>> pq; //重定义为降序（小根堆）
```

### pair排序
```cpp
sort(a.begin(), a.end(), [](const auto& x, const auto& y) {
    return x.second < y.second;
});
sort(a.begin(), a.end(), [](const auto& x, const auto& y) {
    if (x.first != y.first) {
        return x.first < y.first; 
    }
    return x.second > y.second;   
});
```

# 基础

### 保留小数
```cpp
cout << std::fixed << setprecision(n) << x;
```
### 对角线判断
```cpp
|行1 - 行2| == |列1 - 列2|
```

### 双指针MEX 0-index
```cpp
auto mex = [&]() mutable {
    int l = 0, r = n;
    while (l != r) {
        if (a[l] == l + 1) l++;
        else if (a[l] <= l) r--, swap(a[r], a[l]);
        else if (a[l] > r) r--, swap(a[r], a[l]);
        else if (a[a[l] - 1] == a[l]) r--, swap(a[r], a[l]);
        else swap(a[l], a[a[l] - 1]);
    }
    return l + 1;
};
```

### 二分
```cpp
// 最后一个满足条件的值(最大化最小值)
int ans = 0;
while(l <= r) {
    int mid = (l + r) >> 1;
    if(check(mid)) {
        ans = mid;
        l = mid + 1; // 从左往右移动
    } else {
        r = mid - 1; 
    }
}
cout << ans << endl; 
```
```cpp
// 第一个满足条件的值(最小化最大值)
int ans = 0;
while(l <= r) {
    int mid = (l + r) >> 1;
    if(check(mid)) {
        ans = mid;
        r = mid - 1; // 从右往左移动   
    } else {
        l = mid + 1;
    }
}
cout << ans << endl;
```


### 前缀和 & 差分
```cpp
void solve() {
	int n; cin >> n;
	vector<int> a(n + 1), pre(n + 1);
	for(int i = 1; i <= n; ++i) {
		cin >> a[i];
		pre[i] = pre[i - 1] + a[i];
	}
	int m; cin >> m;
	for(int i = 1; i <= m; ++i) {
		int l, r; cin >> l >> r;
		cout << pre[r] - pre[l - 1] << endl;
	}
}
```
```cpp
void solve() {
    int n; cin >> n;
    vector<int> diff(n + 2, 0);
    int m; cin >> m;
    for(int i = 1; i <= m; ++i) {
        int l, r, c; 
        cin >> l >> r >> c;
        diff[l] += c;       
        diff[r + 1] -= c;  
    }
    for(int i = 1; i <= n; ++i) {
        diff[i] += diff[i - 1];
        cout << diff[i] << " ";
    }
    cout << endl;
}
```
### 二维前缀和
```cpp
void solve() {
    int n, m, q;
    cin >> n >> m >> q;
    vector<vector<int>> a(n + 1, vector<int>(m + 1));
    vector<vector<int>> s(n + 1, vector<int>(m + 1));
    for(int i = 1; i <= n; ++i) {
        for(int j = 1; j <= m; ++j) {
            cin >> a[i][j];
            s[i][j] = s[i-1][j] + s[i][j-1] - s[i-1][j-1] + a[i][j];
        }
    }
    while(q--) {
        int x1, y1, x2, y2;
        cin >> x1 >> y1 >> x2 >> y2;
        cout << s[x2][y2] - s[x1-1][y2] - s[x2][y1-1] + s[x1-1][y1-1] << endl;
    }
}
```

### 归并排序
`将数组递归地分成两半，直到子数组长度为 1(天然有序); 递归排序两个子数组; 将两个已排序的子数组合并成一个有序数组。`
```cpp
function<void(int, int)> merge = [&](int l, int r) {
    if (l >= r) return;
    int mid = (l + r) >> 1;
    merge(l, mid);
    merge(mid + 1, r);
    
    int i = l, j = mid + 1, p = 0;
    static vector<int> t(r - l + 1);
    
    while(i <= mid && j <= r) {
        if (a[i] <= a[j]) {
            t[p++] = a[i++];
        } else {
            t[p++] = a[j++];
        }
    }
    while (i <= mid) t[p++] = a[i++];
    while (j <= r)   t[p++] = a[j++];
    
    for (int k = 0; k < p; ++k) {
        a[l + k] = t[k];
    }
```


### 桶排序
```cpp
vector<int> cnt(100010, 0);
int n = read();
for(int i = 0; i < n; ++i) {
    int x = read();
    cnt[x]++;
}
queue<int> q1, q2;
for(int i = 1; i <= 100009; ++i) {
    while(cnt[i]--) {
        q1.push(i);
    }
}
```
### 阶乘
```cpp
vector<int> f(200010);
main {
    f[0] = 1;
    for(int i = 1; i < 200010; ++i) {
        f[i] = f[i - 1] * i % mod;
    }
}
```

### 快读
```cpp
inline int read() {
    int x = 0, f = 1; char ch = getchar();
    while (ch < '0' || ch > '9') { if (ch == '-') f = -1; ch = getchar(); }
    while (ch >= '0' && ch <= '9') { x = x * 10 + ch - '0'; ch = getchar(); }
    return x * f;
}
```

### 位运算
1. 求出一个数字二进制的第$i$位是 $0$ 还是 $1$ :
`(x >> i) & 1`、
检查 $a[i]$ 的第 $k$ 位: `a[i] & (1 << k)`
2. 判断一个数字是否是 $2$ 的幂次方： 
`x & (x - 1) == 0`
3. 求出一个数字二进制最低的 $1$ 所在的值：
`x & (-x)`
4. $A⊕A=0$、$A⊕0=A$、若 $A⊕B=1$，则 $∣A−B∣=1$
5. 变$1$：`ans |= val`、变$0$：`ans &= ~val`
6. $a + b = a \oplus b + 2 \cdot (a \wedge b)$
7. $a = a \oplus b + (a \wedge b) = (a \oplus b) \vee (a \wedge b)$
8. 性质：
- `X | Y = (X ^ Y) + (X & Y)`、
- `X + Y = (X | Y) + (X & Y)`、
- `A + B = (A ^ B) + ((A & B) << 1)`
- `A ^ B = X ⟺ A ^ X = B`
- `Al ^ Al + 1 ^ ... ^Ar = Pre[r] ^ Pre[l - 1]`
- 
$$
\bigoplus_{i=1}^n i =
\begin{cases}
1 & \text{n mod 4 = 1} \\
n + 1 & \text{n mod 4 = 2} \\
0 & \text{n mod 4 = 3} \\
n & \text{n mod 4 = 0}
\end{cases}
$$
-
$$
\bigoplus_{i=1}^n \bigoplus_{j=1}^i j =
\begin{cases}
n + 1 & \text{n mod 8 = 0} \\
1 & \text{n mod 8 = 1} \\
n + 1 & \text{n mod 8 = 2} \\
0 & \text{n mod 8 = 3} \\
0 & \text{n mod 8 = 4} \\
n + 1 & \text{n mod 8 = 5} \\
1 & \text{n mod 8 = 6} \\
n + 1 & \text{n mod 8 = 7}
\end{cases}
$$

### 二进制枚举
数据范围 $N<=20$ 且 元素只有两种状态时使用.

相关STL：`__builtin_popcount (计算一个整数的二进制表示中有多少个 1)`

例： 

高桥需要从 $N$ 种花苗中选出一个子集进行种植，使得**总收益最大化**。


- **数量限制**：选出的花苗种类数量必须在 $0$ 到 $K$ 之间（含）。
- **二选一**：每种花苗要么种一株，要么不种。


总收益由两部分组成：
- **美丽值**：所有被选中花苗的美丽值 $S_i$ 之和。
- **比赛奖金**：对于 $M$ 场比赛，只要选中的花苗中**至少有一株**落在该比赛的区间 $[L_j, R_j]$ 内，就能获得该场比赛的奖金 $P_j$。

求最大的收益的和.

```cpp
void solve() {
    int n, k, m;
    cin >> n >> k >> m;
    vector<int> s(n);
    for(int i = 0; i < n; ++i) {
        cin >> s[i];
    }
    int tot = 1 << n, ans = 0;
    vector<int> l(m), r(m), p(m);
    for(int i = 0; i < m; ++i) {
        cin >> l[i] >> r[i] >> p[i];
        l[i]--, r[i]--;
    }
    for(int mask = 0; mask < tot; ++mask) {
        if(__builtin_popcount(mask) > k) {
            continue;
        }
        int sum1 = 0;
        for(int i = 0; i < n; ++i) {
            if(mask & (1 << i)) {
                sum1 += s[i];
            }
        }
        int sum2 = 0;
        for(int i = 0; i < m; ++i) {
            bool ok = 0;
            for(int j = l[i]; j <= r[i]; ++j) {
                if(mask & (1 << j)) {
                    ok = 1;
                    break;
                }
            }
            if(ok) {
                sum2 += p[i];
            }
        }
        ans = max(ans, sum1 + sum2);
    }
    cout << ans << endl;
}
```

### 归并排序求逆序对数
```cpp
vector<int> a(n), t(n);
int res = 0;
function<void(int, int)> guibing = [&](int l, int r) {
    if (l >= r) return;
    int mid = (l + r) >> 1;
    guibing(l, mid);
    guibing(mid + 1, r);
    int i = l, j = mid + 1;
    int p = 0;
    while (i != mid + 1 && j != r + 1) {
        if (a[i] <= a[j]) {
            t[p++] = a[i];
            i++;
        } else {
            res += mid - i + 1;
            t[p++] = a[j];
            j++;
        }
    }
    while (i != mid + 1) {
        t[p++] = a[i++];
    }
    while (j != r + 1) {
        t[p++] = a[j++];
    }
    for (int k = 0; k < p; k++) {
        a[k + l] = t[k];
    }
};
guibing(0, n - 1);
cout << res << endl;
```
### 最长上升（下降）子序列
```cpp
void solve() {
    int n;
    cin >> n;
    vector<int> a(n);
    for (int i = 0; i < n; ++i) {
        cin >> a[i];
    }
    vector<int> g;
    for (int x : a) {
        // 最长上升子序列
        auto it = lower_bound(g.begin(), g.end(), x);
        // 最长不下降子序列
        auto it = upper_bound(g.begin(), g.end(), x);
        // 最长下降子序列
        auto it = lower_bound(g.begin(), g.end(), x, greater<int>());
        // 最长不上升子序列
        auto it = upper_bound(g.begin(), g.end(), x, greater<int>());
        if (it == g.end()) {
            g.push_back(x);
        } else {
            *it = x;
        }
    }

    cout << g.size() << endl;
}
```

# 数学


### GCD & LCM
```cpp
int gcd(int a, int b) {
	return b > 0 ? gcd(b, a % b) : a;
}

int lcm(int a, int b) {
	return a / gcd(a, b) * b;
}
```

### 高精度

#### 高精度乘低精度

```cpp
string mul(string a, int b) {
    if (b == 0) return "0";
    vector<int> na(a.size());
    for (int i = 0; i < a.size(); ++i)
        na[i] = a[a.size() - 1 - i] - '0';

    int carry = 0;
    for (int i = 0; i < na.size(); ++i) {
        carry += na[i] * b;
        na[i] = carry % 10;
        carry /= 10;
    }
    while (carry) {
        na.push_back(carry % 10);
        carry /= 10;
    }

    string res;
    for (int i = (int)na.size() - 1; i >= 0; --i)
        res += char('0' + na[i]);
    return res;
}
```
#### 高精度除低精度 
```cpp
string div(string a, int b) {
    if (a == "0") return "0";
    string r;
    int rem = 0;
    for (char c : a) {
        rem = rem * 10 + (c - '0');
        r += char('0' + rem / b);
        rem %= b;
    }
    int i = 0;
    while (i < r.size() && r[i] == '0') i++;
    if (i == r.size()) return "0";
    return r.substr(i);
}
```


### 进制转换
```cpp
long long num = stoll(str, 起始位置, 进制); // 这里的进制是str的进制
```
```cpp
// 当前值 = 旧值 * 进制 + 当前位的数字
long long num = 0;
for (int i = 0; i < len; ++i) {
    num = num * base + (s[idx + i] - '0');
}
```
```cpp
cout << "8进制: " << oct << num << endl;
cout << "10进制: " << dec << num << endl;
cout << "16进制: " << hex << num << endl;
cout << "2进制: " << bitset<8>(num) << endl;
```
```cpp
// 字符 -> 数字
auto cal = [&](char c) {
    if(c <= '9' && c >= '0') return c - '0';
    return c - 'A' + 10;
};
// n 进制字符串 -> 十进制数
auto c1 = [&](string s, int base) {
    int res = 0;
    for(auto& c : s) {
        res = res * base + cal(c);
    }
    return res;
};
// 十进制数 -> m 进制字符串
auto c2 = [&](int n, int base) {
    string d = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    string res = "";
    while(n) {
        res = d[n % base] + res;
        n /= base;
    }
    return res;
};
```

### 快速幂
```cpp
typedef long long ll;

ll fastpow(ll a, ll b) {
    ll res = 1;
    while (b) {
        if (b & 1) res = (res * a) % mod;
        b >>= 1;
        a = (a * a) % mod;
    }
    return res % mod;
}
```

### 矩阵快速幂
```cpp
typedef long long ll;

struct Matrix {
    int n;
    vector<vector<ll>> a;

    // 构造函数：n x n 零矩阵
    Matrix(int n) : n(n), a(n, vector<ll>(n, 0)) {}

    // 单位矩阵
    static Matrix identity(int n) { // 相当于数字1
        Matrix I(n);
        for (int i = 0; i < n; ++i)
            I.a[i][i] = 1;
        return I;
    }

    // 矩阵乘法：C = A * B
    Matrix operator*(const Matrix& other) const {
        Matrix res(n);
        for (int i = 0; i < n; ++i) {
            for (int k = 0; k < n; ++k) {
                if (a[i][k] == 0) continue; // 小优化
                for (int j = 0; j < n; ++j) {
                    res.a[i][j] = (res.a[i][j] + a[i][k] * other.a[k][j]) % mod;
                }
            }
        }
        return res;
    }
};
Matrix fastpow(Matrix base, ll pow) {
    Matrix res = Matrix::identity(base.n);
    while (pow > 0) {
        if (pow & 1) {
            res = res * base;
        }
        base = base * base; 
        pow >>= 1;
    }
    return res;
}
```

### 线性筛
```cpp
constexpr int N = 1000000;
bool check(int x) {
    static vector<int> primes;           // 存质数列表（静态，只初始化一次）
    static vector<bool> is_prime;        // is_prime[i] 表示 i 是否为质数
    static bool ok = false;     // 标记是否已完成筛法

    // 只在第一次调用时执行筛法
    if (!ok) {
        ok = true;
        is_prime.assign(N + 1, true);
        is_prime[0] = is_prime[1] = false;

        for (int i = 2; i <= N; i++) {
            if (is_prime[i]) {
                primes.push_back(i);
            }
            for (int j = 0; j < primes.size(); j++) {
                int p = primes[j];
                int cur = i * p;
                if (cur > N) break;
                is_prime[cur] = false;
                if (i % p == 0) {
                    break;
                }
            }
        }
    }

    if (x < 0 || x > N) return false;
    return x >= 2 && is_prime[x];
}
```
```cpp
constexpr int N = 100000000;
vector<int> primes;
vector<bool> is_prime;
void init() {
    is_prime.assign(N + 1, true);
    is_prime[0] = is_prime[1] = false;
    for (int i = 2; i <= N; ++i) {
        if (is_prime[i]) {
            primes.push_back(i);
        }
        for (size_t j = 0; j < primes.size(); ++j) {
            long long cur = 1LL * i * primes[j];
            if (cur > N) break;
            is_prime[cur] = false;
            if (i % primes[j] == 0) {
                break;
            }
        }
    }
}
```
### 埃氏筛
```cpp
bool ok[N];
void init(int n) {
    for (int i = 0; i <= n; ++i) ok[i] = true;
    ok[0] = ok[1] = false;
    for (int p = 2; p * p <= n; ++p) {
        if (ok[p]) {
            for (int i = p * p; i <= n; i += p) {
                ok[i] = false;
            }
        }
    }
}

vector<int> get() {
    vector<int> p;
    for(int i = 2; i < N; ++i) {
        if(ok[i]) p.push_back(i);
    }
    return p;
}
```

### 试除法
```cpp
bool check(int n) {
    if (n <= 1) return 0;
    for (int i = 2; i * i <= n; ++i) {
        if (n % i == 0) {
            return 0;
        }
    }
    return 1;
}
```

### 卡常
```cpp
auto check = [&](int n) -> bool {
    if(n < 2) return 0;
    if(n == 2 || n == 3) return 1;
    if(!(n & 1) || n % 3 == 0) return 0;
    for(int i = 5; i * i <= n; i += 6) {
        if(n % i == 0 || n % (i + 2) == 0) return 0;
    }
    return 1;
};
```

### exgcd
功能：求解形如
$$ax + by = \gcd(a, b)$$
的整数解 $(x, y)$，并返回 $\gcd(a, b)$。
```cpp
int exgcd(int a, int b, int& x, int& y) {
    if (b == 0) {
        x = 1, y = 0; return a;
    }
    int x1, y1; int temp = exgcd(b, a % b, x1, y1);
    x = y1;
    y = x1 - a / b * y1;
    return temp;
}

```

### 欧拉函数
$\varphi(x)$ 是指 $1 \sim x$ 的数中，与 $x$ 互质的数的个数。
```cpp
ll phi(ll n) {
    ll res = n;
    for (ll i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            res = res / i * (i - 1);
            while (n % i == 0) n /= i;
        }
    }
    if (n > 1) res = res / n * (n - 1);
    return res;
}
```
### 对顶堆求第$k$大的数字
```cpp
void solve() {
    int n, k;
    cin >> n >> k;
    vector<int> arr(n); 
    for(int i = 0; i < n; i++) {
        cin >> arr[i];
    }
    priority_queue<int> a; 
    priority_queue<int, vector<int>, greater<int>> b;
    auto duidingdui = [&](int x) {
        if (b.empty() || x >= b.top())
            b.push(x);
        else
            a.push(x);

        while (b.size() > k) {
            a.push(b.top());
            b.pop();
        }
        while (b.size() < k) {
            if (a.empty()) return;
            b.push(a.top());
            a.pop();
        }
    };

    auto get = [&]() -> int {
        return b.top();
    };

    for (int i = 0; i < n; i++) {
        duidingdui(arr[i]);
    }

    cout << get() << endl;
}
```


# 字符串

### KMP

- 核心思想：利用已经匹配过的信息，不用把指针回退到老位置，而是让指针尽可能地往多了跳，跳过那些明知道不可能匹配得到的位置

- 我们可以使用`Next`数组，对字串进行一个预处理，算出字串的每个前缀中，最长的 **相等的前缀和后缀** 的长度。
- 举个例子：$S = ABABCA$， 
- **idx = 0**, `A`:  
  前缀、后缀不存在（长度为 0）。  
  `next[0] = 0`

- **idx = 1**, `AB`:  
  前缀：`A`，后缀：`B`。不相等。  
  `next[1] = 0`

- **idx = 2**, `ABA`:  
  前缀：`A`，后缀：`A`。相等！长度为 1。  
  `next[2] = 1`

- **idx = 3**, `ABAB`:  
  前缀：`AB`，后缀：`AB`。相等！长度为 2。  
  `next[3] = 2`

- **idx = 4**, `ABABC`:  
  所有可能的真前缀（`A`, `AB`, `ABA`）与真后缀（`BABC`, `ABC`, `BC`, `C`）无相等项。  
  `next[4] = 0`

- 当 `P` 在第 `j` 位失配时，说明 `P[0...j-1]` 这部分是匹配的

```cpp
// 1. 计算 next 数组（即前缀函数）
vector<int> next(const string& p) {
    int m = p.size();
    vector<int> nxt(m, 0);
    for (int i = 1; i < m; ++i) {
        int j = nxt[i - 1];
        while (j > 0 && p[i] != p[j])
            j = nxt[j - 1];
        if (p[i] == p[j])
            j++;
        nxt[i] = j;
    }
    return nxt;
}

// 2. KMP 匹配：返回所有出现位置（1-based）
vector<int> kmp_search(const string& s, const string& p, const vector<int>& nxt) {
    vector<int> pos;
    int n = s.size(), m = p.size();
    int j = 0;
    for (int i = 0; i < n; ++i) {
        while (j > 0 && s[i] != p[j])
            j = nxt[j - 1];
        if (s[i] == p[j])
            j++;
        if (j == m) {
            pos.push_back(i - m + 2); // 0-based改成+1
            j = nxt[j - 1];
        }
    }
    return pos;
}

void solve() {
    string s, p;
    cin >> s >> p;
    vector<int> nxt = next(p), ans = kmp_search(s, p, nxt);
    if (ans.empty()) {
        cout << -1 << endl;
    } else {
        cout << ans.size() << endl;
        for (int pos : ans) {
            cout << pos << " ";
        }
        cout << endl;
    }
}
```
### 字符串哈希
```cpp
// 单哈希
typedef unsigned long long ULL;

ULL str_hash(const string& s) {
    ULL h = 0;
    for (char c : s)
        h = h * 131 + c;
    return h;
}
int main() {
    int n;
    cin >> n;
    unordered_set<ULL> hashes;
    string s;
    while (n--) {
        cin >> s;
        hashes.insert(str_hash(s));
    }
    cout << hashes.size() << endl;
}
```
```cpp
// 双哈希
typedef unsigned long long ULL;

const ULL BASE1 = 131, BASE2 = 13331;

struct DoubleHash {
    ULL h1, h2;
    bool operator==(const DoubleHash& other) const {
        return h1 == other.h1 && h2 == other.h2;
    }
};

struct Hasher {
    size_t operator()(const DoubleHash& dh) const {
        return dh.h1 ^ dh.h2; // 简单哈希组合
    }
};

DoubleHash get_hash(const string& s) {
    ULL h1 = 0, h2 = 0;
    for (char c : s) {
        h1 = h1 * BASE1 + c;
        h2 = h2 * BASE2 + c;
    }
    return {h1, h2};
}

int main() {
    int n; cin >> n;
    unordered_set<DoubleHash, Hasher> seen;
    while (n--) {
        string s; cin >> s;
        seen.insert(get_hash(s));
    }
    cout << seen.size() << endl;
}

```

# 数据结构

### 单调队列
单调队列用于在滑动窗口中高效维护最大值或最小值
```cpp
void solve() {
    int n, k; cin >> n >> k;
    vector<int> h(n);
    for(int i = 0; i < n; ++i) {
        cin >> h[i];
    }
    deque<int> q1, q2;
    int ans = 0;
    for(int i = 0; i < n; ++i) {
        while(!q1.empty() && h[i] >= h[q1.back()]) q1.pop_back();
        while(!q2.empty() && h[i] <= h[q2.back()]) q2.pop_back();
        q1.push_back(i);
        q2.push_back(i);
        if(q1.front() <= i - k) q1.pop_front();
        if(q2.front() <= i - k) q2.pop_front();
        ans = max(ans, h[q1.front()] - h[q2.front()]);
    }
    cout << ans << endl; // 窗口内最大差值
}
```



### 二叉堆
```cpp
priority_queue<int, vector<int>, greater<int>> heap; // 最小堆
```


# DP

### 如何设计 DP 模型
关于定义：有多少个需要知道的信息就开多少维数组，数组的值一般是题目询问的答案。$DP$ 问题很大一部分难点就在于如何设计出符合题目需求的 $DP$ 定义。
常见的几种定义：（仅仅是作为举例，具体题目务必具体分析）
1. 前 $i$ 个元素可以得到的最大值/最小值/方案数。
2. 前 $i$ 个元素中，第 $i$ 个元素不选/选的方案可以得到的最大值/最小值/方案数。
3. 前 $i$ 个元素使用了 $i$ 次操作$1$使用了 $j$ 次操作$2$可以得到的最大值/最小值/方案数。
4. 前 $i$ 个元素选 $j$个可以得到的最大值/最小值/方案数。
5. 前 $i$ 个元素是否可以组合出 $j$。
6. 前 $i$ 个元素剩余空间为 $j$ 可以得到的最大值/最小值/方案数。（背包dp）
7. 区间 $[l, r]$ 可以得到的最大值/最小值/方案数。（区间dp）
8. 选择的集合为 $s$ 可以得到的最大值/最小值/方案数。（状压dp）
    
值得注意的是，在设计定义时，应该特别留意题目的数据范围和是否满足最优子结构和无后效性。在设计定义时切记依据题目分析出的性质来设计模型，而非生搬硬套。

关于初始化：和题目询问反着来就可以了。比如问某某操作得到的最大值，那么就设置成无穷小；问最小值，就设置成无穷大；问方案数，就全部设置成 0 。接下来，自己手算出最初的状态/最开始的一两步，定义的 DP 数组的值即可。

关于转移：首先要将问题的状态全部抽象出来，将各个状态之间的联系用箭头连起来，然后去考虑从状态  A 转移到状态 B 时会对dp值有什么影响。为了一般性，我们还应该从中间的一步骤进行考虑，而非从第一步或最后一步。
状态的转移也应该具体问题具体分析，例如：
1. 若转移是求和运算：$DPb <- DPb + DPa$
2. 若转移是求最大值：$DPb <- max(DPb, DPa)$
3. 若转移是求最小值：$DPb <- min(DPb,)$
4. 若转移是求状态 A 到状态 B 加上 B 点的贡献的最大值：$DPb <- max(DPb, DPa + Ab)$ 其中 Ab 是 B 点的贡献。

### 线性DP
有 $N$ 个台阶，第 $i$ 个台阶的高度为 $h_i$，在第 $i$ 个台阶时，跳到第 $j$ 个台阶，则需要支付的代价为 $|h_i - h_j|$。求到达第 $N$ 个台阶的最小代价。

```cpp
void solve() {
    int n, k;
    cin >> n >> k;
    vector<int> a(n + 1);
    for(int i = 1; i <= n; ++i) {
        cin >> a[i];
    }
    vector<int> dp(n + 1, 1e18);
    dp[1] = 0;
    for(int i = 2; i <= n; ++i) {
        for(int j = max(1LL, i - k); j < i; ++j) {
            dp[i] = min(dp[i], dp[j] + abs(a[i] - a[j]));
        }
    }
    cout << dp[n] << endl;
}
```

在$n×n$的平面中, 每一行有一条线段，线段长度为$[L, R]$，从$(1， 1)$出发，要求走过所有线段最终到达$(n，n)$，求路径最短长度。
```cpp
void solve() {
    int n;
    cin >> n;
    vector<int> l(n + 1), r(n + 1);
    for(int i = 1; i <= n; ++i) {
        cin >> l[i] >> r[i];
    }
    vector<vector<int>> f(n + 1, vector<int>(2, 0));
    f[1][0] = 2 * r[1] - l[1] - 1;
    f[1][1] = r[1] - 1;
    for (int i = 2; i <= n; ++i) {
        int len = r[i] - l[i] + 1;
        f[i][0] = min(
            f[i-1][0] + abs(l[i-1] - r[i]) + len,
            f[i-1][1] + abs(r[i-1] - r[i]) + len
        );
        
        f[i][1] = min(
            f[i-1][0] + abs(l[i-1] - l[i]) + len,
            f[i-1][1] + abs(r[i-1] - l[i]) + len
        );
    }
    cout << min(f[n][0] + n - l[n], f[n][1] + n - r[n]) << endl;
}
```

有$n$颗果树，上面有数个果实，经过树时摘掉所有果实，但此后$k$颗树不能摘果，求最大摘果数
```cpp
void solve() {
    int n, k;
    cin >> n >> k;
    vector<int> a(n + 1);
    for(int i = 1; i <= n; ++i) {
        cin >> a[i];
    }
    vector<int> dp(n + 1, 0);
    for(int i = 1; i <= n; ++i) {
        int t = a[i], idx = i - k - 1;
        if(idx >= 0) {
            t += dp[idx];
        }
        dp[i] = max(dp[i - 1], t);
    }
    cout << dp[n] << endl;
}
```

### 01背包

有 $N$ 个物品。每个物品编号为 $1, 2, \ldots, N$。对于每个 $i$（$1 \leq i \leq N$），物品 $i$ 的重量为 $w_i$，价值为 $v_i$。
太郎君打算从这 $N$ 个物品中选择一些，放入背包带回家。背包的容量为 $W$，所选物品的总重量不能超过 $W$. 求出能带回家的物品的最大总价值。

（1≤W≤10^5 1≤v≤10^9）
```cpp
void solve() {
    int n, mx;
    cin >> n >> mx;
    vector<int> w(n), v(n);
    for(int i = 0; i < n; ++i) {
        cin >> w[i] >> v[i];
    }
    vector<int> dp(mx + 1, 0);
    for(int i = 0; i < n; ++i) {
        for(int j = mx; j >= w[i]; --j) { // 倒序遍历
            dp[j] = max(dp[j], dp[j - w[i]] + v[i]);
        }
    }
    cout << dp[mx] << endl;
}
```
（1≤W≤10^9  1≤v≤10^3）
```cpp
void solve() {
    int n, mx;
    cin >> n >> mx;
    vector<int> w(n), v(n);
    int sum = 0;
    for(int i = 0; i < n; ++i) {
        cin >> w[i] >> v[i];
        sum += v[i];
    }
    vector<int> dp(sum + 1, 2e18);
    dp[0] = 0;
    for(int i = 0; i < n; ++i) {
        for(int j = sum; j >= v[i]; --j) {
            if(dp[j - v[i]] != 2e18) {
                dp[j] = min(dp[j], dp[j - v[i]] + w[i]);
            }
        }
    }
    int ans = 0;
    for(int j = sum; j >= 0; --j) {
        if(dp[j] <= mx) {
            ans = j;
            break;
        }
    }
    cout << ans << endl;
}
```

恰好装满型的01背包：有$n$件商品，商品的满意度是$v$，价格是$c$，最多买一次。手上有$s$元要全部花光，求总满意度最大值。
```cpp
constexpr int inf = 1e18;
void solve() {
    int n, s;
    cin >> n >> s;
    vector<int> v(n + 1), c(n + 1);
    for(int i = 1; i <= n; ++i) {
        cin >> v[i] >> c[i];
    }
    vector<int> dp(s + 1, -inf);
    dp[0] = 0;
    for(int i = 1; i <= n; ++i) {
        for(int j = s; j >= c[i]; --j) {
            if(dp[j - c[i]] != -inf) {
                dp[j] = max(dp[j], dp[j - c[i]] + v[i]);
            }
        }
    }
    if(dp[s] == -inf) {
        cout << -1 << endl;     
    } else {
        cout << dp[s] << endl;
    }
}
```

有$n$头奶牛，奶牛有$s$与$f$两个属性，单一属性之和不能小于$0$，选出一些奶牛（可以不选）。求他们两个属性之和的最大值。
```cpp
void solve() {
    int n;
    cin >> n;
    vector<pair<int, int>> cow(n + 1);
    for (int i = 1; i <= n; ++i) {
        cin >> cow[i].first >> cow[i].second;
    }

    const int OFFSET = 400000;
    const int MX = 800005;
    vector<int> dp(MX);
    fill(dp.begin(), dp.end(), INT_MIN);
    dp[OFFSET] = 0;
    
    for (int i = 1; i <= n; ++i) {
        int s = cow[i].first; 
        int f = cow[i].second; 

        if (s >= 0) {
            for (int j = MX - 1; j >= s; --j) {
                if (dp[j - s] != INT_MIN) {
                    dp[j] = max(dp[j], dp[j - s] + f);
                }
            }
        } else {
            for (int j = 0; j < MX + s; ++j) {
                if (dp[j - s] != INT_MIN) {
                    dp[j] = max(dp[j], dp[j - s] + f);
                }
            }
        }
    }

    int ans = 0;
    for (int i = OFFSET; i < MX; ++i) {
        if (dp[i] >= 0) {
            ans = max(ans, dp[i] + i - OFFSET);
        }
    }

    cout << ans << endl;

}
```


有$n$座山，有入场费$c$与海拔高度$h$两个属性，可以选择性地爬，但爬山数量最多为$k$，且爬山的海拔必须严格递增，爬山的费用不能超过$b$，求最多能爬的山的数量。
```cpp
constexpr int inf = 1e18;
void solve() {
    int n, k, b;
    cin >> n >> k >> b;
    vector<int> c(n + 1), s(n + 1);
    for(int i = 1; i <= n; ++i) {
        cin >> c[i] >> s[i];
    }
    vector<vector<int>> dp(k + 1, vector<int>(b + 1, inf));
    dp[0][0] = 0;
    for(int i = 1; i <= n; ++i) {
        int cost = c[i], h = s[i];
        for(int j = k; j >= 1; --j) {
            for(int co = b; co >= cost; --co) {
                if(dp[j - 1][co - cost] < h) {
                    dp[j][co] = min(dp[j][co], h);
                }
            }
        }
    }
    int ans = 0;
    for(int j = 1; j <= k; ++j) {
        for(int co = 0; co <= b; ++co) {
            if(dp[j][co] < inf) {
                ans = j;
            }
        }
    }
    cout << ans << endl;
}
```


### 完全背包
草药采每一株都需要一些时间，每一株也有它自身的价值。给你一段时间，在这段时间里，你可以采到一些草药。使采到的草药的总价值最大。
```cpp
void solve() {
	int t, m;
	cin >> t >> m;
	vector<int> time(m), val(m);
	for(int i = 0; i < m; ++i) {
		cin >> time[i] >> val[i];
	}
	vector<vector<int>> dp(m + 1, vector<int>(t + 1, 0));
	for(int i = 1; i <= m; ++i) {
		for(int j = 0; j <= t; ++j) {
			dp[i][j] = dp[i - 1][j];
			if(j >= time[i - 1]) { 
				dp[i][j] = max(dp[i][j], 
                dp[i - 1][j - time[i - 1]] + val[i - 1]);
			}
		}
	}
	cout << dp[m][t] << endl
}
```

### 混合背包
有$n$颗树，魅力值为$c$，看一棵树有三种方式：看$1$次、看最多$p$次，$p=0$意味着无数次。看树需要时间$t$，给出开始和结束时间，求出时间内怎么看使魅力值最高。
```cpp
void solve() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    int h1, h2, m1, m2, n, sum;
    char cc;
    cin >> h1 >> cc >> h2 >> m1 >> cc >> m2;
    sum = 60 * (m1 - h1) + m2 - h2;
    cin >> n;
    vector<int> c(n + 1), a(n + 1), t(n + 1);
    vector<int> dp(1001, 0); 
    for (int p = 1; p <= n; p++) {
        cin >> t[p] >> c[p] >> a[p];
    }
    for (int i = 1; i <= n; i++) {
        if (a[i] == 0) {
            // 完全背包逻辑
            for (int j = t[i]; j <= sum; j++) {
                dp[j] = max(dp[j], dp[j - t[i]] + c[i]);
            }
        } else {
            for (int l = 1; l <= a[i]; l++) {
                for (int j = sum; j >= l * t[i]; j--) {
                    dp[j] = max(dp[j], dp[j - t[i]] + c[i]);
                }
            }
        }
    }
    cout << dp[sum];
}
```


### 区间DP
区间 DP 一个很显著的特点：一般允许 $O(n ^ 3)$ 级别的时间复杂度。区间 DP 的核心是将问题抽象成一个个子区间的问题，然后考虑可以组成一个大区间的任意两个子区间进行转移的最小代价。

设有 $N$ 堆石子环形排列，每堆石子有一定的质量 $m_i\ (m_i \le 1000)$。现在要将这 $N$ 堆石子合并成为一堆。每次只能合并相邻的两堆，合并的代价为这两堆石子的质量之和，求合并成一堆的最大代价和最小代价。

```cpp
void solve() {
    int n;
    cin >> n;
    
    // 1. 断环成链：数组开到 2*n + 1，把石子复制一份接在后面
    vector<int> a(2 * n + 1);
    vector<int> sum(2 * n + 1, 0); // 前缀和数组也要扩大
    
    for (int i = 1; i <= n; ++i) {
        cin >> a[i];
        a[i + n] = a[i]; // 复制一份，实现断环成链
    }
    
    // 预处理前缀和（注意上限是 2*n）
    for (int i = 1; i <= 2 * n; ++i) {
        sum[i] = sum[i-1] + a[i];
    }
    
    // 2. 增加 dp_max 数组，分别记录最小和最大代价
    vector<vector<int>> dp_min(2 * n + 1, vector<int>(2 * n + 1, INT_MAX));
    vector<vector<int>> dp_max(2 * n + 1, vector<int>(2 * n + 1, 0)); // 求最大值初始化为0即可
    
    // 初始化：长度为1的区间代价为0
    for (int i = 1; i <= 2 * n; ++i) {
        dp_min[i][i] = 0;
        dp_max[i][i] = 0;
    }
    
    // 区间DP核心逻辑
    // 1. 枚举长度（最长只需要合并 n 堆）
    for (int len = 2; len <= n; ++len) {
        // 2. 枚举起点（上限是 2*n）
        for (int i = 1; i + len - 1 <= 2 * n; ++i) {
            int j = i + len - 1; // 计算终点
            
            // 3. 枚举断点 k
            for (int k = i; k < j; ++k) {
                int cost = sum[j] - sum[i-1]; // 当前合并的代价（区间和）
                // 状态转移：同时更新最小值和最大值
                dp_min[i][j] = min(dp_min[i][j], dp_min[i][k] + dp_min[k+1][j] + cost);
                dp_max[i][j] = max(dp_max[i][j], dp_max[i][k] + dp_max[k+1][j] + cost);
            }
        }
    }
    
    // 3. 统计最终答案：遍历所有长度为 n 的区间，找出全局的最小和最大值
    int ans_min = INT_MAX;
    int ans_max = 0;
    for (int i = 1; i <= n; ++i) {
        ans_min = min(ans_min, dp_min[i][i + n - 1]);
        ans_max = max(ans_max, dp_max[i][i + n - 1]);
    }
    
    cout << ans_min << endl;
    cout << ans_max << endl;

    // 若非环则无需遍历统计
    // cout << dp_min[1][n] << endl;
    // cout << dp_max[1][n] << endl;
}
```

一个序列含$n$个正整数，可以选择两个相邻且相等的数替换为一个比原数大$1$的数，最大化序列的最大值。
```cpp
void solve() {
    int n;
    cin >> n;
    int ans = 0; 
    vector<int> a(n + 1);
    for (int i = 1; i <= n; ++i) {
        cin >> a[i];
        ans = max(ans, a[i]);
    }
    vector<vector<int>> dp(n + 1, vector<int>(n + 1, 0));
    for (int i = 1; i <= n; ++i) {
        dp[i][i] = a[i];
    }
    for (int len = 2; len <= n; ++len) {
        for (int i = 1; i + len - 1 <= n; ++i) {
            int j = i + len - 1;
            for (int k = i; k < j; ++k) {
                if (dp[i][k] > 0 && dp[i][k] == dp[k+1][j]) {
                    dp[i][j] = max(dp[i][j], dp[i][k] + 1);
                }
            }
            ans = max(ans, dp[i][j]);
        }
    }
    cout << ans << endl;
}
```

有一排纸牌，两个人轮流拿取最左边或者最右边的纸牌，得分为纸牌数字总和，两人都希望最大化自己的得分，求两个人的得分最后是多少。
```cpp
void solve() {
    int n;
    cin >> n;
    vector<int> a(n + 1);
    int sum = 0;
    for(int i = 1; i <= n; ++i) {
        cin >> a[i];
        sum += a[i];
    }
    // dp[l][r] 表示在区间 [l, r] 内，当前先手玩家能比后手玩家多拿的分数
    vector<vector<int>> dp(n + 1, vector<int>(n + 1, 0));
    for(int len = 1; len <= n; ++len) {
        for(int l = 1; l + len - 1 <= n; ++l) {
            int r = l + len - 1;
            if(l == r) {
                dp[l][r] = a[l];
            } else {
                dp[l][r] = max(a[l] - dp[l + 1][r], a[r] - dp[l][r - 1]);
            }
        }
    }
    // 高桥是先手，dp[1][n] 就是他比青木多拿的分数
    int diff = dp[1][n];
    cout << (sum + diff) / 2 << ' ' << (sum - diff) / 2;
}
```

有一串珠子，可视为一个长度为 $n$ 的环形序列，每个珠子有头和尾。你可以选择两颗相邻的珠子，合并后会得到 $a×b×c$ 的分数（其中 $c$ 是 第二颗数字的尾）。合并后珠子消失，合并成一个新珠子，头尾改变。你的目标是制定一个合并顺序，直到序列只剩下一个数，使得累加的总分数最大。
```cpp
void solve() {
    int n;
    cin >> n;
    // 断环成链：把数组复制一倍，变成长度为 2*n 的链
    // e[i] 既表示第 i 颗珠子的头标记，也隐含了第 i-1 颗珠子的尾标记
    vector<int> e(2 * n + 1);
    for (int i = 1; i <= n; ++i) {
        cin >> e[i];
        e[i + n] = e[i]; // 复制一份接在后面
    }
    // dp[j][i] 表示合并区间 [j, i] 内的所有珠子能释放的最大能量
    vector<vector<int>> dp(2 * n + 1, vector<int>(2 * n + 1, 0));
    int ans = 0; // 记录全局最大能量
    // i 代表区间的右端点
    for (int i = 2; i < 2 * n; ++i) {
        // j 代表区间的左端点，从 i-1 向前推
        // 限制条件：i - j < n 保证区间长度不超过 n（最多合并 n 颗珠子）
        for (int j = i - 1; i - j < n && j >= 1; --j) {
            // k 是区间 [j, i] 的分割点，把区间分成 [j, k] 和 [k+1, i]
            for (int k = j; k < i; ++k) {
                // 状态转移方程：
                // 当前能量 = 左区间最大能量 + 右区间最大能量 + 合并左右区间产生的新能量
                // 合并产生的新能量 = 左区间第一颗珠子的头(e[j]) * 右区间第一颗珠子的头(e[k+1]) * 总区间最后一颗珠子的尾(e[i+1])
                dp[j][i] = max(dp[j][i], dp[j][k] + dp[k + 1][i] + e[j] * e[k + 1] * e[i + 1]);
            }
            // 随时更新全局最大值
            ans = max(ans, dp[j][i]);
        }
    }
    
    cout << ans << endl;
}
```

### LCS(最长公共子序列)
给定一个字符串 $s$ 和一个字符串 $t$ ，输出 $s$ 和 $t$ 的最长公共子序列。

```cpp
void solve() {
    string s, t;
    cin >> s >> t;
    int n = s.length(), m = t.length();
    vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (s[i] == t[j]) {
                dp[i + 1][j + 1] = dp[i][j] + 1;
            } else {
                dp[i + 1][j + 1] = max(dp[i][j + 1], dp[i + 1][j]);
            }
        }
    }
    string lcs = "";
    int i = n - 1, j = m - 1;
    while (i >= 0 && j >= 0) {
        if (s[i] == t[j]) {
            lcs = s[i] + lcs;
            i--;
            j--;
        } else if (dp[i][j + 1] > dp[i + 1][j]) {
            i--;
        } else {
            j--;
        }
    }
    cout << lcs << endl;
}
```

### DAG上找到最长路径
给定一个有 $N$ 个顶点、$M$ 条边的有向图 $G$。顶点编号为 $1, 2, \ldots, N$。对于每个 $i$（$1 \leq i \leq M$），第 $i$ 条有向边从顶点 $x_i$ 指向顶点 $y_i$。$G$ 不包含有向环。

请你求出 $G$ 中所有有向路径中最长的那条路径的长度。这里，有向路径的长度指的是该路径上包含的边的数量。

```cpp
void solve() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> g(n + 1);
    vector<int> in(n + 1, 0);
    for(int i = 0; i < m; ++i) {
        int x, y;
        cin >> x >> y;
        g[x].push_back(y);
        in[y]++;
    }
    vector<int> dp(n + 1, 0);
    queue<int> q;
    for(int i = 1; i <= n; ++i) {
        if(in[i] == 0) {
            q.push(i);
        }
    }
    while(!q.empty()) {
        int u = q.front();
        q.pop();
        for(int v : g[u]) {
            dp[v] = max(dp[v], dp[u] + 1);
            in[v]--;
            if(in[v] == 0) {
                q.push(v);
            }
        }
    }
    cout << *max_element(dp.begin() + 1, dp.end());
}
```

### 二维
有一个高 $H$ 行、宽 $W$ 列的网格。第 $i$ 行第 $j$ 列的格子用 $(i, j)$ 表示。

对于每个 $i, j$（$1 \leq i \leq H$，$1 \leq j \leq W$），格子 $(i, j)$ 的信息由字符 $a_{i, j}$ 给出。如果 $a_{i, j}$ 为 `.`，则格子 $(i, j)$ 是空格；如果 $a_{i, j}$ 为 `#`，则格子 $(i, j)$ 是墙。保证格子 $(1, 1)$ 和 $(H, W)$ 都是空格。

太郎君从格子 $(1, 1)$ 出发，每次只能向右或向下移动到相邻的空格，目标是到达格子 $(H, W)$。

请问从 $(1, 1)$ 到 $(H, W)$ 的路径有多少种？由于答案可能非常大，请输出答案对 $10^9 + 7$ 取模的结果。
```cpp
void solve() {
    int n, m;
    cin >> n >> m;
    vector<vector<char>> a(n + 1, vector<char>(m + 1));
    for(int i = 1; i <= n; ++i) {
        for(int j = 1; j <= m; ++j) {
            cin >> a[i][j];
        }
    }
    vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
    dp[1][1] = 1;
    for(int i = 1; i <= n; ++i) {
        for(int j = 1; j <= m; ++j) {
            if(a[i][j] == '.') {
                if(i == 1 && j == 1) continue;
                dp[i][j] = (dp[i - 1][j] + dp[i][j - 1]) % mod;
            }
        }
    }
    cout << dp[n][m] << endl;
}
```

### 有限状态
在长度为$n$且各点值不同的上序列上可以选择覆盖$3$段长为$d$、值为$0$的子段，求剩下未覆盖的字段的最小和。
```cpp
void solve() {
    int n, d, t0, t1, t2;
    cin >> n >> d >> t0 >> t1 >> t2;
    string s;
    cin >> s;
    vector<int> a(n, 0);
    for(int i = 0; i < n; ++i) {
        if(s[i] == '0') {
            a[i] = t0;
        } else if(s[i] == '1') {
            a[i] = t1;
        } else {
            a[i] = t2;
        }
    }
    vector<vector<int>> dp(n + 1, vector<int>(4, 2e18)); // 在长度为i，用了j段后的值
    dp[0][0] = 0;
    for(int i = 0; i < n; ++i) {
        for(int j = 0; j <= 3; ++j) {
            if(dp[i][j] == 2e18) {
                continue;
            } // 无效状态
            dp[i + 1][j] = min(dp[i + 1][j], dp[i][j] + a[i]);
            if(j < 3) {
                int t = min(n, i + d);
                dp[t][j + 1] = min(dp[t][j + 1], dp[i][j]);
            }
        }
    }
    int ans = 2e18;
    for(int j = 0; j <= 3; ++j) {
        ans = min(ans, dp[n][j]);
    }
    cout << ans << endl;
}

```

# 图

### 基础读入
```cpp
// 建立邻接表(有向图)
void solve() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> adj(n + 1);
    for(int i = 0; i < m; ++i) {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
    }
}
```
```cpp
// 建立带边权邻接表(无向图)
void solve() {
    int n, m;
    cin >> n >> m;
    vector<vector<pair<int, int>>> adj(n + 1);
    for(int i = 0; i < m; ++i) {
        int u, v, w;
        cin >> u >> v >> w;
        adj[u].push_back({v, w});
        adj[v].push_back({u, w});
    }
}
```



### 图上DFS
```cpp
void solve() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> G(n + 1);
    for (int i = 0; i < m; ++i) {
        int x, y;
        cin >> x >> y;
        G[x].push_back(y);
    }
    vector<bool> vis(n + 1, false);
    function<void(int)> dfs = [&](int u) {
        vis[u] = true;
        for (int v : G[u]) {
            if (!vis[v]) {
                dfs(v);
            }
        }
    };
    dfs(1);
    for (int i = 1; i <= n; ++i) {
        if (vis[i]) {
            cout << i << " ";
        }
    }
    cout << "\n";
}
```

### Dilworth定理
```cpp
对于任意有限偏序集，其最大反链中元素的数目必等于最小链划分中链的数目 
即：一个序列剖成若干个单调不升子序列的最小个数等于该序列最长上升子序列的个数
```

### LCA
典型应用场景：
求树上两点间距离：
$$\text{dist}(u, v) = \text{depth}[u] + \text{depth}[v] - 2 \times \text{depth}[\text{LCA}(u, v)]$$
```cpp
const int MAXN = 500005;
const int LOG = 20;
vector<int> e[MAXN];
int fa[MAXN][LOG + 1];
int deep[MAXN];

void dfs(int u, int f) {
    deep[u] = deep[f] + 1;
    fa[u][0] = f;
    for (int i = 1; i <= LOG; i++) {
        fa[u][i] = fa[fa[u][i - 1]][i - 1];
    }
    for (int v : e[u]) {
        if (v != f) {
            dfs(v, u);
        }
    }
}

int lca(int x, int y) {
    if (deep[x] < deep[y]) swap(x, y);

    for (int i = LOG; i >= 0; i--) {
        if (deep[fa[x][i]] >= deep[y]) {
            x = fa[x][i];
        }
    }
    
    if (x == y) return x;
    
    for (int i = LOG; i >= 0; i--) {
        if (fa[x][i] != fa[y][i]) {
            x = fa[x][i];
            y = fa[y][i];
        }
    }
    return fa[x][0];
}
```


### 并查集
```cpp
constexpr int N = 200010;
int fa[N], sz[N], deg[N];
int find(int x) {
    return fa[x] == x ? x : fa[x] = find(fa[x]);
} 
void unite(int x, int y) {
    fa[find(x)] = find(y); 
}
void init(int n){
    for(int i = 1; i <= n; ++i) {
        fa[i] = i;
		sz[i] = 1;
		deg[i] = 0;
    }
}
```


### 单源最短路径
```cpp
constexpr ll INF = 1e18;
vector<int> dijkstra(int start, const vector<vector<pair<int, int>>> &adj) {
    int n = adj.size();
    vector<int> dist(n, INF);
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;

    dist[start] = 0;
    pq.push({0, start});

    while (!pq.empty()) {
        auto [d, u] = pq.top();
        pq.pop();
        if (d != dist[u]) continue;

        for (auto [v, w] : adj[u]) {
            if (dist[v] > dist[u] + w) {
                dist[v] = dist[u] + w;
                pq.push({dist[v], v});
            }
        }
    }
    return dist;
}
void solve() {
    int n, m, s;
    cin >> n >> m >> s; // 读入点数、边数、起点 (假设点编号从 0 到 n-1)
    
    // 1. 建图：初始化邻接表
    vector<vector<pair<int, int>>> adj(n);

    // 2. 读入边
    for (int i = 0; i < m; ++i) {
        int u, v, w;
        cin >> u >> v >> w;
        adj[u].push_back({v, w}); // 有向图
        // adj[v].push_back({u, w}); // 无向图
    }

    vector<int> dist = dijkstra(s, adj);
    for (int i = 0; i < n; ++i) {
        if (dist[i] == INF) cout << "-1 "; // 如果距离还是 INF，说明走不到
        else cout << dist[i] << " ";
    }
    cout << endl;
}
```
### 最小生成树
#### Kruskal_稀疏图_输入点边权
```cpp
constexpr int N = 200010;
int fa[N];
int find(int x) {
    return fa[x] == x ? x : fa[x] = find(fa[x]);
} 
void unite(int x, int y) {
    fa[find(x)] = find(y);
}
void init(int n){
    for (int i = 1; i <= n; ++i) {
        fa[i] = i;
    }
}
struct Edge {
    int u, v, w;
    bool operator<(const Edge& other) const {
        return w < other.w;
    }
};
long long kruskal(int n, vector<Edge>& edges) {
    init(n);
    sort(edges.begin(), edges.end());
    long long total = 0;
    int cnt = 0;

    for (auto& e : edges) {
        if (find(e.u) != find(e.v)) {
            unite(e.u, e.v);
            total += e.w;
            cnt++;
            if (cnt == n - 1) break;
        }
    }

    return (cnt == n - 1) ? total : -1;
}
```

```cpp
// 最小生成森林
long long kruskal(int n, int k, vector<Edge>& edges) {
    init(n);
    sort(edges.begin(), edges.end());
    long long total = 0;
    int cnt = 0;
    int need = n - k;
    if(need == 0) return 0;

    for (auto& e : edges) {
        if (find(e.u) != find(e.v)) {
            unite(e.u, e.v);
            total += e.w;
            cnt++;
            if (cnt == need) break;
        }
    }

    return (cnt == need) ? total : -1;
}
```
#### Prim_稠密图_输入矩阵
```cpp
using pii = pair<int, int>;

long long prim(int n, const vector<vector<pii>>& adj) {
    vector<long long> dist(n + 1, LLONG_MAX);
    vector<bool> vis(n + 1, false);
    priority_queue<pii, vector<pii>, greater<pii>> pq;

    dist[1] = 0;
    pq.push({0, 1});

    long long total = 0;
    int cnt = 0;

    while (!pq.empty()) {
        auto [d, u] = pq.top();
        pq.pop();
        if (vis[u]) continue;
        vis[u] = true;
        total += d;
        cnt++;

        for (auto [v, w] : adj[u]) {
            if (!vis[v] && (long long)w < dist[v]) {
                dist[v] = w;
                pq.push({w, v});
            }
        }
    }

    return (cnt == n) ? total : -1;
}
```

# 板子题

### 贪心

终点为$g$，初始油量为$f$，有$n$个加油站，读入位置和一次性加的油。求最少要在几个加油站加油。（反悔贪心）
```cpp
void solve() {
    int n, g, f;
    cin >> n >> g >> f;
    vector<pair<int, int>> a(n);
    for(int i = 0; i < n; ++i) {
        cin >> a[i].first >> a[i].second;
    }
    sort(a.begin(), a.end());
    priority_queue<int> pq;
    int ans = 0, cur = f, idx = 0;
    while(cur < g) {
        while(idx < n && a[idx].first <= cur) {
            pq.push(a[idx].second);
            idx++;
        }
        if(pq.empty()) {
            cout << -1 << endl;
            return;
        }
        cur += pq.top();
        pq.pop();
        ans++;
    }
    cout << ans << endl;
}
```

区间调度/匹配：有$n$个车位，$m$辆车，第$i$辆车可以用$l$到$r$的车位，判断能否分配。
```cpp
void solve() {
    int n, m;
    cin >> n >> m;
    if(m > n) {
        cout << "No" << endl;
        return;
    }
    vector<pair<int, int>> a(m);
    for(int i = 0; i < m; ++i) {
        cin >> a[i].first >> a[i].second;
    }
    sort(a.begin(), a.end());
    priority_queue<int, vector<int>, greater<int>> pq;
    int idx = 0;
    for(int pos = 1; pos <= n; ++pos) {
        while(idx < m && a[idx].first <= pos) {
            pq.push(a[idx].second);;
            idx++;
        }
        while(!pq.empty() && pq.top() < pos) {
            cout << "No" << endl;
            return;
        }
        if(!pq.empty()) {
            pq.pop();
        }
    }
    if(!pq.empty() || idx < m) {
        cout << "No" << endl;
    } else {
        cout << "Yes" << endl;
    }
}
```
最大不相交区间数：有$n$个会议，时间从$l$到$r$，时间不能重叠，求最大可开会议数。
```cpp
void solve() {
    int n;
    cin >> n;
    vector<pair<int, int>> a(n);
    for(int i = 0; i < n; ++i) {
        cin >> a[i].first >> a[i].second;
    }
    sort(a.begin(), a.end(), [](const auto& a, const auto& b) {
        return a.second < b.second;
    });
    int cnt = 0, lst = 0;
    for(auto&x : a) {
        int fir = x.first, sec = x.second;
        if(fir >= lst) {
            cnt++;
            lst = sec;
        }
    }
    cout << cnt << endl;
}
```

区间合并：每天都判断是否可以种花，重复$n$天，有$m$段雨，从$l$持续到$r$，雨期可能重叠，求种完种子是什么时候。
```cpp
void solve() {
    int n, m;
    cin >> n >> m;
    vector<pair<int, int>> a(m), b;
    for(int i = 0; i < m; ++i) {
        cin >> a[i].first >> a[i].second;
    }
    sort(a.begin(), a.end());
    for(auto&x : a) {
        if(b.empty() || x.first > b.back().second) {
            b.push_back(x);
        } else {
            b.back().second = max(b.back().second, x.second);
        }
    }
    int l = 1, r = 2e18, ans = r;
    auto cal = [&](int d) -> int {
        int sum = 0;
        for(auto& x : b) {
            if(x.first > d) {
                break;
            }
            sum += min(x.second, d) - x.first + 1;
        }
        return sum;
    };
    while(l <= r) {
        int mid = l + r >> 1;
        if(mid - cal(mid) >= n) {
            ans = mid;
            r = mid - 1;
        } else {
            l = mid + 1;
        }
    }
    cout << ans << endl;
}
```

区间合并：求$n$个区间$[l, l + w]$的并集长度。
```cpp
void solve() {
    int n, w;
    cin >> n >> w;
    vector<pair<int, int>> a; // l, l + w
    for(int i = 0; i < n; ++i){
        int x, y;
        cin >> x >> y;
        a.push_back({x, x + y});
    }
    sort(a.begin(), a.end());
    int ans = 0, st = -1, ed = -1;
    for(auto&[l, r] : a) {
        if(st == -1) {
            st = l, ed = r;
        } else if(l <= ed) {
            ed = max(r, ed);
        } else {
            ans += (ed - st);
            st = l, ed = r;
        }
    }
    ans += (ed - st);   
    cout << ans << endl;
}
```

均分纸牌问题：有$n$个人，每个人有不同数量的纸牌，可以选择相邻的人拿走一张给另一个人，求出让所有人纸牌相等的次数。
```cpp
void solve() {
    int n;
    cin >> n;
    vector<int> a(n);
    for(int i = 0; i < n; ++i) {
        cin >> a[i];
    }
    int sum = accumulate(a.begin(), a.end(), 0LL);
    if(sum % n) {
        cout << -1 << endl;
        return;
    }
    int avg = sum / n, ans = 0, pre = 0;
    for(int i = 0; i < n - 1; ++i) {
        pre += (a[i] - avg);
        ans += abs(pre);
    }
    cout << ans << endl;
}
```


### 字符串

统计字符串中出现最频繁的固定长度的子串，如果频率相同按升序排序
```cpp
void solve() {
    int n, k; cin >> n >> k;
    string s; cin >> s;
    map<string, int> mp;
    for(int i = 0; i < s.size() - k + 1; ++i) {
        string t = s.substr(i, k);
        mp[t]++;
    }
    int ans = 0, num = 0;
    vector<string> t;
    for(auto i : mp) if(i.second > ans) ans = i.second;
    for(auto i : mp) if(i.second == ans) t.push_back(i.first);
    sort(t.begin(), t.end());
    cout << ans << endl;
    for(auto i : t) cout << i << ' ';
    cout << endl;
}
```



### 结论
- 交换相邻两项的数组使其有序，所要的操作次数就是逆序对数
- 一个区间的和是 $K$ 的倍数，等价于这个区间两端的“前缀和”对 $K$ 取余的结果相同（同余定理）
- 