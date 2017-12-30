## EXPAND THE POND
##### Problem Link : [Expand the pond](https://hack.codingblocks.com/contests/c/1001/1227)  

First we can use a flood fill algorithm to determine the individual pond and their sizes. Then we can take each dry cell (there's no point in changing the colour of a watered cell) and see what happens if we choose it. We need to check its watered neighbouring cells. We take the sizes of the neighbouring ponds, being careful not to choose the same pond twice, and sum their sizes. The answer is the maximum of these sums plus 1 (for the chosen dry cell).

_**Time Complexity:** O(N x M)_ where N,M are the dimensions of the matrices.

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

// TEMPLATE FOR POLICY BASED DATA STRUCTURES //

// #include <ext/pb_ds/assoc_container.hpp>
// #include <ext/pb_ds/tree_policy.hpp>
// using namespace __gnu_pbds;
// typedef tree<int, null_type, less<int>, rb_tree_tag, tree_order_statistics_node_update> OST;

// TEMPLATE OVER //

ll inf = 1e18;
ll mod = 1e9 + 7 ;
ll r,c;
ll gcd(ll a , ll b){return b==0?a:gcd(b,a%b);}

// Every pond is identified uniquely by a timer variable
ll dfs(ll i,ll j,vector<vector<ll>> &ar,ll timer){
	ar[i][j]=timer;
	ll ct=1;
	if(i-1 > 0 and ar[i-1][j]==1)ct+=dfs(i-1,j,ar,timer);
	if(i+1 <= r and ar[i+1][j]==1)ct+=dfs(i+1,j,ar,timer);
	if(j-1 > 0 and ar[i][j-1]==1)ct+=dfs(i,j-1,ar,timer);
	if(j+1 <= c and ar[i][j+1]==1)ct+=dfs(i,j+1,ar,timer);
	return ct;
}

int main()
{
	// freopen("input.txt","r",stdin);
 	// freopen("output.txt","w",stdout);
	ll t=1;
	//s(t);
	while(t--){
		
		s2(r,c);

		vector<vector<ll>>ar(r+1);

		F(i,1,r){
			ar[i].resize(c+1);
			F(j,1,c){
				cin>>ar[i][j];
			}
		}

		ll ans=-inf;
		// mp stores the size of each pond. Note that each pond is 
		// identified uniquely by a timer.
		map<ll,ll> mp;
		ll timer=2;

		F(i,1,r){
			F(j,1,c){
				if(ar[i][j]==1){
					mp[timer]=dfs(i,j,ar,timer);
					ans=max(ans,mp[timer]);
					timer++;
				}
			}
		}

		// Finally for every dry cell, sum up the size of each pond it is
		// sorrounded by because the moment that dry cell is watered, it will
		// connect all adjacent ponds.
		F(i,1,r){
			F(j,1,c){
				if(ar[i][j]==0){
					set<ll> st;
					if(i-1 > 0 and ar[i-1][j]>1)st.insert(ar[i-1][j]);
					if(i+1 <= r and ar[i+1][j]>1)st.insert(ar[i+1][j]);
					if(j-1 > 0 and ar[i][j-1]>1)st.insert(ar[i][j-1]);
					if(j+1 <= c and ar[i][j+1]>1)st.insert(ar[i][j+1]);

					ll sum=1;
					for(auto it:st){
						sum+=mp[it];
					}
					ans=max(ans,sum);
				}
			}
		}

		p(ans);
	}
}

```
