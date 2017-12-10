## Pair of Roses
##### Problem Link : [Pair of Roses](https://hack.codingblocks.com/contests/c/133/696)  

This problem is a very basic application of binary search. Since we need to find a pair of roses whose sum of prices must satisfy the given sum, we will simply sort the prices of the roses, then traverse the array linearly. For every price, we would like to see if there exists another rose whose price is equal to the remaining money Deepak has. Hence we would do a binary search over the 
array for the needed price.

_**Time Complexity:** O(N*logN)_

**Note that this question can also be accomplished by using hash map in lesser time complexity of O(N) where N is the total number of roses.**



```C
#include<bits/stdc++.h>

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
  ll n;
  ll ct=0;
  ll t;
  s(t);
  while(t--){
    cin>>n;
    ct++;
    // Accept the input in a vector 
    vector<ll>ar(n);
    for(ll i=0;i<n;i++)cin>>ar[i];

    // Sort the vector in ascending order
    sort(ar.begin(), ar.end());

    // Accept the money Deepak has
    ll tar;
    s(tar);

    // Since in cases of multiple solutions, we have to output that one which
    // has the minimum difference betweeen the two prices of the roses, so we
    // are maintaining the difference for every solution and will output the one
    // with minimum difference. Initially, the minimum difference is assumed to be 
    // infinity.

    ll curDif=INT_MAX;
    ll p1,p2;

    for(ll i=0;i<n-1;i++){
      if(ar[i]>tar)break;
      
      // toFind is the value of the price of the second rose in the pair
      ll tofind=tar-ar[i];

      // ind is the index of the toFind variable in the sorted array if it exists
      ll ind = lower_bound(ar.begin()+i+1,ar.end(),tofind)-ar.begin();

      // if ind is greater than n, it indicates that the second rose of the pair 
      // does not exists, so continue finding another pair.
      if(ind>=n)continue;

      // if we have found the second rose of the pair, we will check for the minimum difference condition
      // and then assign the value of the two prices in p1 and p2.
      if(ar[ind]==tofind){
        if(ar[ind]-ar[i]<curDif){
          curDif=ar[ind]-ar[i];
          p1=ar[i];
          p2=ar[ind];
        }
      }
    }
    cout<<"Deepak should buy roses whose prices are "<<p1<<" and "<<p2<<"."<<endl;
  }
}
```
