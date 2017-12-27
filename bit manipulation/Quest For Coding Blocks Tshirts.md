## QUEST FOR CODING BLOCKS TSHIRTS
##### Problem Link : [Quest For Coding Blocks TShirts](https://hack.codingblocks.com/contests/c/1001/1219)  

This is a very simple Bit Manipulation Problem. Simply compute the number of set bits in every number assigned to students and then sort them to find the top K students which have the maximum bits assigned and sum them up to find the required answer.

To find the number of set bits, we can use varios techniques but the best one is using **number = number&(number-1)**. This formula simply clears the first set bit from right in number. Keep applying this formula till **number** is greater than 0.

_**Time Complexity:** O(NlogN)_ where N = Number of Students

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
ll mod = 1e9 + 7 ;
ll gcd(ll a , ll b){return b==0?a:gcd(b,a%b);}

int main()
{
  freopen("input.txt","r",stdin);
  freopen("output.txt","w",stdout);
  ll t;
  s(t);
  while(t--){
    ll n,k;
    s2(n,k);
    vector<ll>ar(n);
    while(n--){
      ll num; s(num);
      // ct counts the number of set bits in num
      ll ct=0;
      while(num){
        // This formula simply clears the first set bit from right in num
        num=num&(num-1);
        ct++;
      }
      ar.pb(ct);
    }
    // sort to find the K largest number of set bits and sum them up
    sort(ar.begin(),ar.end());
    ll ans=0;
    for(ll i=ar.size()-1,j=1;i>=0 and j<=k;j++,i--)ans+=ar[i];
    p(ans);
  }
}
```
