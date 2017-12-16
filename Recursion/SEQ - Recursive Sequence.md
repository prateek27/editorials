## SEQ - RECURSIVE SEQUENCE
##### Problem Link : [SEQ - Recursive Sequence](https://hack.codingblocks.com/practice/p/119/757)  

This problem involves the concept of [Matrix Exponentiation](http://www.geeksforgeeks.org/matrix-exponentiation/), the concept of [Modular Exponentiation](http://www.geeksforgeeks.org/modular-exponentiation-power-in-modular-arithmetic/) and the concept of [Transformation for Linear Recurrences](http://fusharblog.com/solving-linear-recurrence-for-programming-contest/). Before preceding, please get yourself familiar with these concepts.

Once you are familiar with the above mentioned concepts, you can follow the below commented code for understanding this problem.


 For `k=4` , the grid will be like :

     [ c1 1 0 0 ]

     [ c2 0 1 0 ]
     
     [ c3 0 0 1 ]
     
     [ c4 0 0 0 ] 

   The base equation we formed was:

    [a5 a4 a3 a2] = [a4 a3 a2 a1] * (grid) ;

   For solving `n>k` , we will be basically calculating:
   
    [a(n) a(n-1) a(n-2) ... a(n-k-1)] = [a4 a3 a2 a1] * (grid)^(n-k) ;

_**Time Complexity:** O(log(n-k)*dim<sup>3</sup>)_ where dim=4 (the dimension the grid), n,k as given in the question.

Read below commented code for implementation details.
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
ll mod = 1e9;
ll gcd(ll a , ll b){return b==0?a:gcd(b,a%b);}
ll k;

// Read matrix multiplication to understand this function
vector<vector<ll> > matMul(vector<vector<ll> > &mat1, vector<vector<ll> > &mat2){
  vector<vector<ll> > mat(k,vector<ll>(k,0));
  for(ll i=0;i<k;i++){
    for(ll j=0;j<k;j++){
      ll sum=0;
      for(ll pp=0;pp<k;pp++){
        sum+=(mat1[i][pp]*mat2[pp][j])%mod;
        sum%=mod;
      }
      mat[i][j]=sum;
    }
  }
  return mat;
}

// Read modular exponentiation to understand this function
void matExpo(vector<vector<ll> > &grid, ll n){
  if(n==1)return;

  vector<vector<ll> > mat(k,vector<ll>(k,0));
  F(i,0,k-1){
    mat[i][i]=1;
  }

  while(n>0){
    if(n&1){
      mat = matMul(mat,grid);
    }
    grid=matMul(grid,grid);
    n>>=1;
  }
  grid=mat;
  return;
}

int main()
{
  // freopen("input.txt","r",stdin);
   // freopen("output.txt","w",stdout);
  ll t=1;
  s(t);
  while(t--){
    s(k);
    vector<ll> b,c;
    F(i,1,k){
      ll num; 
      s(num);
      b.pb(num);
    }
    F(i,1,k){
      ll num;
      s(num);
      c.pb(num);
    }

    ll n;
    s(n);
    if(n<=k){ cout<<b[n-1]<<endl; continue;}

    vector<vector<ll> > grid(k,vector<ll>(k,0));

    F(i,0,k-1)grid[i][0]=c[i];
    
    F(i,0,k-1){
      F(j,1,k-1){
        if(j==i+1)grid[i][j]=1;
      }
    }

    // for k=4 , the grid will be like :
    //  [ c1 1 0 0 ]
    //  [ c2 0 1 0 ]
    //  [ c3 0 0 1 ]
    //  [ c4 0 0 0 ]

    // The base equation we formed was:
    // [a5 a4 a3 a2] = [a4 a3 a2 a1] * (grid) ;

    // for solving n>k, we will be basically calculating:
    // [a(n) a(n-1) a(n-2) ... a(n-k-1)] = [a4 a3 a2 a1] * (grid)^(n-k) ;

    matExpo(grid,n-k);

    ll sum=0;
    F(i,0,k-1){
      sum+=(b[k-i-1]*grid[i][0])%mod;
      sum%=mod;
    }
    cout<<sum<<endl;
  }
}

```
