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




# 基础
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
### 二分基础练习题
```
小红拿到了一个数组，她每次可以进行如下操作：
选择一个数，使其减去 𝑥 ，小红希望 𝑘 次操作之后，该数组的最大值尽可能小。
请你求出这个尽可能小的最大值。
```
```cpp
void solve() {
	int n, k, x; cin >> n >> k >> x;
	vector<int> a(n);
	int mx = 0;
	for(int i = 0; i < n; ++i) {
		cin >> a[i];
		if(a[i] > mx) mx = a[i];
	}
	auto check = [&](int m) -> bool {
		int cnt = 0;
		for(int v : a) {
			if(v > m) cnt += (v - m + x - 1) / x; 
			// 把超过mid的数减少到<=mid需要的操作数，取上界 
			if(cnt > k) return 0;
		}
		return cnt <= k;
	};
	int l = -2e18, r = mx, ans = r;
	while(l <= r) {
		int mid = (l + r) >> 1;
		if(check(mid)) {
			ans = mid;
			r = mid - 1;
		} else {
			l = mid + 1;
		}
	}
	cout << ans << endl;
}
```

### 前缀和
```cpp
void solve() {
	int n; cin >> n;
	vector<int> a(n), pre(n);
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

# MATH

### GCD & LCM
```cpp
int gcd(int a, int b) {
	return b > 0 ? gcd(b, a % b) : a;
}

int lcm(int a, int b) {
	return a / gcd(a, b) * b;
}
```

### 快速幂
```cpp
ll FP(ll b, ll pow){
	ll res = 1;
    ll MOD = 1e9 + 7; 
    b %= MOD;
	while (pow > 0) {
		if (pow & 1) {//此处等价于pow%2==1 
			res = res * b % MOD;
		}
		pow >>= 1; //此处等价于pow=pow/2;  指数减半
		b = (b * b) % MOD;  //底数平方
	}
	return res;
}
```

### 线性筛
```cpp
#include <iostream>
#include <vector>
using namespace std;

const int N = 1000000;

