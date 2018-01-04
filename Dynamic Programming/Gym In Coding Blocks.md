## GYM IN CODING BLOCKS
##### Problem Link : [Gym In Coding Blocks](https://hack.codingblocks.com/contests/c/1001/1234)  

Note that paths need to cross exactly one cell and Prateek Bhaiya can go only right and down, Arnav Bhaiya can go only right and up. 
Let's analyze configurations possible for meeting cell.

Prateek Bhaiya can come either from right or down and Arnav Bhaiya can come either from right or up. However, if both come from right, they must have met in other cell as well before (the cell in the left of the meet one). Similarly, if one comes from up and other one from down, their paths will cross either on upper cell, lower cell or right cell.

Only 2 possible cases are: 
>Prateek Bhaiya comes from right, Arnav Bhaiya comes from down 

```C

P	 ____	
|____|___
     |  |
_____|  |
A
```

OR

>Prateek Bhaiya comes from up, Arnav Bhaiya comes from right.

```C
P
_____
____|___|
|   |____
A

```
 

Construct a dp to find the maximum workout possible to reach a cell (i,j) from top left, from bottom left, from top right and from bottom right. Now for any cell (i,j), these four DPs will give the required information needed to analyse any of the two possible configurations.

_**Time Complexity:** O(N x M)_ where N,M = Dimensions of the gym

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

/****************************************************************************/

ll dp[5][1005][1005];

int main()
{
  //freopen("input.txt","r",stdin);
  //freopen("output.txt","w",stdout);
  ll t=1;
  // s(t);
  while(t--){
  	ll n,m;
  	s2(n,m);
  	vector<vector<ll>> gym(n+5,vector<ll>(m+5,0ll));
  	memset(dp,0ll,sizeof(dp));
  	F(i,1,n){
  		F(j,1,m){
  			s(gym[i][j]);
  		}
  	}

  	// Maximum workout possible if one person comes from top left to (i,j)
  	F(i,1,n){
  		F(j,1,m){
  			dp[1][i][j]=max(dp[1][i][j-1],dp[1][i-1][j])+gym[i][j];
  		}
  	}

  	// Maximum workout possible if one person comes from (i,j) to bottom right
  	RF(i,n,1){
  		RF(j,m,1){
  			dp[2][i][j]=max(dp[2][i][j+1],dp[2][i+1][j])+gym[i][j];
  		}
  	}

  	// Maximum workout possible if one person comes from bottom left to (i,j)
  	RF(i,n,1){
  		F(j,1,m){
  			dp[3][i][j]=max(dp[3][i][j-1],dp[3][i+1][j])+gym[i][j];
  		}
  	}

  	// Maximum workout possible if one person comes from (i,j) to top right
  	F(i,1,n){
  		RF(j,m,1){
  			dp[4][i][j]=max(dp[4][i][j+1],dp[4][i-1][j])+gym[i][j];
  		}
  	}
  	ll ans=0ll;

  	// Take the maximum of the two possible cases as discussed above in the editorial for every 
  	// cell (NOT ON BOUNDARIES).
  	F(i,2,n-1){
  		F(j,2,m-1){
  			// Path in which Prateek Bhaiya comes from top and Arnav Bhaiya comes from left to (i,j)
  			ans=max(ans,dp[1][i][j-1]+dp[2][i][j+1]+dp[3][i+1][j]+dp[4][i-1][j]);
  			
  			// Path in which Prateek Bhaiya comes from left and Arnav Bhaiya comes from bottom to (i,j)
  			ans=max(ans,dp[1][i-1][j]+dp[2][i+1][j]+dp[3][i][j-1]+dp[4][i][j+1]);
  		}
  	}
  	p(ans);
  }
  return 0;
 }
```
