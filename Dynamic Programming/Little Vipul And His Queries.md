## LITTLE VIPUL AND HIS QUERIES
##### Problem Link : [Little Vipul And His Queries](https://hack.codingblocks.com/contests/c/1001/1038)  

It is easy to observe that, if you have sufficient number of `R`'s and `B`'s, you can always duplicate any string by placing `R` and `B` in the same order as that string. To put it more formally, you can always reciprocate an entire string if `count['R' with you] >= count['R' in string]` and `count['B' with you] >= count['B' in string]`.So, for each row, we can pre-compute the number of red-colored and blue-colored balls it requires to be reciprocated.

Now, rest is easy. Basically, try to think it this way. You will either decide to reciprocate that row if possible or leave it anyway for the better hope in future. But, how do you decide whether you will be able to reciprocate that row or not. That is only possible, if you have the exact number of red colored and blue colored balls remaining at that moment. This gives rise to the parameters that we will be requiring in forming our DP state.
<ul>
<li>The current index of the row that we are deciding whether to reciprocate or not. </li>
<li>Number of RED colored balls remaining.</li>
<li>Number of BLUE colored balls remaining.</li>
</ul>

_**Time Complexity:** O(N x X x Y + Q)_ where `N` is the number of rows, `X` is the maximum number of Red balls and `Y` is the maximum number of Blue balls. `Q` is the number of queries.

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

ll dp[101][101][101]={0};

int main()
{
//   // freopen("input.txt","r",stdin);
//   freopen("output.txt","w",stdout);
  ll t=1;
  //s(t);
  while(t--){
    ll n;
    s(n);
    ll rd[201]={0};
    ll bl[201]={0};

    F(i,1,n){
      string str;
      cin>>str;
      for(char ch:str){
        if(ch=='R')rd[i]++;
        else bl[i]++;
      }
    }

    
// dp[i][j][k] = maximum rows upto the ith row which can be reciprocateked if the person has
// j red and k blue balls.
// Concept is very simple: Either we reciprocate a row or we don't reciprocate a row, take the maximum one.    

    for(ll i=1;i<=n;i++){
      for(ll x=0;x<=100;x++){
        for(ll y=0;y<=100;y++){
          if(i==1){
            if(x>=rd[i] and y>=bl[i])dp[i][x][y]=1;
          }else{
            dp[i][x][y]=dp[i-1][x][y]; // We do not reciprocate a row
            if(x>=rd[i] and y>=bl[i])dp[i][x][y]=max(dp[i][x][y],1+dp[i-1][x-rd[i]][y-bl[i]]); // We reciprocate the row, take the maximum of the two
          }
        }
      }
    }

    ll q;
    s(q);
    while(q--){
      ll x,y;
      cin>>x>>y;
      cout<<dp[n][x][y]<<endl;
    }
  }
}

```
