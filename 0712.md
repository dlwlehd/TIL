# 2023-07-12 Week 2 배운 점 정리

# 동적 계획법

동적계획법이란 큰 문제에 대한 답을 얻기 위해 동일한 문제이지만 크기가 더 작은 문제들을 먼저 해결한 뒤, 그 결과들을 이용해 큰 문제를 비교적 간단하게 해결하는 기법을 뜻합니다.


## 문제 유형 - 격자 안에서 한 칸 씩 전진하는 DP

+ [정수 삼각형](https://www.acmicpc.net/problem/1932)

```cpp
#include <bits/stdc++.h>

using namespace std;

#define all(v) (v).begin(), (v).end()
#define sz(v) (v).size()
#define X first
#define Y second
typedef long long ll;
typedef pair<int, int> pi;
typedef pair<ll, ll> pll;

void debug() {
    freopen("textfile/input.txt", "r", stdin);
    freopen("textfile/output.txt", "w", stdout);
    freopen("textfile/debug.txt", "w", stderr);
}

void input() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    debug();
}

int main() {
    int n;
    cin >> n;
    ll board[102][102], dp[102][102];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> board[i][j];
            dp[i][j] = 0;
        }
    }

    dp[0][0] = board[0][0];
    for (int i = 1; i < n; i++) {
        dp[i][0] = dp[i - 1][0] + board[i][0];
        dp[0][i] = dp[0][i - 1] + board[0][i];
    }

    for (int i = 1; i < n; i++) {
        for (int j = 1; j < n; j++) {
            dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]) + board[i][j];
        }
    }

    cout << dp[n - 1][n - 1];
}

```

+ [정수 사각형 최솟값의 최대](https://www.codetree.ai/missions/2/problems/maximin-path-in-square/description)

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++) cin >> s[i];
    if (n == 1) {
        cout << s[1];
        return 0;
    }

    d[1][1] = s[1];
    d[1][2] = 0;
    d[2][1] = s[2];
    d[2][2] = s[1] + s[2];
    for (int i = 3; i <= n; i++) {
        d[i][1] = max(d[i - 2][1], d[i - 2][2]) + s[i];
        d[i][2] = d[i - 1][1] + s[i];
    }
    cout << max(d[n][1], d[n][2]);
}
```

+ [정수 사각형 최장 증가 수열](https://www.codetree.ai/missions/2/problems/lis-on-the-integer-grid/submissions)
```cpp
#include <bits/stdc++.h>

using namespace std;

#define all(v) (v).begin(), (v).end()
#define sz(v) (v).size()
#define X first
#define Y second
typedef long long ll;
typedef pair<int, int> pi;
typedef pair<ll, ll> pll;

void debug() {
    freopen("textfile/input.txt", "r", stdin);
    freopen("textfile/output.txt", "w", stdout);
    freopen("textfile/debug.txt", "w", stderr);
}

void input() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    debug();
}

struct tile {
    int value;
    int x;
    int y;

    bool operator<(const tile &i) const {
        return value < i.value;
    }
};

int n;
int board[502][502], dp[502][502];
int dx[4] = {0, 1, 0, -1};
int dy[4] = {1, 0, -1, 0};

int oob(int x, int y) {
    return x < 0 || x >= n || y < 0 || y >= n;
}

int main() {
    cin >> n;
    vector<tile> v;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> board[i][j];
            dp[i][j] = 1;
            v.push_back({board[i][j], i, j});
        }
    }

    sort(all(v));

    for (int i = 0; i < n * n; i++) {
        auto [value, x, y] = v[i];
        for (int dir = 0; dir < 4; dir++) {
            int nx = x + dx[dir];
            int ny = y + dy[dir];
            if (oob(nx, ny)) continue;

            if (board[nx][ny] > value) dp[nx][ny] = max(dp[nx][ny], dp[x][y] + 1);
        }
    }

    int res = INT_MIN;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            res = max(dp[i][j], res);
        }
    }

    cout << res;
}
```

+ [정수 사각형 최댓값의 최소](https://www.codetree.ai/missions/2/problems/minimax-path-in-square/description)

```cpp
#include <bits/stdc++.h>

using namespace std;

#define all(v) (v).begin(), (v).end()
#define sz(v) (v).size()
#define X first
#define Y second
typedef long long ll;
typedef pair<int, int> pi;
typedef pair<ll, ll> pll;

void debug() {
    freopen("textfile/input.txt", "r", stdin);
    freopen("textfile/output.txt", "w", stdout);
    freopen("textfile/debug.txt", "w", stderr);
}

void input() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    debug();
}

int main() {
    int n;
    cin >> n;
    int board[102][102], dp[102][102];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> board[i][j];
        }
    }
    dp[0][0] = board[0][0];

    for (int i = 1; i < n; i++) {
        dp[i][0] = max(dp[i - 1][0], board[i][0]);
        dp[0][i] = max(dp[0][i - 1], board[0][i]);
    }

    for (int i = 1; i < n; i++) {
        for (int j = 1; j < n; j++) {
            dp[i][j] = max(min(dp[i - 1][j], dp[i][j - 1]), board[i][j]);
        }
    }

    cout << dp[n - 1][n - 1];
}
```