## DISCOUNT AT CODING BLOCKS
##### Problem Link : [Discount At Coding Blocks](https://hack.codingblocks.com/contests/c/1001/1226)  

First let's check if we can give discount to a fixed number of students `A`. For this we need `A * X` discount coupons. Initially we have `M` discount coupons, but we can also ask the other N−A students to pay for additional `(N-A) * Y` coupons. So we should check if `M+(N−A) * Y ≥ A* X`.

We can try out all the values of `A` and print the largest one that is possible. As there are `N` different values to check this algorithm runs in `O(N)`.

Now notice that if we can discount `A` students then we are sure we can also give discount to any number of students smaller than `A`. This allows us to binary search the answer, using the previous formula. 

The binary search works in `O(logN)`.

_**Time Complexity:** O(log N)_

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
ll n,m,x,y;

// Returns if it is possible to give 100% discount to 'stud' number of students 
bool solve(ll stud){
  return (stud*x <= m+(n-stud)*y);
}

int main()
{
  // freopen("input.txt","r",stdin);
   // freopen("output.txt","w",stdout);
  ll t=1;
  // s(t);
  while(t--){
      s3(n,m,x);
      s(y);
      ll lo=0;
      ll hi=n;
      ll ans=0;
      // Just do the binary search over the number of students
      while(lo<=hi){
        ll mid=lo+hi;
        mid>>=1;
        if(solve(mid)){ lo=mid+1; ans=mid; }
        else hi=mid-1;
      }
      p(ans);
  }
  return 0;
 }
```
