## SCRABBLING THE BITS
##### Problem Link : [Scrabbling The Bits](https://hack.codingblocks.com/contests/c/1001/1200)  

Since the constraints are very small, it indicates towards using the bitmasking approach as this approach can be ONLY executed with such small constraints.
Now, we have to find the subset which yield the maximum value for those operations. So, we can try all possible subsets and calculate the value when operation is applied on those numbers.

Since we have to apply those operations cyclically, we are maintaining a `todo` variable which decides which opearation to carry out.

_**Time Complexity:** O(2<sup>N</sup> x N)_ where N = 16 

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

ll solve(vector<ll> &ar){
  ll n=ar.size();
  // fin is the final maximum answer
  ll fin=0;
  // Since n can be maximum 16, therefore we can check for all possible subsets
  // which will be 2^N - 1. 
  for(ll i=1;i<pow(2,n);i++){
    // The subset whose set bits will decide which numbers of the array to consider
    // for the subset.
    ll subset = i;
    // todo will decide the operation to apply on the next number
    ll todo = 0;
    /**
      todo == 0 ; first number of the subset, take it as it is.
      todo == 1 : XOR
      todo == 2 : ADD
      todo == 3 : OR
    **/
    ll temp_ans=0;
    for(ll bit=0;bit<n;bit++){
      if(subset&1){
        if(todo==0)temp_ans=ar[bit];
        else if(todo==1)temp_ans=temp_ans^ar[bit];
        else if(todo==2)temp_ans=temp_ans+ar[bit];
        else temp_ans=temp_ans|ar[bit];
        todo=(todo+1)%4;
        if(todo==0)todo=1;
      }
      subset>>=1;
    }
    fin=max(fin,temp_ans);
  }
  return fin;
}

int main()
{
  // freopen("input.txt","r",stdin);
   // freopen("output.txt","w",stdout);
  ll t=1;
  s(t);
  while(t--){
    ll n;
    cin>>n;
    vector<ll>ar;
    ll num;
    F(i,1,n){
      cin>>num;
      ar.pb(num);
    }

    ll fin = solve(ar);
    if(fin&1){
      cout<<"Odd\n";
    }else{
      cout<<"Even\n";
    }
  }
  return 0;
 }
```
