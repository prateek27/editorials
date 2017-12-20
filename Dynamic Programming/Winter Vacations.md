## WINTER VACATIONS
##### Problem Link : [Winter Vacations](https://hack.codingblocks.com/contests/c/1001/1185)  
 
This is a Dynamic Programming Problem.
In Problems asking the minimum value, one can always think to find the opposite maximum value as it helps in setting the base values easily. So,let's consider the problem differently, and let's find the **maximum** number of days in which Singham **did NOT** take rest.

Let's define a 2D DP, `dp[i][j]` where `dp[i][j]` equals to the maximum number of days,in which Singham will take rest, if `i` days passed and `j` equals to:
>`0`, if Singham had taken rest during the i-th day, 

>`1`, if Singham participated in the contest on the i-th day, 

>`2`, if Singham went to gym on the i-th day. 

Then `dp[i][..]` for the `i`<sup>th</sup> day will be calculated as follows for different `j`:

> `dp[i][0]` must be updated with maximum value of the array dp for the previous day

> If there will be a contest on the `i`-<sup>th</sup> day (i. e. `a[i]` equals to 1 or 3), than we update `dp[i][1]` with maximum of the values `dp[i - 1][0] + 1` and `dp[i - 1][2] + 1`, else `dp[i][1] = 0` as it should not influence at all the future values of i+1, and since we are taking the maximum value, 0 will not influence our answers.

> if the gym is open on the `i-th` day (i. e. `a[i]` equals to 2 or 3), than we update `dp[i][2]` with values `dp[i - 1][0] + 1` and `dp[i - 1][1] + 1`, else zero.

After that we need to choose maximum from all values of the array `dp` for the last day and print `n - max`.

_**Time Complexity:** O(N)_ where N = Number of days in vacation

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

int main()
{
  // freopen("input.txt","r",stdin);
   // freopen("output.txt","w",stdout);
  ll t=1;
  // s(t);
  while(t--){
    ll n;
    s(n);
    vector<ll> ar(n+1);
    F(i,0,n-1)s(ar[i+1]);
    // dp[i][j] represents the maximum number of days Singham will NOT take rest till ith day 
    // if he does the jth activity :
    // j=0 if he is resting, j is 1 if he is giving contest,j is 2 if he is gymming .
    ll dp[n+1][3]={0};
    F(i,1,n){
      dp[i][0]=max(dp[i-1][0],max(dp[i-1][1],dp[i-1][2]));
      if(ar[i]==1 or ar[i]==3)dp[i][1]=1+max(dp[i-1][0],dp[i-1][2]);
      if(ar[i]==2 or ar[i]==3)dp[i][2]=1+max(dp[i-1][0],dp[i-1][1]);
    }
    // Since we have found the complementary answer i.e. Maximum days Singham will not take rest,
    // we will subtract it with the total days to get the actual answer,i.e. Minimum days when 
    // Singham will take rest.
    p(n-max(dp[n][0],max(dp[n][1],dp[n][2])));
  }

  return 0;
 }
```
