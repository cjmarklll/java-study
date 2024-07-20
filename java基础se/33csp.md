1.

```c++
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
using pii = pair<int, int>;
const int N = 1e3 + 55, inf = 0x3f3f3f3f, MOD = 1e9 + 7;
signed main() {
    ios::sync_with_stdio(false); cin.tie(nullptr);
    int n, m;
    cin >> n >> m;
    unordered_map<int, int> H1, H2;
    for (int i = 1; i <= n; ++i) {
        int cnt; cin >> cnt;
        unordered_map<int, int> first;
        for (int j = 1; j <= cnt; ++j) {
            int x; cin >> x;
            if (first.find(x) == first.end()) {
                H1[x]++;
                first[x] = 1;
            }
            H2[x]++;
        }
    }
    for (int i = 1; i <= m; ++i) {
        cout << H1[i] << " " << H2[i] << "\n";
    }
    return 0;
}
```



2.

```c++
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
using pii = pair<int, int>;
const int N = 1e3 + 55, inf = 0x3f3f3f3f, MOD = 1e9 + 7;
signed main() {
    ios::sync_with_stdio(false); cin.tie(nullptr);
    int n, m;
    cin >> n >> m;
    unordered_set<string> S, S1, S2;
    for (int i = 1; i <= n; ++i) {
        string s;
        cin >> s;
        for (auto &c : s) {
            if (c >= 'A' && c <= 'Z') {
                c += 'a' - 'A';
            }
        }
        S.insert(s);
        S1.insert(s);
    }
    for (int i = 1; i <= m; ++i) {
        string s;
        cin >> s;
        for (auto &c : s) {
            if (c >= 'A' && c <= 'Z') {
                c += 'a' - 'A';
            }
        }
        S.insert(s);
        S2.insert(s);
    }
    int ans1 = S.size(), ans2 = 0;
    for (auto s : S1) {
        if (S2.find(s) != S2.end()) {
            ++ans2;
        }
    }
    cout << ans2 << "\n" << ans1 << "\n";
    return 0;
}
```



3.

```c++
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
using pii = pair<int, int>;
const int N = 1e3 + 55, inf = 0x3f3f3f3f, MOD = 1e9 + 7;
const double Eps = 1e-8;
signed main() {
    ios::sync_with_stdio(false); cin.tie(nullptr);
    int Task; cin >> Task;
    while (Task--) {
        int m;
        cin >> m;
        vector<unordered_map<string, int> > H(m + 1);
        unordered_set<string> S;
        for (int i = 1; i <= m; ++i) {
            string s;
            cin >> s;
            for (int j = 0; j < (int)s.size(); ) {
                int k1 = j - 1;
                while (k1 + 1 < (int)s.size() && !isdigit(s[k1 + 1])) ++k1;
                int k2 = k1;
                while (k2 + 1 < (int)s.size() && isdigit(s[k2 + 1])) ++k2;
                auto key = s.substr(j, k1 - j + 1);
                auto val = s.substr(k1 + 1, k2 - (k1 + 1) + 1);
                H[i][key] = H[i][key] + stoi(val);
                S.insert(key);
                j = k2 + 1;
            }
        }
        int n = S.size();
        vector<vector<double> > a(n + 1, vector<double>(m + 1));
        auto it = S.begin();
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= m; ++j) {
                a[i][j] = H[j][*it];
            }
            it = std::next(it);
        }
        int rank = 0;
        for (int i = 1; i <= n; ++i) {
            int r = i;
            for (int j = i + 1; j <= n; ++j) {
                if (fabs(a[j][i]) > fabs(a[r][i])) {
                    r = j;
                }
            }
            if (fabs(a[r][i]) < Eps) {
                continue;
            } else {
                ++rank;
            }
            if (r != i) {
                std::swap(a[i], a[r]);
            }
            for (int j = i + 1; j <= n; ++j) {
                const double div = a[j][i] / a[i][i];
                for (int k = i; k <= m; ++k) {
                    a[j][k] -= div * a[i][k];
                }
            }
        }
        if (rank < m) cout << "Y\n";
        else cout << "N\n";
    }
    return 0;
}
```



4.

