## MONEY CHANGE
##### Problem Link : [Money Change](https://hack.codingblocks.com/contests/c/141/1026)  

This is a very basic Dynamic Programming Approach based solely on the observation that either we take a denomination or not take a denomination. When we try to skip a denomination, we skip it completely but when we try to take a denomination, we can still take it another time as there are infinite amount of coins available for every destination. The following code is written in a bottom up manner.

_**Time Complexity:** O(m*N)_ where **m** = Total Number of Denominations and **N** = Target Amount to be made

Read below commented code for implementation details.
```C++

#include<bits/stdc++.h>
#include<unordered_set>
using namespace std;
 #define fio ios_base::sync_with_stdio(false)
 
#define ll long long int

#define s(x) scanf("%lld",&x)
#define s2(x,y) s(x)+s(y)
#define s3(x,y,z) s(x)+s(y)+s(z)
 
#define p(x) printf("%lld\n",x)
#define p2(x,y) p(x)+p(y)
#define p3(x,y,z) p(x)+p(y)+p(z)
#define F(i,a,b) for(ll i = (ll)(a); i <= (ll)(b); i++)
#define RF(i,a,b) for(ll i = (ll)(a); i >= (ll)(b); i--)
 
#define ff first
#define ss second
#define mp(x,y) make_pair(x,y)
#define pll pair<ll,ll>
#define pb push_back

ll inf = 1e18;
ll mod = 1e9 + 7 ;
ll gcd(ll a , ll b){return b==0?a:gcd(b,a%b);}

int main()
{
	// freopen("input.txt","r",stdin);
 	// freopen("output.txt","w",stdout);
	ll t=1;
	s(t);
	while(t--){
		ll n;
		s(n);
		vector<ll> coins(n);
		F(i,1,n)cin>>coins[i-1];
		ll N;
		s(N);

		// dp[i][j] represents the number of ways to make change for j cents using coins[0] to coins[i] denominations. 
		vector<vector<ll>> dp(n,vector<ll>(N+1,0));
		F(i,0,n-1)dp[i][0]=1ll;
		F(j,1,N){
			F(i,0,n-1){
				// Option 1: You dont't take that denomination
				if(i>0)dp[i][j]=dp[i-1][j];
				if(coins[i]<=j){
					// Option 2: You take the denomination 
					dp[i][j]+=dp[i][j-coins[i]]; // Total number of ways is the sum of both these options
					dp[i][j]%=mod;
				}
			}
		}
		p(dp[n-1][N]);
	}
}


```
