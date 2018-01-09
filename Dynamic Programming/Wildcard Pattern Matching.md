## WILDCARD PATTERN MATCHING
##### Problem Link : [Wildcard Pattern Matching](https://hack.codingblocks.com/contests/c/1001/1058)  

Very simple problem, just maintain a DP state for every index of the string and the pattern.

_**Time Complexity:** O(N x M)_ where N is the size of the string, M is the size of the pattern

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

map<pll,bool> dp;

bool doesPatMatches(string &str, string &pat, ll i,ll j){

  // Base Case : will only reach here if the pattern has matched the string
    if(i==str.size() and j==pat.size())return true;
    if(dp.find({i,j})!=dp.end())return dp[{i,j}];

    if(i==str.size()){
        if(pat[j]!='*')return false; // because * means 0 or more characters, hence if it is not `*`, there is no way to match the finished string with the remaining pattern
        return dp[{i,j}]=doesPatMatches(str,pat,i,j+1);// will only return true if pattern has only `*` ahead left
    }
    if(j==pat.size())return dp[{i,j}]=false; // pattern finished, but string is still left
    if(pat[j]=='*'){
      // since '*' means 0 or more characters, the left part of 'or' considers 0 characters and the right part means 1 or more characters
        return dp[{i,j}] = doesPatMatches(str,pat,i,j+1) or doesPatMatches(str,pat,i+1,j);
    }else if(pat[j]=='?'){
        return dp[{i,j}]= doesPatMatches(str,pat,i+1,j+1);
    }else{
        return dp[{i,j}] = str[i]==pat[j] and doesPatMatches(str,pat,i+1,j+1);
    }
}

int main()
{
    // freopen("input.txt","r",stdin);
    //  freopen("output.txt","w",stdout);
    ll t=1;
    // s(t);
    while(t--){
        dp.clear();
        string str,pat;
        cin>>str>>pat;
        cout<<doesPatMatches(str,pat,0,0)<<endl;    
    }
}

```
