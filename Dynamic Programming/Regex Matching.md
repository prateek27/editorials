## REGEX MATCHING
##### Problem Link : [Regex Matching](https://hack.codingblocks.com/contests/c/1001/1064)  

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

bool isMatch(string str,string pat){
    str='.'+str;
    pat='.'+pat;
    if(str==pat)return true;
    ll n = str.size();
    ll m = pat.size();
    vector<vector<bool>> dp(n,vector<bool>(m,false));
    dp[0][0]=true;

    // This sets the base case when string is of 0 size. In that case if pattern is of x*y*z*....
    // then, dp[0][j] will be true coz in this case , '*' means O or more preceding characters, so 
    // for x*y*z*.... type pattern can be assumed to be pattern of 0 size , hence when string and pattern
    // are both of size 0, dp[][] will be true.
    for(ll j=1;j<m;j+=2){
        if(j+1<m and pat[j+1]=='*'){
            dp[0][j+1]=true;
        }else break;
    }


    for(ll i=1;i<n;i++){
        for(ll j=1;j<m;j++){
            // ignore that character in pattern which has a preceding *, as it will be covered in other cases
            if(j<m-1 and pat[j+1]=='*')j++;
            // '.' matches any single character , hence we don't care for the ith char in string 
            if(pat[j]=='.')dp[i][j]=dp[i-1][j-1];
            else if(pat[j]=='*'){
                // left side of the 'or' considers 0 characters of the preceding char pat[j-1]
                // right side of the 'or' considers 1 or more characters of the preceding char pat[j-1]
                dp[i][j]=dp[i][j-2] or ((str[i]==pat[j-1] or pat[j-1]=='.') and dp[i-1][j]);
            }else{
                dp[i][j] = (str[i]==pat[j]) and dp[i-1][j-1];
            }
        }
    }
    return dp[n-1][m-1];
}



int main()
{
    // freopen("input.txt","r",stdin);
//      freopen("output.txt","w",stdout);
    ll t=1;
    //s(t);
    while(t--){
        string str,pat;
        cin>>str>>pat;
        cout<<isMatch(str,pat)<<endl;
    }
}

```
