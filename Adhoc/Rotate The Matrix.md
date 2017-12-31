## ROTATE THE MATRIX
##### Problem Link : [Rotate The Matrix](https://hack.codingblocks.com/contests/c/1001/1229)  

If we find out how to rotate a matrix by 90&deg;, we can just apply the same procedure on that rotated matrix to obtain 180&deg; and further applying it to get 270&deg;. In order to rotate a matrix by 90&deg; we can use the following relation: 

> Rotated <sub> N−j−1,i </sub> = Original<sub>i,j</sub>,0≤i,j<N

We can succesively rotate A three times by 90&deg; to get the other matrices.

On succesively applying the above formula, we get :

>B<sub>i,j</sub> = A<sub>i,j</sub> || A<sub>N−j−i,i</sub> || A<sub>N−i−1,N−j−1</sub> || A<sub>j,N−i−1</sub>
​​

_**Time Complexity:** O(N <sup>2</sup>)_ where N is the dimension of the square matrix

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

int main()
{
	// freopen("input.txt","r",stdin);
 	// freopen("output.txt","w",stdout);
	ll t=1;
	//s(t);
	while(t--){	
		ll n;
		s(n);
		vector<vector<ll>> a(n+1);
		F(i,1,n){
			a[i].resize(n+1);
			F(j,1,n){
				cin>>a[i][j];
			}
		}

		F(i,1,n){
			F(j,1,n){
				// Basic Observation
				ll res = a[i][j]|a[j][n-i+1]|a[n-j+1][i]|a[n-i+1][n-j+1];
				cout<<res<<" ";
			}
			cout<<endl;
		}
	}
}

```
