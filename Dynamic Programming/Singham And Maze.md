## SINGHAM AND MAZE
##### Problem Link : [Singham And Maze](https://hack.codingblocks.com/contests/c/1001/1188)  

In this problem you had to simulate route of character in graph. 
Note that if you are in vertice **i**, then edges in all vertices with numbers less than **i** are turned to **p<sub>i</sub>**.

Let **dp<sub>i</sub>** represent the number of portals needed to move to the next room.

Let **sum<sub>i</sub>** will represent the cumulative sum of the number of portals needed to move to the `i+1`th room from the first room.

Then just traverse to each room and compute the `dp[i]` values for each `i` in the following manner and update `sum[]` accordingly:

> If portal 2 takes back to the same room, then dp[i] will be 2 because using portal 2 it will come back to the same room at the first(odd) time it enters room i and then using portal 1, he will move to the next room as it will be the second(even) time he enters the room `i`.

> If portal 2 takes to some previous room a, then Singham will use portal 2 to go to the room a, then come back to room i, then use portal 1 to move ahead, so in order to find out the number of portals to move from room a to room i, we will be maintaining a cumulative sum array containing the cumulative sum of the number of portals needed to move from room i to room i+1 for every room from 1 to i.
Hence , the total number of portals will be `2+sum[i-1]-sum[a-1]`.

_**Time Complexity:** O(N)_ where N is the total Number Of Rooms in the maze.

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
  // s(t);
  while(t--){
    ll n;
    s(n);
    vector<ll>ar(n+1,0);
    F(i,1,n)s(ar[i]);
    // dp[i] will represent the number of portals needed to move to the next room
    vector<ll>dp(n+1,0ll);
    // sum[i] will represent the cumulative sum of the number of portals needed to
    // move to the i+1th room from the first room.
    vector<ll>sum(n+1,0ll);
    dp[1]=2;
    sum[1]=2;
    F(i,2,n){
      // if the second portal leads to the same room, you can move to the next room using
      // just two portals, one for coming back to the same portal and the other one to 
      // move to the next portal.
      if(ar[i]==i)dp[i]=2;
      // Else The total number of portals needed will be equal to the number of portals
      // needed to move from i-1 th room to the ith room which is computed using cumulative
      // sum array.
      else dp[i]= 2+(sum[i-1]-sum[ar[i]-1]+mod)%mod;
      sum[i]=sum[i-1]+dp[i];
      sum[i]%=mod;
    }
    // finally sum[n] contains the total number of portals needed to move to the n+1th room 
    // from the first room which is the needed question.
    p(sum[n]);
  }
  return 0;
 }
```
