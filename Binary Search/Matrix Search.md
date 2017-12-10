## Matrix Search
##### Problem Link : [Matrix Search](https://hack.codingblocks.com/contests/c/126/705)  

This question requires a very basic observation that if we start from right top corner of this row-column sorted matrix, at any moment we would only have two choices to exercise. Either go to the previous column or the next row, therby saving so much time.

_**Time Complexity:** O(R+C)_ where R is the number of rows and C is the number of Columns

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
  freopen("input.txt","r",stdin);
   // freopen("output.txt","w",stdout);
  ll t=1;
  // s(t);
  while(t--){
    ll r,c;
    s2(r,c);

    // Accept the input in the matrix
    vector<vector<ll>>mat;
    mat.resize(r);
    for(ll i=0;i<r;i++){
      mat[i].resize(c);
      for(ll j=0;j<c;j++){
        cin>>mat[i][j];
      }
    }

    // Accept the input 
    ll num;
    cin>>num;

    // Initially begin searching from right top corner
    ll row,col;
    row=0;
    col=c-1;

    // This flag indicates if the number has been found or not
    bool fg=0;

    // This loop will run untill we remain in the bounds of the matrix
    while(row<r and col>=0){
      // If the number is found, set the flag and break
      if(mat[row][col]==num){
        fg=1;
        break;
      }
      // If the matrix element is greater than number, we must check in the previous column as that
      // will contain lower numbers.
      if(mat[row][col]>num)col--;
      else row++; // Else check in the next row which will contain higher numbers.
    }

    if(fg)cout<<"1"<<endl;
    else cout<<"0"<<endl;
  }
}	
```
