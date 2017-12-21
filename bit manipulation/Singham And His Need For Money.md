## SINGHAM AND HIS NEED FOR MONEY
##### Problem Link : [Singham And His Need For Money](https://hack.codingblocks.com/contests/c/1001/1194)  

This is a very simple question.Here, main observation is that, if Singham need P Rupees, where P is even, Singham only need P/2 rupees, and Singham's father will deposit another P/2 Rupees. Similarly if P is odd, Singham needs (P-1)/2 Rupees and then Singham's father will deposit (P-1)/2 rupees and then Singham can ask for extra 1 Rupee from Prateek Bhaiya. Going this way, Singham can achieve minimum numbers of rupees, which he will be needing to ask from Prateek Bhaiya. 

_**Time Complexity:** O(log<sub>2</sub>sub>P)_ where P = Required Amount Of Money

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
    s(t);
    while(t--){
      ll P;
      s(P);
      ll ans=0ll;
      while(P){
        if(P&1){ ans++; P--; }
        else {  P=P>>1; }
      }
      p(ans);
    }
    return 0;
   }
```