bool check(int x) {
    static vector<int> primes;           // 存质数列表（静态，只初始化一次）
    static vector<bool> is_prime;        // is_prime[i] 表示 i 是否为质数
    static bool initialized = false;     // 标记是否已完成筛法

    // 只在第一次调用时执行筛法
    if (!initialized) {
        initialized = true;
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
### 埃氏筛
```cpp
void pre(int n) {
    is_prime[0] = is_prime[1] = false;
    for (int p = 2; p * p <= n; ++p) {
        if (is_prime[p]) {
            for (int i = p * p; i <= n; i += p) {
                is_prime[i] = false;
            }
        }
    }
}

bool check(int num) {
    if (num < 0 || num >= MAXN) return false;
    return is_prime[num];
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


# 字符串

### KMP
```cpp
int kmp(string text, string pattern) {
    int n = text.size(), m = pattern.size();
    if (m == 0) {
        return 0;
    }
    vector<int> next(m);
    for (int i = 1, j = 0; i < m; i++) {
        while (j > 0 && pattern[i] != pattern[j]) {
            j = next[j - 1];
        }
        if (pattern[i] == pattern[j]) {
            j++;
        }
        next[i] = j;
    }
    for (int i = 0, j = 0; i < n; i++) {
        while (j > 0 && text[i] != pattern[j]) {
            j = next[j - 1];
        }
        if (text[i] == pattern[j]) {
            j++;
        }
        if (j == m) {
            return i - m + 1;
        }
    }
    return -1;
}
```



# DS

### 并查集
```cpp
int find(int x) {
    if (fa[x] == x) { // 如果 x 是根节点
        return x;     // 就返回 x
    }
    return find(fa[x]); // 否则，递归地去找他父亲的根
}
void unite(int x, int y) {
    fa[find(x)] = find(y); // 把 x 的根的父亲，设置成 y 的根
}
```
```cpp
struct DSU {
	vector<size_t> pa; 
  	vector<size_t> size;

  	DSU(size_t size_) : pa(size_), size(size_, 1) {
  	  	iota(pa.begin(), pa.end(), 0);
  	}

  	size_t find(size_t x) {

  	  	if (pa[x] != x) {
  	  	    pa[x] = find(pa[x]);
  	  	}
  	  	return pa[x];
  	}

  	void unite(size_t x, size_t y) {
    x = find(x);
    y = find(y);
    if (x == y) return;
    
    if (size[x] < size[y]) {
        swap(x, y);
    }

    pa[y] = x;
    size[x] += size[y];

  	}
};
```
```cpp
struct DSU_map {
	map<ll, ll> pa; 
  	map<ll, size_t> sz;
  	ll find(ll x) {
        if (pa.find(x) == pa.end()) {
            pa[x] = x; sz[x] = 1; 
            return x; 
        }
  	  	if (pa[x] != x) pa[x] = find(pa[x]);
  	  	return pa[x];
  	}
  	void unite(ll x, ll y) {
        ll rootX = find(x); 
        ll rootY = find(y);
        if (rootX == rootY) return;
        if (sz[rootX] < sz[rootY]) swap(rootX, rootY);
        pa[rootY] = rootX; 
        sz[rootX] += sz[rootY];
  	}
    void clear() { 
        pa.clear(); sz.clear(); 
    }
};
```

### ST表
```cpp
#include <bits/stdc++.h>
using namespace std;

template <typename T>
class SparseTable {
  	using func_type = function<T(const T &, const T &)>;

  	vector<VT> ST;

  	static T default_func(const T &t1, const T &t2) { return max(t1, t2); }

  	func_type op;

 	public:
  		SparseTable(const vector<T> &v, func_type _func = default_func) {
    	op = _func;
    	int len = v.size(), l1 = ceil(log2(len)) + 1;
    	ST.assign(len, vector<T>(l1, 0));
    	for (int i = 0; i < len; ++i) {
    	  	ST[i][0] = v[i];
    	}
    	for (int j = 1; j < l1; ++j) {
    	  	int pj = (1 << (j - 1));
    	  	for (int i = 0; i + pj < len; ++i) {
    	   		ST[i][j] = op(ST[i][j - 1], ST[i + (1 << (j - 1))][j - 1]);
    	  	}
    	}
  	}

  	T query(int l, int r) {
    	int lt = r - l + 1;
    	int q = floor(log2(lt));
    	return op(ST[l][q], ST[r - (1 << q) + 1][q]);
  	}
};
```





# 图
### 邻接矩阵
```cpp
#include <iostream>
#include <vector>

using namespace std;

int n, m;
vector<bool> vis;
vector<vector<bool>> adj;

bool find_edge(int u, int v) { return adj[u][v]; }

void dfs(int u) {
  	if (vis[u]) return;
  	vis[u] = true;
  	for (int v = 1; v <= n; ++v) {
    	if (adj[u][v]) {
      	dfs(v);
    	}
  	}
}

int main() {
  	cin >> n >> m;

  	vis.resize(n + 1);
  	adj.resize(n + 1, vector<bool>(n + 1));

  	for (int i = 1; i <= m; ++i) {
    	int u, v;
    	cin >> u >> v;
    	adj[u][v] = true;
  	}

  	return 0;
}
```


### 邻接表
```cpp
#include <iostream>
#include <vector>

using namespace std;

int n, m;
vector<bool> vis;
vector<vector<int>> adj;

bool find_edge(int u, int v) {
 	for (int i = 0; i < adj[u].size(); ++i) {
 	  	if (adj[u][i] == v) {
 	    	return true;
 	  	}
 	}
 	return false;
}

void dfs(int u) {
  	if (vis[u]) return;
  	vis[u] = true;
  	for (int i = 0; i < adj[u].size(); ++i) dfs(adj[u][i]);
}

int main() {
  	cin >> n >> m;

  	vis.resize(n + 1);
  	adj.resize(n + 1);

  	for (int i = 1; i <= m; ++i) {
  	  	int u, v;
  	  	cin >> u >> v;
  	  	adj[u].push_back(v);
  	}

  	return 0;
}
```

### LCA 最近公共祖先(倍增 & Tarjan)
```cpp
#include <cstring>
#include <iostream>
#include <vector>
constexpr int MXN = 40005;
using namespace std;
vector<int> v[MXN];
vector<int> w[MXN];
int fa[MXN][31], cost[MXN][31], dep[MXN];
int n, m;
int a, b, c;
// dfs，用来为 lca 算法做准备。接受两个参数：dfs 起始节点和它的父亲节点。
void dfs(int root, int fno) {
    // 初始化：第 2^0 = 1 个祖先就是它的父亲节点，dep 也比父亲节点多 1。
    fa[root][0] = fno;
    dep[root] = dep[fa[root][0]] + 1;
    // 初始化：其他的祖先节点：第 2^i 的祖先节点是第 2^(i-1) 的祖先节点第
    // 2^(i-1) 的祖先节点。
    for (int i = 1; i < 31; ++i) {
        fa[root][i] = fa[fa[root][i - 1]][i - 1];
        cost[root][i] = cost[fa[root][i - 1]][i - 1] + cost[root][i- 1];
    }
    // 遍历子节点来进行 dfs。
    int sz = v[root].size();
    for (int i = 0; i < sz; ++i) {
        if (v[root][i] == fno) continue;
        cost[v[root][i]][0] = w[root][i];
        dfs(v[root][i], root);
    }
}
// lca。用倍增算法算取 x 和 y 的 lca 节点。
int lca(int x, int y) {
    // 令 y 比 x 深。
    if (dep[x] > dep[y]) swap(x, y);
    // 令 y 和 x 在一个深度。
    int tmp = dep[y] - dep[x], ans = 0;
    for (int j = 0; tmp; ++j, tmp >>= 1)
        if (tmp & 1) ans += cost[y][j], y = fa[y][j];
    // 如果这个时候 y = x，那么 x，y 就都是它们自己的祖先。
    if (y == x) return ans;
    // 不然的话，找到第一个不是它们祖先的两个点。
    for (int j = 30; j >= 0 && y != x; --j) {
        if (fa[x][j] != fa[y][j]) {
            ans += cost[x][j] + cost[y][j];
            x = fa[x][j];
            y = fa[y][j];
        }
    }
    // 返回结果。
    ans += cost[x][0] + cost[y][0];
    return ans;
}
void Solve() {
    cin.tie(nullptr)->sync_with_stdio(false);
    // 初始化表示祖先的数组 fa，代价 cost 和深度 dep。
    memset(fa, 0, sizeof(fa));
    memset(cost, 0, sizeof(cost));
    memset(dep, 0, sizeof(dep));
    // 读入树：节点数一共有 n 个，查询 m 次，每一次查找两个节点的 lca点。
    cin >> n >> m;
    // 初始化树边和边权
    for (int i = 1; i <= n; ++i) {
        v[i].clear();
        w[i].clear();
    }
    for (int i = 1; i < n; ++i) {
        cin >> a >> b >> c;
        v[a].push_back(b);
        v[b].push_back(a);
        w[a].push_back(c);
        w[b].push_back(c);
    }
    // 为了计算 lca 而使用 dfs。
    dfs(1, 0);
    for (int i = 0; i < m; ++i) {
        cin >> a >> b;
        cout << lca(a, b) << '\n';
    }
}
int main() {
    cin.tie(nullptr)->sync_with_stdio(false);
    int T;
    cin >> T;
    while (T--) Solve();
    return 0;
}
```
```cpp
#include <algorithm>
#include <cstring>
#include <iostream>
using namespace std;

class Edge {
 	public:
  		int toVertex, fromVertex;
  		int next;
  		int LCA;
  		Edge() : toVertex(-1), fromVertex(-1), next(-1), LCA(-1) {};
  		Edge(int u, int v, int n) : fromVertex(u), toVertex(v), next(n), LCA(-1) {};
};

constexpr int MAX = 100;
int head[MAX], queryHead[MAX];
Edge edge[MAX], queryEdge[MAX];
int parent[MAX], visited[MAX];
int vertexCount, queryCount;

int find(int x) {
  	if (parent[x] == x) {
  	  	return x;
  	} 	else {
  	  	return parent[x] = find(parent[x]);
  	}
}

void tarjan(int u) {
  	parent[u] = u;
  	visited[u] = 1;

  	for (int i = head[u]; i != -1; i = edge[i].next) {
  	  	Edge& e = edge[i];
  	  	if (!visited[e.toVertex]) {
  	  	  tarjan(e.toVertex);
  	  	  parent[e.toVertex] = u;
  	  	}
  	}

  	for (int i = queryHead[u]; i != -1; i = queryEdge[i].next) {
  	  	Edge& e = queryEdge[i];
  	  	if (visited[e.toVertex]) {
  	  	  queryEdge[i ^ 1].LCA = e.LCA = find(e.toVertex);
  	  	}
  	}
}

int main() {
  	memset(head, 0xff, sizeof(head));
  	memset(queryHead, 0xff, sizeof(queryHead));

  	cin >> vertexCount >> queryCount;
  	int count = 0;
  	for (int i = 0; i < vertexCount - 1; i++) {
    	int start = 0, end = 0;
    	cin >> start >> end;

    	edge[count] = Edge(start, end, head[start]);
    	head[start] = count;
    	count++;

    	edge[count] = Edge(end, start, head[end]);
    	head[end] = count;
    	count++;
  	}

  	count = 0;
  	for (int i = 0; i < queryCount; i++) {
  	  	int start = 0, end = 0;
  	  	cin >> start >> end;

  	  	queryEdge[count] = Edge(start, end, queryHead[start]);
  	  	queryHead[start] = count;
  	  	count++;

  	  	queryEdge[count] = Edge(end, start, queryHead[end]);
  	  	queryHead[end] = count;
  	  	count++;
  	}

  	tarjan(1);

  	for (int i = 0; i < queryCount; i++) {
   		Edge& e = queryEdge[i * 2];
    	cout << "(" << e.fromVertex << "," << e.toVertex << ") " << e.LCA << endl;
  	}

  	return 0;
}
```

### Dilworth定理
```cpp
对于任意有限偏序集，其最大反链中元素的数目必等于最小链划分中链的数目 
即：一个序列剖成若干个单调不升子序列的最小个数等于该序列最长上升子序列的个数
```



# 板子题

### 统计字符串中出现最频繁的固定长度的子串，如果频率相同按升序排序
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

### 删除逆序数的贪心（局部最优指向全局最优）
```cpp
void solve() {
    string n; cin >> n;
    int k; cin >> k;
    while(k > 0) {
        int idx = 0;
        while(n[idx] <= n[idx + 1]) idx++;
        n.erase(idx, 1);
        k--;
    }
    int st = 0;
    while(st < n.length() - 1 && n[st] == '0') st++;
    cout << n.substr(st) << endl; // 考虑前导零
}
```

### 截取一段连续字串，保证该字串不同的字母种类不超过$k$个
```cpp
void solve() {
	int n, k; cin >> n >> k;
	string s; cin >> s;
	vector<int> cnt(26, 0); // 统计次数
	int ans = 0, l = 0, r = 0, kk = 0; // 左右指针，字符种类数量
	for(r = 0; r < n; ++r) {
		if(cnt[s[r] - 'a'] == 0) kk++; // 如果是新字符，种类++
		cnt[s[r] - 'a']++; // 无论如何都计数
		while(kk > k) {
			cnt[s[l] - 'a']--; // 左指针指向的字符计数--
			if(cnt[s[l] - 'a'] == 0) kk--; // 如果数量变成0，就证明他完全离开这个窗口了，种类--
			l++; // 左指针向右移动
		}
		ans = max(ans, r - l + 1); // 更新
	}
	cout << ans << endl;
}
```

