## Painter's Partition Problem
##### Problem Link : [Painter's Partition Problem](https://hack.codingblocks.com/contests/c/133/716)  
The problem aims at finding the minimum time needed to paint all the boards. So, we should start thinking how we can check if any given time is minimum or not. For this, we can simply check if it is possible to paint all the fences satisfying all the constraints. If it is possible, we can go on checking for smaller values. And if it is not, we can increase the time. 

Now since we have a deciding factor ready using which we can move either left or right, it very much points towards the application of binary search. 
So, clearly, we would be applying the binary search over the time needed to paint all the fences.
We can definitely choose the minimum and maximum time needed to be 0 and INT_MAX(infinity) values. Rather practically speaking, it should be chosen in this manner only as definitely the answer would converge to the correct point. But just for basic understanding, one can think that the maximum time will be taken when only one painter is painting all the boards and rest all are sitting idle. (Which painter we choose, doesn't matter since in the given question, all painters take equal time). However the minimum time can be zero as there can be no fences to be painted. In this way, we can make informed decision about the choices of lower bound and the upper bound of the values of the time taken to start the binary search. But as alredy stated, this won't be needed and we can set them arbitrarily to 0 and infinity respectively in most cases. 

Refer the below commented code to see the implementation details. 

_**Time Complexity:** O()_

```C++
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

// Method to compute if it is possible to paint all the boards using K painters 
// given that each painter can only take atmost "time" units of time.
bool isPossible(vector<ll> &board, ll time, ll k){
  ll i=0;
  ll timeRemaining=time;
  while(k>0 and i<board.size()){
    // if this condition is true, then the painter has enough time to paint the corresponding board
    if(timeRemaining>=board[i]){
      // the total time remaining with the painter gets reduced by the time of the board
      timeRemaining-=board[i];
      // Now move to the next board
      i++;
    }
    else {
      // Since the painter does not has enough time, we move on to the next painter who will initially have "time"
      // units of time 
      k--;
      timeRemaining=time;
    }
  }

  // If i is less than the number of boards, it will indicate that all the boards have not got painted
  // hence false is returned indicating it is not possible to paint all the boards in the allotted time
  if(i<board.size())return false;
  return true;
}

int main()
{
  // freopen("input.txt","r",stdin);
   // freopen("output.txt","w",stdout);
  ll t=1;
  //s(t);
  while(t--){
    ll  k,n;
    s2(k,n);
    vector<ll> board(n);
    // hi will indicate the maximum amount of time needed to paint all the boards
    // It can be found out by assuming that only a single painter will be painting all the boards
    // So just add the length of each board to compute hi
    ll hi=0;
    for(ll i=0;i<n;i++){
      cin>>board[i];
      hi+=board[i];
    }

    // lo indicates the minimum amount of time needed to paint all the boards, initialised by 0.
    ll lo=0;

    // ans represents the minimum amout of time needed to paint all the boards
    ll ans=(ll)INT_MAX;

    while(lo<=hi){
      ll mid=(lo+hi);
      mid>>=1;
      // If it is possible to paint the boards with "mid" time, then we will try reducing the time further, hence hi=mid-1
      if(isPossible(board,mid,k)){
        ans=min(ans,mid);
        hi=mid-1;
      }
      else lo=mid+1;
    }
    p(ans);
  }
}

```
