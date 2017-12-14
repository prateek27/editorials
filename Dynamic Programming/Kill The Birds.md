## KILL THE BIRDS
##### Problem Link : [Kill The Birds](https://hack.codingblocks.com/contests/c/1001/747)  

In this question, we have two possibilities at every shoot, either we hit the bird, or we miss the bird and we have to maximise the probability of winning the game.So we compute this probability for both the birds at every point and chose the maximum answer.

Refer below commented code for implementation details.

_**Time Complexity:** O(N*W)_ where N = number of coins initially and W = The score needed to win

```C++

#include<bits/stdc++.h>
// #include<unordered_set>
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
ll x,y,n,w;
double p1,p2;
double dp[1005][1005];

double solve(ll n, ll w){
  // If w is less than 0 , then we need not shoot any bird and we have already won, so return 1.0
  if(w<=0)return 1.0;

  // If w is not less than or equal to 0 and we also don't have any coins left, we can't win the match, so return 0.0
  if(n<=0)return 0.0;

  if(dp[n][w]>=0.0)return dp[n][w];

  // Either we kill the bird 1 with p1 percentage and try to obtain the remaining w-x points with n-1 coins
  // or we miss the shot with 1-p1 percentage and try to obtain the remaining w points with n-1 coins.
  double c1 = p1*solve(n-1,w-x) + (1-p1)*solve(n-1,w);

  // Same as above
  double c2 = p2*solve(n-1,w-y) + (1-p2)*solve(n-1,w);

  // Return the maximum of the two
  return dp[n][w]=max(c1,c2);
}

int main()
{
  // freopen("input.txt","r",stdin);
   // freopen("output.txt","w",stdout);
  ll t=1;
  s(t);
  while(t--){    
    ll p1,p2;
    cin>>x>>y>>n>>w>>p1>>p2;
    ::p1=(double)p1/100.0;
    ::p2=(double)p2/100.0;
    memset(dp,-1.0,sizeof(dp));
    double ans = solve(n,w)*100.0;
    printf("%.6f\n",ans);
  }
}

```
