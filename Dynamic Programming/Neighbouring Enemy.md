## NEIGHBOURING ENEMY
##### Problem Link : [Neighbouring Enemy](https://hack.codingblocks.com/practice/p/140/1036)  

This is a very basic problem.
First, maintain the map of the numbers and their frequency in an ordered map. This takes `O(NxlogN)` time. Then make another array of distinct elements in the array and traverse through it.
Let `dp[i] = maximum sum possible till the ith number of this new array`. 
Here, we have only two possibilities:

>Either we take the ith digit and delete it, Or

>We take another digit,1 less than the current digit, and delete that

We pick the maximum of the two.

_**Time Complexity: O(NxlogN)**_ where N = Number Of Elements in the Array

Read below code for implementation details.
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
    s(n);
    vector<ll>b;
    map<ll,ll>mp;
    ll num;
    F(i,0,n-1){cin>>num; mp[num]++;}
    for(auto ele:mp){
      b.push_back(ele.first);
    }

    n=b.size();
    vector<ll>dp(n,0);
    dp[0]=b[0]*mp[b[0]];

    for(ll i=1;i<n;i++){
      if(b[i]==b[i-1]+1){
        ll temp = b[i]*mp[b[i]];
        if(i>=2) temp+=dp[i-2];
        dp[i]=max(dp[i-1],temp);
      }else dp[i]=dp[i-1]+b[i]*mp[b[i]];
    }
    cout<<dp[n-1]<<endl;
  }
}


```
