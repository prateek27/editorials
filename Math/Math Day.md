## MATH DAY
##### Problem Link : [Math Day](https://hack.codingblocks.com/admin/preview/1251)  

We can take advantage of a basic property of exponents: a<sup>b * c</sup> can be rewritten as a<sup>b<sup>c</sup></sup>.

So, we can write A<sup>N!</sup> as A<sup>1<sup>2<sup>3</sup></sup>....</sup> And, since multiplication modulo P is associative, we can maintain our answer modulo P at all times.

We will be using [Modular exponentiation](https://www.geeksforgeeks.org/modular-exponentiation-power-in-modular-arithmetic/) to compute exponentiation in log E time where E is the power to be computed.

_**Time Complexity:** O(N log N)_ where N is described in the question

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

// Modular Exponentiation
ll modExp(ll a,ll e,ll mod){
  if(e==0)return 1ll;
  if(a==0) return 0ll;

  ll pro=1;
  while(e>0){
    if(e&1)pro=(pro*a)%mod;
    a=(a*a)%mod;
    e=e/2;
  }
  return pro;
}

int main()
{
  //freopen("input.txt","r",stdin);
   //freopen("output1.txt","w",stdout);
  ll t=1;
  s(t);
  while(t--){
    ll a,n,pp;
    s3(a,n,pp);
    ll ans = a;
    F(i,1,n)ans=modExp(ans,i,pp);
    cout<<ans<<endl;
  }
  return 0;
 }

 

```
