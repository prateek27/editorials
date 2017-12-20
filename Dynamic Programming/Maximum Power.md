## Maximum Power
##### Problem Link : [Maximum Power](https://hack.codingblocks.com/contests/c/141/1098)

In this problem we have to calculate the maximum power i.e. the maximum product of a subarray. In this problem we'll assume that minimum possible answer can be 1 and then we can apply modified Kadane's algorithm to find the maximum possible subarray. 

Refer below commented code for implementation details.

**Time Complexity :** O(n)

```C++
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
#define test int t; cin >> t; while(t--)
#define debug(x) cout << '>' << #x << ':' << x << "\n";
#define endl '\n'
#define off ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0)

int main() {
	off;

	int n;
	cin >> n;

	int a[n];

	for(int i = 0; i < n; i++) {	
		cin >> a[i];
	}
	
	// product variable store maximum possible subarray product of entire array.	
	ll product = 1;
	// maxend variable store maximum possible subarray product of array ending at i.
	ll maxend = 1;
	// minend variable store minimum possible subarray product of array ending at i.
	ll minend = 1;

	for(int i = 0; i < n; i++) {
		// if element at ith index is positive multiply maxend by a[i] and if minend is negative it will be multiplied by a[i] else it will be 1.
		if (a[i] > 0) {
			maxend = maxend * a[i];
			minend = min(minend * a[i], (ll)1);
		}
		// if a[i] is 0 then both maxend and minend will be minimum possible product i.e. 1
		else if (a[i] == 0) {
			maxend = 1;
			minend = 1;
		}
		// if element at ith index is negative store value of maxend in temp and now maxend will be maximum of 1 or minend*a[i] and minend will be temp * a[i].
		else {
			int temp = maxend;
			maxend = max((ll)1, minend*a[i]);
			minend = temp * a[i];
		}
		// if product is smaller than maxend, update product to maxend.
		if (product < maxend) prod = maxend;
	}

	// Print the product to stdout.
	cout << product << endl;

  	return 0;
}
```