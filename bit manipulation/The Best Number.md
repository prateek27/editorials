## THE BEST NUMBER
##### Problem Link : [The Best Number](https://hack.codingblocks.com/contests/c/1001/1198)  

For given **N** integers, here we will maintain the **freq** array, with size **32**, where  **freq[i]** will store the count of integers in which **i<sup>th</sup>** bit is set. The bit having maximum count and is smallest is the required answer. 
For the above operation, for each integer, we can go through all the 32 bits and check whether the **i<sup>th</sup>** bit is set in the integer or not. If yes, increase the count **freq[i]** by **1**.

_**Time Complexity:** O(T * N * log<sub>2</sub>num)_ where T = Number Of Test Cases, N = Total numbers for that test case , num = 32 bit Number

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
    ll n;
    s(n);
    vector<ll> freq(32);
    while(n--){
      ll num;
      s(num);
      // bt will keep track of which bit is currently checked.
      ll bt=0;

      while(num>0){
        // if bt bit is set, freq for that bit is increased.
        if(num&1)freq[bt]++;

        // Move to the next bit;
        num>>=1;

        // Also increment the bit
        bt++;
      }
    }

    // Finally output that bit which has the maximum frequency 
    ll ans=0;
    ll maxi=0;
    for(ll bt=0;bt<32;bt++){
      if(freq[bt]>maxi){
        maxi=freq[bt];
        ans=bt;
      }
    }
    p(ans);
  }
  return 0;
 }
```
