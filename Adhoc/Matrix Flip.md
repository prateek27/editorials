## MATRIX FLIP
##### Problem Link : [Matrix Flip](https://hack.codingblocks.com/admin/preview/1245)  

First of all, there are two important observations that simplify the problem:

1. The order of the operations doesn't matter.

2. There's no point in applying more than one operation on any row/column. Applying the same operation the second time just cancels the changes made the first time.

Basically, we can choose for each row/column if we perform a flip on it or not. There are 2<sup>N</sup>x 2<sup>M</sup> = 2<sup>N+M</sup> possible ways of activating the rows/columns.

Next we can split these solutions in two categories: those that have an operation on the first column and those that don't. We can treat these two categories independently, with the mention that in the first case we perform the operation of flipping the first column in the beginning. The rest of the moves can be found using a greedy approach:

<ul><li>
  For every row that has the first element equal to 0, perform a flip operation. The elements on the first column can now only be changed by flipping the rows. It is always better to have the most significant bit equal to 1, no matter the values of the other bits. So it makes sense to flip the line if the most significant bit (the one on the first column) is 0.

  <li>For every other column besides the first, perform a flip operation if the column contains more values equal to 0 than 1.
</ul>


_**Time Complexity:** O(N x M)_ where N and M are the dimnesions of the matrix.

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
  freopen("input.txt","r",stdin);
  freopen("output.txt","w",stdout);
  ll t=1;
  // s(t);
  while(t--){
    ll r,c;
    s2(r,c);
    vector<vector<bool>>mat(r,vector<bool>(c,0));
    F(i,0,r-1){
      F(j,0,c-1){
        bool a;
        cin>>a;
        mat[i][j]=a;
      }
    }

// Flip that row whose most adjacent bit is set to zero
    F(i,0,r-1){
      if(mat[i][0]==0)mat[i].flip();
    }
// Since all bits of first column are set to 1, add it to the overall sum+= r x pow(2,c-1).
    ll ans=r*(1ll<<(c-1ll));

    F(j,1,c-1){
      ll ct=0;
      // Now compute the number of 1s and 0s in every column and flip it if 0s exceed 1s.
      F(i,0,r-1){
        if(mat[i][j]==1)ct++;
      }
      // Here the flip takes place (only logically, not the actual flip operation as it's not needed)
      ct=max(ct,r-ct);

      // Now add it to the overall sum , note that any bit at the jth columns will be the c-1-j th bit in the overall number
      ans+=ct*(1ll<<(c-1ll-j));
    }
    p(ans);
  }
}

```
