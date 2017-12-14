## PLACING THE TILES
##### Problem Link : [Placing the tiles](https://hack.codingblocks.com/practice/p/114/739)  

Note that if we place the tile vertically the problem reduces to finding the number of ways in 2 X n-1 board. If we place the tile horizontally , the problem reduces to finding the number of ways in 2 x n-2  board since  only 1 tile can only be placed in only horizontal manner above the horizontally placed tile.

So total number of ways : F(n)= F(n+1)+F(n+2).

Or if placed in the opposite manner : F(n) = F(n-1) + F(n-2) = nth fibonacci number

_**Time Complexity:** O(n)_ where n is the number of columns

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
  // s(t);
  while(t--){
    ll n;
    cin>>n;
    ll a=1;
    ll b=2;
    // Note that if we place the tile vertically the problem reduces to finding the number of ways in 2 X n-1 board
    // If we place the tile horizontally , the problem reduces to finding the number of ways in 2 x n-2  board since 
    // only 1 tile can only be placed in only horizontal manner above the horizontally placed tile.
    // So total number of ways : F(n)= F(n+1)+F(n+2).
    // Or if placed in the opposite manner : F(n) = F(n-1) + F(n-2) = nth fibonacci number

    if(n==1)cout<<a<<endl;  // base case 1.
    else if(n==2)cout<<b<<endl; // base case 2.
    else{
      ll c;
      for(ll i=3;i<=n;i++){
        c=a+b;
        c%=mod;
        a=b;
        b=c;
      }
      cout<<c<<endl;
    }
  }
}

```
