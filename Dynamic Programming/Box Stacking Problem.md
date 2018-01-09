## BOX STACKING PROBLEM
##### Problem Link : [Box Stacking Problem](https://hack.codingblocks.com/contests/c/1001/1054)  

In this question, the main observation is : 

1. We can rotate boxes. For example, if there is a box with dimensions {1x2x3} where 1 is height, 2×3 is base, then there can be three possibilities, {1x2x3}, {2x1x3} and {3x1x2}.

2. We can use multiple instances of boxes. What it means is, we can have two different rotations of a box as part of our maximum height stack.

So, therefore, for every box given, we will rotate it and generate all possible boxes of different dimensions with just hieght, width, length exchanged.

After this generate a list of all the boxes whose base dimensions are smaller than a particular box for every box, `i`.

Now using DFS, find out the maximum stack height possible when ith box is placed at top. Store the answer in `maxHtPossibleWithIthBoxAtTop[]` array and compute it only once by maintaining a `vis[]` array.

Finally take the maximum of all the elements in the `maxHtPossibleWithIthBoxAtTop[]` array.

_**Time Complexity:** O(N)_ where N is the number of boxes 

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

// array to store a box as a three value tuple (length, breadth, height)
vector<pair<ll,pll>> dims;

// dp array to store the maximum height possible with ith box at top
vector<ll> maxHtPossibleWithIthBoxAtTop;

// list for storing all boxes over which ith box can be placed
vector<vector<ll>>adj;

// boolean array to prevent redundant computations
vector<bool>vis;

void dfs(ll i){
  vis[i]=true;

  ll ans=dims[i].first;
  for(ll j=0;j<adj[i].size();j++){
    if(!vis[adj[i][j]])dfs(adj[i][j]);
    ans=max(ans,maxHtPossibleWithIthBoxAtTop[adj[i][j]]+dims[i].first);
  }
  // Store the answer as done in dp and use it whenever ith box is needed
  maxHtPossibleWithIthBoxAtTop[i]=ans;
}

int main()
{
  // freopen("input.txt","r",stdin);
//    freopen("output.txt","w",stdout);
  ll t=1;
  s(t);
  while(t--){
    ll n;
    s(n);
    dims.clear();
    maxHtPossibleWithIthBoxAtTop.clear();
    adj.clear();
    vis.clear();

    F(i,0,n-1){
      ll h,w,b;
      vector<ll>temp;
      s3(h,w,b);
      temp.pb(h);temp.pb(w);temp.pb(b);
      sort(temp.begin(), temp.end());
      h=temp[0];
      w=temp[1];
      b=temp[2];
      // Generate all possible boxes from the given box by just exchanging the height,width and length.
      // and store them in the dims[] array.
      dims.push_back({h,{w,b}});
      dims.push_back({w,{h,b}});
      dims.push_back({b,{h,w}});
    }

    
    adj.resize(dims.size());
    vis.resize(dims.size());

    maxHtPossibleWithIthBoxAtTop.resize(dims.size());

    for(ll i=0;i<dims.size();i++){
      for(ll j=0;j<dims.size();j++){
        if(i==j)continue;
        if(dims[i].second.first<dims[j].second.first){
          if(dims[i].second.second<dims[j].second.second)
            // means ith box can be placed over jth box.
            adj[i].push_back(j);
        }
      }
    }

    for(ll i=0;i<dims.size();i++){
      if(vis[i])continue;
      dfs(i);
    }

    ll ans=0;
    for(ll i=0;i<maxHtPossibleWithIthBoxAtTop.size();i++)
      ans=max(ans,maxHtPossibleWithIthBoxAtTop[i]);

    p(ans);
  }
}
```
