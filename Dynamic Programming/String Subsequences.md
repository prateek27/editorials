## STRING SUBSEQUENCES
##### Problem Link : [String Subsequences](https://hack.codingblocks.com/contests/c/1001/751)  

This is an advanced DP problem. However once understood the logic, the coding becomes fairly simple.
The most important thing is to understand the states of the DP. Here we are considering `4-D DP`.
`dp[i][j][k][flag]` represents the answer till the i<sup>th</sup> and j<sup>th</sup> index of the two strings for `k` subsequences remaining and flag represents if the last character from both the strings has been chosen or not to be a part of some subsequence or not.

Now we have two options.
>**Option 1:** Start a new subsequence irrespective of the fact if `str1[i]`==`str2[j]` or not by choosing for `(str1[i] and str2[j+1])` or `(str1[i+1] and str2[j])` and then taking the maximum of the lengths of the two.

>**Option 2:** If `str1[i]==str2[j]`, we have the option to start a new subsequence or continue with the previous subsequence provided the flag is set. 

We will take the maximum of all the cases.

_**Time Complexity:** O(n * m *k * 2)_ where n = length of string 1, m = length of string 2, k = number of subsequences to be made

Refer below commented code for implementation details.

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

ll dp[1005][1005][15][2];
ll n,m;
string str1,str2;

ll solve(ll i,ll j,ll k,ll flag){

  // i and j are the respective indices of the two strings str1 and str2 respctively.
  // k represents the number of disjoint subsets more to be made.
  // if flag is set, it indicates that some previous substring from the two strings are same .
  // This function returns the length as desired in the question till ith and jth index of the two strings resp.

  // we need exact k disjoint subsets, so negative k does not makes any sense, so return -infinity.
  if(k<0)return INT_MIN;

  // this condition indicates that we have reached the end of one or both the strings and also we have 
  // found k disjoint subsets. So we are done. return 0.
  if((i>=n or j>=m) and k==0)return 0;

  // we have reached till the end, still we could not find the exact k disjoint subsets, so return -infinity.
  if((i>=n or j>=m) and k!=0)return INT_MIN;

  // Use memoization
  if(dp[i][j][k][flag] != -1)return dp[i][j][k][flag];

  // these recursive calls analyse two possible cases when we don't wish to continue with any previous 
  // substrings(if any), even if str1[i] and str2[j] are equal.
  //	1. take ith of str1 and j+1th of str2
  //    2. take i+1th of str1 and jth of str2
  // Take maximum of the two.
  ll ans = max(solve(i,j+1,k,0),solve(i+1,j,k,0));

  // Now we will consider this case when the two respective characters are equal
  if(str1[i]==str2[j]){
  	// If the flag is set, some previous susbstrings are already equal for the two strings and we can continue with them
  	// Since we are not creating any new substring, so we have not reduced k.
    if(flag) ans=max(ans,1+solve(i+1,j+1,k,1));

	// We can start a new substring from here itself , that's why we set the flag to 1 
	// for the next recursive call and reduced the k by 1 since we have started a new substring.
    ans=max(ans,1+solve(i+1,j+1,k-1,1));
  }
  dp[i][j][k][flag]=ans;
  return ans;
}

int main()
{
  // freopen("input.txt","r",stdin);
   // freopen("output.txt","w",stdout);
  ll t=1;
  // s(t);
  while(t--){
    ll k;
    memset(dp,-1,sizeof(dp));
    s3(n,m,k);
    cin>>str1>>str2;
    cout<<solve(0,0,k,0)<<endl;
  }
}
```
