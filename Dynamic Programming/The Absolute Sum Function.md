## THE ABSOLUTE SUM FUNCTION
##### Problem Link : [The Absolute Sum Function](https://hack.codingblocks.com/contests/c/1001/1074)  

Just try every possibility out there. Either you place `1` or `B[i]` in the answer , because you have to maximise the sum of the absolute difference, hence only extreme values can contribute to the answer. So keep maintaining the answer in `dp[]` array.

`dp[0][i]` means maximum achievable sum by placing `1` at index `i` using the first `i` elements only.
`dp[1][i]` means maximum achievable sum by placing `B[i]` at index `i` using the first `i` elements only.

Now for computing them, find the answer by taking the maximum of the two cases when you have place either `1` or `B[i-1]` at index `i-1`.

_**Time Complexity:** O(N)_ where N is the size of the array

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
//   freopen("output.txt","w",stdout);
  ll t=1;
  s(t);
  while(t--){
    ll n;
    s(n);
    vector<ll>ar;
    F(i,0,n-1){
      ll num;
      cin>>num;
      ar.pb(num);
    }

    if(n==1){
      cout<<"0\n";
      continue;
    }

     vector<ll>dp[2];
     // dp[0][i] means maximum achievable sum by placing 1 at index i
    dp[0]=vector<ll>(n,0ll);  
     // dp[1][i] means maximum achievable sum by placing ar[i] at index i
    dp[1]=vector<ll>(n,0ll);

    for(ll i=1;i<n;i++){
      dp[0][i]=max(dp[0][i-1]+abs(1ll-1ll),dp[1][i-1]+abs(1ll-ar[i-1])); // either place 1 or ar[i-1] at index i-1
      dp[1][i]=max(dp[1][i-1]+abs(ar[i]-ar[i-1]),dp[0][i-1]+abs(ar[i]-1ll)); // either place 1 or ar[i-1] at index i-1
    }
    p(max(dp[0][n-1],dp[1][n-1]));
  }
}

```
