## Mike And A Lift Game
##### Problem Link :[Mike and a lift game](https://hack.codingblocks.com/contests/c/203/944)


Lets define our recurrence relation consisting of two states ,'i' and 'j' where i denotes the current floor we are on
and j denotes the stop we have made.For simplicity let abs(i-b)=(i-b)
Then,


dp[j][i]=dp[j-1][i-(i-b)+1]+dp[j-1][i-(i-b)+2]+...dp[j-1][i]+dp[j-1][i+1].....dp[j-1][i+(i-b)-1]

We can observe dp[j][i] depends on a continuous segment of (j-1)th row of of the dp table.Therefore,instead of
calculating dp[j][i] by summing over all possible values,we can store cumulative sum .With the help of this,we can calculate
dp[j][i] in O(1) time.

_**Time Complexity:** O(n*k)_ where n is the number of floors and k is the length of sequence of floors

```C++
# include <iostream>
# include <bits/stdc++.h>
# define ll long long
# define mod 1000000007
using namespace std;
int main()
{
	ll n,k,a,b;
	cin>>n>>a>>b>>k;
	ll dp[2][n+1];
	dp[1][0]=0;
	dp[0][0]=0;
	for(ll j=1;j<=n;j++)
		dp[0][j]=dp[0][j-1]+1;
	for(ll i=1;i<=k;i++)
	{
		for(ll j=1;j<=n;j++)
		{
			if(b>=j)
			{
			    ll x=2*j-b;
			    if(x>n)
                    dp[i%2][j]=dp[i%2][j-1];
                else {
				ll left=2*j-b<1?0:dp[(i+1)%2][2*j-b];
				ll right=dp[(i+1)%2][b-1];
				ll mid=(dp[(i+1)%2][j]-dp[(i+1)%2][j-1]+mod)%mod;
				dp[i%2][j]=(dp[i%2][j-1]+((right-left+mod)%mod-mid+mod)%mod)%mod;}
			}
			else
			{
			    ll x=2*j-b-1;
			    if(x<1)
                    dp[i%2][j]=dp[i%2][j-1];
                else{
				ll right=2*j-b-1>n?dp[(i+1)%2][n]:dp[(i+1)%2][2*j-b-1];
				ll left=dp[(i+1)%2][b];
				ll mid=(dp[(i+1)%2][j]-dp[(i+1)%2][j-1]+mod)%mod;
				dp[i%2][j]=(dp[i%2][j-1]+((right-left+mod)%mod-mid+mod)%mod)%mod;}
			}
		}
	}
	cout<<(dp[(k)%2][a]-dp[(k)%2][a-1]+mod)%mod;
}
```
