## MODULO SUM
##### Problem Link : [Modulo Sum](https://hack.codingblocks.com/practice/p/119/754)  

In this question,we need to find any possible subsequence whose total sum is zero when taken modulo with a given number `modulo`. So, the first thought that should come into our mind is that we have an option to pick or not to pick a given number in the needed subsequence. 

Now, once we choose a number, we need to see if choosing this number gives us the needed result or not.For that, we need to maintain the modulo of the sum of the subsequence chosen so far.So when we pick a number, we can directly do the modulo addition with that modulo and see what we get. The moment we get zero,we will return true.

_**Time Complexity:** O(N*P)_ where N = Number of elements in the array and P = Number whose modulo zero is to be calculated.

Read below commented code for implementation details.
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

map<pair<ll,ll>,bool> dp; 

// Returns the result if there is a nonempty set of elements  whose sum%m is zero
bool isPossible(vector<ll> &vec, ll m, ll ind, ll modulo){
  // base case : if we have reached the end, we will see if the modulo value is zero or not
  // Note that the modulo will contain the remainder obtained after dividing the sum of all 
  // the elements considered so far by m.
  if(ind>=vec.size())return modulo==0;

  // Return the solution if already calculated
  if(dp.find({ind,modulo})!=dp.end())return dp[{ind,modulo}];
 
  // If by adding the current element in the overall sum, we attain modulo zero , return success.
  if(vec[ind]%m == m-modulo)return dp[{ind,modulo}]=true;

  // Else consider the two options, either add this element in the overall sum or don't add.
  return dp[{ind,modulo}]= isPossible(vec,m,ind+1,modulo) or isPossible(vec,m,ind+1,(modulo+vec[ind])%m);  
}

int main()
{
  // freopen("input.txt","r",stdin);
   // freopen("output.txt","w",stdout);
  ll t=1;
  //s(t);
  while(t--){
    ll n,m;
    s2(n,m);
    vector<ll>vec;
    F(i,1,n){
      ll num;
      cin>>num;
      vec.push_back(num);
    }
    if(isPossible(vec,m,0,m))cout<<"YES\n";
    else cout<<"NO\n";
  }
}
```
