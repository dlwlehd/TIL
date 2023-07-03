# 2023-07-03 Week 1 배운 점 정리

# 동적 계획법

동적계획법이란 큰 문제에 대한 답을 얻기 위해 동일한 문제이지만 크기가 더 작은 문제들을 먼저 해결한 뒤, 그 결과들을 이용해 큰 문제를 비교적 간단하게 해결하는 기법을 뜻합니다.

## 구현 방법

+ Memoization
    + 값을 기록하고, 그 기록한 값을 참조하는 방법입니다.

+ Tabulation
    + 순서대로 배열에 값을 채워나가는 방식입니다.

## 문제 유형 - Subproblem을 그대로 합치면 되는 문제

+ [피보나치 수](https://www.acmicpc.net/problem/2748)

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
    int n;
    cin >> n;
    int dp[50];
    dp[1] = dp[2] = 1;
    for (int i = 3; i <= 45; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }

    cout << dp[n];
}
```

+ [계단 오르기](https://www.acmicpc.net/problem/2579)

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

+ [타일 채우기 3](https://www.acmicpc.net/problem/14852)
```cpp
#include <bits/stdc++.h>

using namespace std;

int n;
long long dp[1002];

#define MOD 1000000007

int main() {
    cin >> n;

    dp[0] = 1;
    dp[1] = 2;

    for(int i = 2; i <= n; i++) {
        dp[i] = (dp[i - 1] * 2 + dp[i - 2] * 3) % MOD;
        for(int j = i - 3; j >= 0; j--)
            dp[i] = (dp[i] + dp[j] * 2) % MOD;
    }
    
    cout << dp[n];
}
```

+ [서로 다른 BST의 개수](https://leetcode.com/problems/unique-binary-search-trees/)

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
    int n;
    cin >> n;
    int dp[22] = {};
    dp[0] = dp[1] = 1;

    for (int i = 2; i <= n; i++) {
        for (int j = 1; j <= i; j++) {
            dp[i] += dp[j - 1] * dp[i - j];
        }
    }

    cout << dp[n];
}
```

+ [2 x n 타일링 2](https://www.acmicpc.net/problem/11727)

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
    int n;
    cin >> n;
    ll dp[1002] = {};
    dp[0] = dp[1] = 1;

    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 2] * 2 + dp[i - 1];
        dp[i] %= 10007;
    }
    
    cout << dp[n];
}
```