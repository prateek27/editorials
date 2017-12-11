## 0-N Knapsack
#### Problem Link : [0-N Knapsack](https://hack.codingblocks.com/contests/c/1001/922)

This is a variation of standard 0-1 knapsack. Here, you can pick an item unlimited number of times. So, the recurrence becomes :

_**f(i,j) = max(f(i,j-1), val(j)+f(i-wt(j], j)**_

_**Time complexity: ** O(n*cap)_

C++ code (bottom-up approach):

```C++
#include<bits/stdc++.h>
#define mod 1000000007
#define pp pair<ll,ll>
#define mp make_pair
#define ll long long
#define pb push_back
#define ff first
#define ss second
using namespace std;

ll dp[1010][1010],n,cap,wt[1010],val[1010];
int main(){
//	freopen("input2.txt","r",stdin);
//	freopen("output2.txt","w",stdout);
	ios::sync_with_stdio(0);
	
	cin>>n>>cap;
	for(int i=1;i<=n;i++)
		cin>>wt[i];
	for(int i=1;i<=n;i++)
		cin>>val[i];
	
	for(int i=0;i<=n;i++)
		dp[i][0] = 0;
	for(int j=1;j<=cap;j++)
		dp[0][cap] = 0;
	
	for(int i=1;i<=cap;i++){
		for(int j=1;j<=n;j++){
			dp[i][j] = dp[i][j-1];
			if(wt[j] <= i)
				dp[i][j] = max(dp[i][j], val[j]+dp[i-wt[j]][j]);
		}
	}
	cout<<dp[cap][n];
	
	return 0;
}

```