```c++
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
using pii = pair<int, int>;
const int N = 1e3 + 55, inf = 0x3f3f3f3f, MOD = 1e9 + 7;
signed main() {
    ios::sync_with_stdio(false); cin.tie(nullptr);
    int C, m, n;
    cin >> C >> m >> n;
    vector<pii> val;
    for (int i = 1; i <= m; ++i) {
        int x, w;
        cin >> x >> w;
        val.emplace_back(x, w);
    }
    int ans = m;
    sort(val.begin(), val.end());
    vector<int> L(m + 2), R(m + 2), W(m + 2);
    unordered_map<int, int> Pos;
    for (int i = 0; i < m; ++i) {
        auto p = i + 1, w = val[i].second;
        L[p] = p - 1, R[p] = p + 1;
        W[p] = w;
        Pos[val[i].first] = p;
    }
    vector<int> vis(m + 2);
    for (int _ = 1; _ <= n; ++_) {
        int p;
        cin >> p;
        p = Pos[p];
        priority_queue<int, vector<int>, greater<int> > q;
        auto add = [&](int x) {
            if (++W[x] >= 5) {
                if (!vis[x]) {
                    vis[x] = 1;
                    q.push(x);
                }
            }
        };
        add(p);
        while (!q.empty()) {
            auto x = q.top(); q.pop();
            W[x] = 0;
            --ans;
            if (L[x] > 0) {
                add(L[x]);
            }
            if (R[x] < m + 1) {
                add(R[x]);
            }
            R[L[x]] = R[x];
            L[R[x]] = L[x];
        }
        cout << ans << "\n";
    }
    return 0;
}
```



5.

```c++
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
using pii = pair<int, int>;
const int N = 5e5 + 55, inf = 0x3f3f3f3f, MOD = 1e9 + 7;
int n, m, DFN;
i64 dat[N];
int dep[N], Size[N], son[N], id[N];
int fa[N], top[N], c[N];
vector<vector<int> > G;
void dfs(int x, int depth) {
    dep[x] = depth, Size[x] = 1;
    int maxs = 0, t = 0;
    for (auto v : G[x]) {
        fa[v] = x;
        dfs(v, depth + 1);
        Size[x] += Size[v];
        if (Size[v] > maxs) {
            maxs = Size[v];
            t = v;
        }
    }
    if (t) son[x] = t;
}
void dfs2(int x, int topp) {
    id[x] = ++DFN, top[x] = topp;
    if (son[x]) {
        dfs2(son[x], topp);
    } else {
        return;
    }
    for (auto v : G[x]) {
        if (v == fa[x] || v == son[x]) continue;
        dfs2(v, v);
    }
}
inline int lowbit(int x) { return x & -x; }
void update(int x, int k) {
    for (int i = x; i <= n; i += lowbit(i)) {
        c[i] += k;
    }
}
int query(int l, int r) {
    int ret = 0;
    for (int i = r; i; i -= lowbit(i)) {
        ret += c[i];
    }
    for (int i = l - 1; i; i -= lowbit(i)) {
        ret -= c[i];
    }
    return ret;
}
int Query(int x, int y) {
    int ret = 0;
    while (top[x] != top[y]) {
        if (dep[top[x]] < dep[top[y]]) {
            ret += query(id[top[y]], id[y]);
            y = fa[top[y]];
        } else {
            ret += query(id[top[x]], id[x]);
            x = fa[top[x]];
        }

    }
    if (dep[x] > dep[y]) {
        ret += query(id[y], id[x]);
    } else {
        ret += query(id[x], id[y]);
    }
    return ret;
}
pair<size_t, i64> merge(int x) {
    vector<int> sonx = std::move(G[x]);
    // G[x].swap(std::vector<int>());
    G[x].clear();
    for (auto v : sonx) {
        if (G[v].size() > G[x].size()) std::swap(G[x], G[v]);
        for (auto t : G[v]) {
            G[x].push_back(t);
        }
        dat[x] += dat[v];
        // G[v].swap(std::vector<int>());
        G[v].clear();
        update(id[v], -1);
    }
    return make_pair(G[x].size(), dat[x]);
}
signed main() {
    ios::sync_with_stdio(false); cin.tie(nullptr);
    cin >> n >> m;
    G.resize(n + 1);
    for (int i = 2; i <= n; ++i) {
        int par;
        cin >> par;
        G[par].push_back(i);
    }
    for (int i = 1; i <= n; ++i) {
        cin >> dat[i];
    }
    dfs(1, 0);
    dfs2(1, 1);
    for (int i = 1; i <= m; ++i) {
        int op, x;
        cin >> op >> x;
        if (op == 1) {
            auto ans = merge(x);
            cout << ans.first << " " << ans.second << "\n";
        } else {
            int dx = Query(1, x);
            cout << dep[x] + 1 + dx << "\n";
        }
    }
    return 0;
}
```

