## SINGHAM AND HIS LOVE FOR LADDUS
##### Problem Link : [Singham And His Love For Laddus](https://hack.codingblocks.com/contests/c/1001/1187)  

Here, we can represent the number of laddus of the two types as a binary string of `0s` and `1s`, where `0s` represent `Type 2` laddus and `1s` represent `Type 1` laddus.The binary string will be valid only if any sequence of `0s` will have a number of `0s` which is divisible by `k` as Singham will only eat laddus of `Type 2`(represented by `0` in the binary string) if they are collectively presented in the group of size `k`.

We can construct it as a dynamic programming problem in this way: 

nr<sub>i</sub> = the number of good strings of length `i`. If the `i-th` character is `"1"` then we can have any character before and if the `i-th` character is `"0"` we must have another `k - 1` `"0"` characters before, so `nr`<sub>`i`</sub> `= nr`<sub>`i-1`</sub> `+ nr`<sub>`i-k`</sub> for `i ≥ k` and `nr`<sub>`i`</sub> `= 1` for `i < k`. Then we compute the partial sums `(sum`<sub>`i`</sub> `= nr`<sub>`1`</sub> `+ nr`<sub>`2`</sub> `+ ... + nr`<sub>`i`</sub>) and for each query the result will be `sum`<sub>`b`</sub> `- sum`<sub>`a-1`</sub>.

_**Time Complexity:** O(maxVal + t)_ where maxVal is the maximum value of b<sub>i</sub>.

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
    ll n,k;
    s2(n,k);
    // dp[i] represents the number possible number of ways Singham can have laddu if
    // i laddus are presented to him.
    vector<ll>dp(100005,0);
    // cum_sum[i] represents the cumulative sum of all the ways in which Singham can
    // have laddu for all number of laddus less than equal to i
    vector<ll>cum_sum(100005,0);
    dp[0]=1ll;
    F(i,1,100000){
      if(i<k)dp[i]=1ll;
      else dp[i]=(dp[i-1]+dp[i-k])%mod;
      cum_sum[i]=(cum_sum[i-1]+dp[i])%mod;
    }
    while(n--){
      ll a,b;
      s2(a,b);
      ll ans=(cum_sum[b]-cum_sum[a-1]+mod)%mod;
      p(ans);
    }
  }
  return 0;
 }
```
