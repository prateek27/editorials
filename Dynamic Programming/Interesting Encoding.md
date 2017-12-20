## Interesting Encoding
##### Problem Link : [Interesting Encoding](https://hack.codingblocks.com/contests/c/141/1096)

We have to find the no. of different decodings possible for an encrypted message consisting of integers. We can break this problem into subproblems. We will initialize dp array that such that dp[i] represent possible decodings for first i digits of message. dp[i] will be equal to dp[i-1] if ith index is not zero as we can add another character in all possible decodings of first i-1 digits. Also if ith digit and (i-1)th digit can form a valid character i.e. if they are less than 26 then we can also add another different character in all possible decodings of first (i-2) digits.

Refer below code for implementation details.

**Time Complexity :** O(n)

```C++
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
typedef string st;
#define test int t; cin >> t; while(t--)
#define debug(x) cout << '>' << #x << ':' << x << "\n";
#define endl '\n'
#define off ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0)

bool foo(char a, char b) {
	if (a == '1') return true;
	if (a == '2' && b <= '6') return true;
	return false;
}

int main() {
	off;
	test {
		st s;
		cin >> s;
		int len = s.length();

		ll dp[len+1];
		memset(dp, 0, sizeof dp);

		// Only one decoding possible for first 0 and 1 digit.
		dp[0] = 1;
		dp[1] = 1;

		for(int i = 2; i <= len, i++) {
			// if (i-1)th character is not 0 increase dp[i] by dp[i-1]
			if (s[i-1] != '0') dp[i] = dp[i-1];

			// if (i-1)th and (i-2)th digits can form valid character increase dp[i] by dp[i-2]
			if (foo(s[i-2], s[i-1])) dp[i] += dp[i-2];
		}

		// Print decodings possible for encrypted message to stdout.
		cout << dp[len] << endl;
	}

  	return 0;
}
```