## XORED ARRANGEMENT
##### Problem Link : [XORed Arrangement](https://hack.codingblocks.com/contests/c/1001/1225)  

We are given arrays **A** and **P**. The value of operation of the i<sup>th</sup> value of the array A is defined as `A`<sub>`i-2`</sub> `^ A`<sub>`i-1`</sub>  `^ A`<sub>`i`</sub> `for all i ≥ 2. For i = 0` and `i = 1`, `A`<sub>`i`</sub> `= 0`. We wish to minimize the sum of `A`<sub>`i`</sub> `* P`<sub>`i`</sub> `for i ranging from 0 to n - 1`.

A brute force solution for this problem would be to try all the **n!** permutations of the array **A** and minimize sum of **Ai * Pi** for all **i**. But the complexity of this approach would be **O(n! * n)** which would exceed the time limit. So, let's go for a better approach.

This problem requires dynamic programming with bitmasking. We can see that values at 3 different indices need to be considered for calculating the value at a certain **Ai**. Also, since we have **n** elements, we have **2<sup>n</sup>** possibilities of choosing or not choosing a certain index. This way, we can build subsets of elements we're considering and arrive at a solution for the total problem. We will try an **O(n<sup>2</sup> * 2<sup>n</sup>)** solution that takes **O(n<sup>2</sup> * 2<sup>n</sup>)** space.

Let **DP[mask][fr][sec]** denote the minimum value of the function we wish to minimize considering only the indices that are active in the mask, such that the last index considered so far is `fr` and the second last is `sec`.

>**DP[mask][fr][sec] = min(DP[mask][fr][sec], (A[fr]^A[sec]^A[th]) * P[bitcount(mask) - 1] + DP[mask | (1 << th)][fr][sec])**
 
for all `th` that are inactive in mask and for all `fr` and `sec` that are active in mask. This implies that we're going to `th` having visited `fr` and then `sec` which is why we wish for `th` to be inactive in `mask` and `fr` and `sec` to be active.

_**Time Complexity:** O(n<sup>2</sup> * 2<sup>n</sup>)_ where n is the size of the array.

Read below commented code for implementation details.
```C++
#include<bits/stdc++.h>
#include<unordered_set>
using namespace std;
 
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
ll n;
// dp[mask][first][second] gives the answer for the arrangement where only those
// integers are considered so far for which the bits are set in the mask and 
// the 'first' and 'second' represent numbers which were taken first and second 
//  in the given combination.
ll dp[5000][13][13];


vector<ll> ar,pos;


ll solveDP(ll mask,ll fr,ll sc){
  if(dp[mask][fr][sc]!=-1) return dp[mask][fr][sc];

  // po contains the number of set bits in the mask
  ll po = __builtin_popcountll(mask);

  // if all numbers have been considered, return 0
  if(po==n)return 0;

  
  ll ans=inf;
  // th represents the third number to be chosen after first 'fr' 
  // and second number 'sc',all from the last in the chosen arrangement,
  // have been considered
  for(ll th=0;th<n;th++){
    // if that number has already been considered, continue
    if((mask&(1<<th))!=0)continue;
    // take the xor of the last three numbers just taken and multiply it with
    // pos[po] which tells that the third number just chosen will be placed at
    // the position 'po' in the finally chosen subset.
    ans=min(ans,(ar[fr]^ar[sc]^ar[th])*pos[po] + solveDP(mask|(1<<th),sc,th));
  }
  dp[mask][fr][sc]=ans;
  return ans;
}


int main()
{
  freopen("input.txt","r",stdin);
  freopen("output.txt","w",stdout);
  ll t;
  s(t);
  while(t--){
    s(n);
    memset(dp,-1,sizeof(dp));
    ar = vector<ll>(n);
    pos = vector<ll>(n);
    
    F(i,0,n-1)cin>>ar[i];
    F(i,0,n-1)cin>>pos[i];
    
    if(n<3) { cout<<"0\n"; continue; }

    ll ans=inf;
    // i and j indicates the possible values of the first and second numbers 
    // which can be chosen in any possible arrangement of numbers
    for(ll i=0;i<n;i++){
      for(ll j=0;j<n;j++){
        if(i==j)continue;
        ans=min(ans,solveDP((1<<i)|(1<<j),i,j));
      }
    }
    p(ans);
  }
}
```
