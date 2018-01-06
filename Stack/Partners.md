## PARTNERS
##### Problem Link : [Partners](https://hack.codingblocks.com/admin/preview/1241)  

This problem can be easily solved by stack data structure.
Since at any moment, we need to find the largest index ,`x`, less than current index,`i`, such that `A[x]>=A[i]`,  we can simply use stack.
Stack can be emulated to only contain array elements in ascending order starting from top of the stack, at any moment. And this is exactly what we need. 
Since while putting a number in stack, all those numbers, which are smaller than the current number to be put, will never be of any use, to the current number or any other number yet to come, since we have to find the <b>largest</b> index which has an element greater than the elemet at index in consideration. Hence one can easily remove the smaller elements from the stack.

Using this, fill two arrays `l2r[]` (left to right) and `r2l[]` (right to left) containing `x` and `y` respectively for each `i`. Note that these two arrays are to be filled in exactly reverse order. Then finally sum them up to find the answer.

_**Time Complexity:** O(N)_ where N is the size of the array.

Read below commented code for implementation details.
```C++
#include<bits/stdc++.h>
#include<unordered_set>
using namespace std;
 
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
ll mod = 1e9 + 7 ;
ll gcd(ll a , ll b){return b==0?a:gcd(b,a%b);}

int main(){
   // freopen("input.txt","r",stdin);
   // freopen("output.txt","w",stdout);

  ll n;
  s(n);
  vector<ll>ar(n+1);
  F(i,1,n)s(ar[i]);

  // l2r[] arrays stores the value of x for every i as stated in the question
  vector<ll>l2r(n+1);
  // r2l[] arrays stores the value of y for every i as stated in the question
  vector<ll>r2l(n+1);
  // st is a stack that stores a pair of numbers ({Number, Index of the number})
  // such that at any moment the stack top will contain a pair of numbers such that
  // the second number in the pair is the largest number smaller than i 
  // in case of l2r[] and smallest number larger than i in case of r2l[] and the first
  // number in the pair is greater than ar[i].
  stack<pll> st;

  // Base case for l2r[]
  l2r[1]=-1;
  st.push({ar[1],1});

  for(ll i=2;i<=n;i++){
    // keep popping out unless the stack top's pair's first number is larger than ar[i]
    while(!st.empty() and st.top().ff<=ar[i])st.pop();
    
    // If stack is empty, simply means there is no possible value of x, so make it -1.
    if(st.empty())l2r[i]=-1;
    // Else y is the index which is stored as the second number of the pair
    else l2r[i]=st.top().ss;
    
    // Now push the current element
    st.push({ar[i],i});
  }
  while(!st.empty())st.pop();

  // Base case for r2l[]
  r2l[n]=-1;
  st.push({ar[n],n});
  for(ll i=n-1;i>0;i--){

    // keep popping out unless the stack top's pair's first number is larger than ar[i]
    while(!st.empty() and st.top().ff<=ar[i])st.pop();

    // If stack is empty, simply means there is no possible value of x, so make it -1.
    if(st.empty())r2l[i]=-1;
    // Else y is the index which is stored as the second number of the pair
    else r2l[i]=st.top().ss;
    // Now push the current element
    st.push({ar[i],i});
  }
  // Now l2r[] and r2l[] respectively stores the values of x and y respectively for each i
  for(ll i=1;i<=n;i++)cout<<l2r[i]+r2l[i]<<" ";

}
```
