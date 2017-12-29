## PIRATES AND THEIR CHESTS
##### Problem Link : [Pirates And Their Chests](https://hack.codingblocks.com/contests/c/1001/1223)  

We have to maximise the amount of treasure the pirates can find.
**Input**
> Array of `n` keys, `X`

> Array of `m` chests, `C`

> Array of `m` treasure values, `Z`

We know that the **i<sup>th</sup>** key can open the **j<sup>th</sup>** treasure if **X<sub>i</sub>** and **C<sub>j</sub>** are co-prime.

For each key, we have a list of chests that can match. You can keep a 2D matrix, M, where **M<sub>ij</sub>** denotes whether the **i<sup>th</sup>** key can match the **j<sup>th</sup>** chest. **M<sub>ij</sub>** is either **0** or **1**.

To speed up all the computations, instead of having a 2D matrix, we can have a 1D matrix, **M**, where **M<sub>i</sub>** is a bitmask. The **j<sup>th</sup>** bit of **M<sub>i</sub>** tells whether the **j<sup>th</sup>** chest matches the **i<sup>th</sup>** key.

Now, we have to choose no more than k keys so as to maximize the treasure we can obtain. As `k≤20` , we can iterate over all the 2<sup>n</sup> combinations of keys. 2<sup>n</sup> because a key may be in use or not.

No more than **k** keys can be used though. Let's use the variable, mask to iterate over all possible key combinations. The active bits of mask tell us which keys are in use. Now each key matches a set of chests (i.e., some values in the array C). So, given a mask, we would like to get the set of all chests that match with the given set of keys. 

```C
chests = 0;
for (bit = 0; bit < MAX_BIT; bit++)
if (mask & (1 << bit))
chests |= M[bit];
```

At the end of the above routine, for a given mask, `chests` will be a bitmask whose bits indicate which chests have matched and which haven't. The **i<sup>th</sup>** bit of `chests` tells us whether the **i<sup>th</sup>**  chest has matched or not. If a chest matches, then we can take all of the treasure or gems from the corresponding chest.

```C
treasureVal = 0;
for (bit = 0; bit < MAX_BIT; bit++)
if (chests & (1 << bit))
treasureVal += Z[bit];
```

Following the above routine for each mask which has at most **`k`** bits and taking the max of all possible values of treasureVal that we obtain for each mask, we can obtain the final answer. 

_**Time Complexity:** O(2<sup>N</sup> x max(N,M))_ where N = Number of Keys, M = Number of Chests

Read below commented code for implementation details.
```C++
#include<bits/stdc++.h>
#include<unordered_set>
using namespace std;
 
#define ll long long in
 
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
ll inf = 1e18 ;
ll gcd(ll a , ll b){return b==0?a:gcd(b,a%b);}

int main()
{
  // freopen("input.txt","r",stdin);
  // freopen("output.txt","w",stdout);
  ll t;
  cin>>t;
  while(t--){
    ll n,m,k;
    cin>>n>>m>>k;
    vector<ll>K(n),C(m),Z(m);

    // Chose this data type as we need atleast 100 bits to keep track of 100 possible chests
    __int128_t one=1;

    // key[i] will contain the bit representation of the chests ith key can open
    vector<__int128_t>key(n,0);

    for(ll i=0;i<n;i++)cin>>K[i];
    for(ll i=0;i<m;i++)cin>>C[i];
    for(ll i=0;i<m;i++)cin>>Z[i];

    for(ll i=0;i<n;i++){
      for(ll j=0;j<m;j++){
        if(__gcd(K[i],C[j])>1){
          key[i]|=one<<j;
        }
      }
    }
    ll ans=-inf;
    // all possible combination of K keys from the given n are checked 
    for(ll mask=1;mask<(1<<n);mask++){
      if(__builtin_popcount(mask) > k)continue;
      
      // 'chests' contain the bit representation of the chests which K keys
      // (set bits in the mask)can together open.
      __int128_t chests=0;
      for(ll j=0;j<n;j++){
        if(mask&(1<<j)){
          chests|=key[j];
        }
      }
      ll sum=0;
      // Calculate the total treasure which can be obtained together from 
      // all the chests indicated by the set bits in the variable 'chest'
      for(ll z=0;z<m;z++){
        if(chests&(one<<z))sum+=Z[z];
      }
      ans=max(ans,sum);
    }
    p(ans);
  }
}
```
