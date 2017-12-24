## FRIENDS AT CODING BLOCKS
##### Problem Link : [Friends At Coding Blocks](https://hack.codingblocks.com/contests/c/1001/1204)  

Here the main observation is that, the batch will always be formed of prime numbers which are less than or equal to **N**. Possible combinations of largest batches can be all the subsets containing numbers which are co-prime to each other, but as we need the lexicographically smallest combination , when sorted in ascending order, it will restrict us to choose all the prime numbers which are less than or equal to N. 
Now in order to get the minimum total friction, we need to arrange them efficiently. To define an efficient arrangement we need to consider each possible arrangement and can see which arrangement gives us the minimum total friction.
We can follow the top down approach of Dynamic Programming with the states mask, and prev.
> mask = represents the prime numbers which are choosen till now.

> prev = represents the last prime number we chose. 

For each position in the batch, we will have the choice of choosing a prime number from the remaining prime numbers which have not been chosen yet. To represent the prime numbers which are already chosen, we will use the mask. 

**How?** 

As there will always be less than `20` prime numbers, you can take the `integer` **(mask )** (where each position of bit will represent the prime number), where `1` on `ith`
position in the mask represents that `ith` prime number is selected earlier in the arrangement, and `0`represent it is not. 

We will the store the answer of state mask,prev in the matrix **dp[prev][mask]** to avoid re-calculation the same state again. here **dp[prev][mask]** represents the minimum friction where total number of primes selected is equal to the number set bits in mask.


_**Time Complexity:** O(N x 2<sup>N</sup>)_ where N = number of primes less than 50

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
#define all(v) v.begin(), v.end()

ll inf = 1e18;
ll mod = 1e9 + 7 ;
ll gcd(ll a , ll b){return b==0?a:gcd(b,a%b);}

/****************************************************************************/

// primes[] contains all primes less than 50
vector<ll> primes;

void mySieve(){
  primes.pb(1);
  primes.pb(2);
  vector<bool>pr(50,true);
  for(ll i=3;i*i<=50;i+=2){
    if(pr[i]){
      for(ll j=i*i;j<=50;j+=2*i)pr[j]=false;
    }
  }
  for(ll i=3;i<=50;i+=2)if(pr[i])primes.pb(i);
}

// dp[prev][mask] contains the answer for a particular mask whose last bit set was 'prev' 
ll dp[20][65536];
ll minFriction(ll finalInd,ll prev,ll mask, ll P){
  
  // if the number of set bits is equal to finalInd+1, this indicates that all friends 
  // have been included
  if(__builtin_popcount(mask)==finalInd+1)return 0ll;
  
  // prev+1 is considered for every prev in order to counter the case when prev is -1
  if(dp[prev+1][mask]!=-1)return dp[prev+1][mask];
  
  ll ret=inf;
  for(ll i=0;i<=finalInd;i++){
    if((1<<i)&mask)continue;// if ith bit is already set, we will ignore this

    // if prev is -1 ith bit is the first bit to be set
    if(prev==-1) ret=min(ret,minFriction(finalInd,i,mask|(1<<i),P));
    // else we will measure the friction and add it up to rest
    else ret=min(ret,((primes[prev]*primes[i])%P+minFriction(finalInd,i,mask|(1<<i),P)));
  }
  return dp[prev+1][mask]=ret;
}

int main()
{
  // freopen("input.txt","r",stdin);
   // freopen("output.txt","w",stdout);
  ll t=1;
  s(t);
  mySieve();
  while(t--){
    memset(dp,-1,sizeof(dp));
    ll N,P;
    s2(N,P);
    ll ind = lower_bound(all(primes),N) - primes.begin();
    if(ind==primes.size() or primes[ind]!=N)ind--;
    // cout<<ind<<endl;
    ll miniFriction = minFriction(ind,-1,0ll,P);
    p(miniFriction);
  }
  return 0;
 }
```
