# 2023-07-21 Week 3 배운 점 정리

# 애드혹 (Ad hoc)

알고리즘과 컴퓨팅 용어에서 "ad-hoc"이라는 표현은 특별히 어떤 문제를 해결하기 위해 특별히 설계된, 일반적인 해결책이 아닌 특별한 해결책을 나타냅니다. Ad-hoc 솔루션은 특정 문제에 대한 특정 해결책이며, 그 이외의 다른 상황이나 문제에는 적용되지 않을 수 있습니다.

예를 들어, 특정한 코딩 문제를 해결하기 위해 작성된 알고리즘은 그 문제를 효율적으로 해결할 수 있지만, 다른 유사한 문제에는 적용할 수 없을 수 있습니다. 이러한 경우, 그 알고리즘은 ad-hoc 알고리즘이라고 할 수 있습니다.

알고리즘 문제를 풀 때 'Ad-hoc' 카테고리는 일반적인 알고리즘이나 데이터 구조를 사용하지 않고, 문제의 특성을 직접 이해하고 그에 맞는 특별한 해결 방법을 요구하는 문제들을 가리키는 경우도 있습니다.


## 문제 유형 - 지극히 최선인 전략이 확실히 정해지는 경우

+ [움직이는 블록](https://www.codetree.ai/missions/5/problems/moving-block/description)

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
    vector<int> a(n);
    for (int &i: a) cin >> i;

    int sum = 0;
    for (int i = 0; i < n; i++) sum += a[i];

    int avg = sum / n;
    int res = 0;
    for (int i = 0; i < n; i++) if (a[i] > avg) res += a[i] - avg;

    cout << res;
}

```

# 이진 탐색 (Parametric Search)

이진 탐색이란 데이터가 정렬돼 있는 배열에서 특정한 값을 찾아내는 알고리즘이다. 배열의 중간에 있는 임의의 값을 선택하여 찾고자 하는 값 X와 비교한다. X가 중간 값보다 작으면 중간 값을 기준으로 좌측의 데이터들을 대상으로, X가 중간값보다 크면 배열의 우측을 대상으로 다시 탐색한다. 동일한 방법으로 다시 중간의 값을 임의로 선택하고 비교한다. 해당 값을 찾을 때까지 이 과정을 반복한다.


+ [자연수 n개의 합](https://www.codetree.ai/missions/8/problems/sum-of-n-natural-numbers/submissions)

```cpp
#include <iostream>

using namespace std;

int main() {
    int s;
    cin >> s;

    long long left = 1, right = 1000000000;
    long long ans = left;

    while (left <= right) {
        long long mid = (left + right) / 2;
        long long x = mid * (mid + 1) / 2;

        if (x > s) {
            right = mid - 1;
        } else {
            left = mid + 1;
            ans = max(ans, mid);
        }
    }

    cout << ans;
}
```

+ [정수 분배하기](https://www.codetree.ai/missions/8/problems/distributing-integers/submissions)
```cpp
#include <bits/stdc++.h>

using namespace std;

int n, m;
vector<int> a;

int cnt(int x) {
    if (x == 0) return 0;
    int sum = 0;
    for (int i: a) sum += i / x;
    return sum;
}

int main() {
    cin >> n >> m;
    a.resize(n);
    for (int &i: a) cin >> i;
    
    int left = 0, right = 100000;
    int mx = left;
    while (left <= right) {
        int mid = (left + right) / 2;
        if (cnt(mid) >= m) {
            left = mid + 1;
            mx = max(mid, mx);
        } else {
            right = mid - 1;
        }
    }
    cout << mx;
}
```