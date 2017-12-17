## MIN STRING
##### Problem Link : [Min String](https://hack.codingblocks.com/contests/c/141/1027)  

Let's fix the character that is going to stay in the final string. Let `dp[ch][i]` be the smallest length of the resultant string which can be achieved by applying the reduction operation over the prefix ending at position i in an optimal order such that the characters left on the final string are equal to ch.

Now how to calculate  `dp[ch][i]`?

The key observation here is that we must convert a suffix of that prefix to a single character  which is equal to ch.So for all j<=i we need to check if we can reduce the suffix starting from  index j ( and ending at index i) to a single character equal to  ch. If we can , then   
> `dp[ch][i]=min(dp[ch][i],1+dp[ch][j-1])for all such j.`

_**Time Complexity:** O(N<sup>3</sup>)_ where N = Length of the input string

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

int canBeConverted[102][102][3];

bool findIfPossible(ll lf, ll rt, char &ch, string &str){
  if(lf>rt or lf>=str.size())return false;
  if(lf==rt)return str[lf]==ch;
  if(canBeConverted[lf][rt][ch-'a']!=-1)return canBeConverted[lf][rt][ch-'a'];

  char ch1,ch2;
	if(ch=='a'){
	    ch1='b';
	    ch2='c';
	}else if(ch=='b'){
	    ch1='a';
	    ch2='c';
	}else{
	    ch1='a';
	    ch2='b';
	}
  bool ans=false;
  for(ll k=lf;k<rt;k++){
    ans=findIfPossible(lf,k,ch1,str) and findIfPossible(k+1,rt,ch2,str);
    if(!ans)ans=findIfPossible(lf,k,ch2,str) and findIfPossible(k+1,rt,ch1,str);
    if(ans)return canBeConverted[lf][rt][ch-'a']=ans;
  }
  return canBeConverted[lf][rt][ch-'a']=false;
}

int main()
{
  // freopen("input.txt","r",stdin);
  // freopen("output.txt","w",stdout);
  ll t=1;
  s(t);
  while(t--){
    string str;
    cin>>str;
    ll n=str.size();

    if(n==1){
      cout<<"1\n";
      continue;
    }

    ll dp[3][102]={0};
    memset(canBeConverted,-1,sizeof(canBeConverted));

    F(i,0,n-1){
      RF(j,i,0){
        for(char ch='a'; ch<='c'; ch++){
          bool isPossible = findIfPossible(j,i,ch,str);
          if(isPossible){
            if(j==0)dp[ch-'a'][i]=1;
            else if(dp[ch-'a'][j-1]!=0){
              if(dp[ch-'a'][i]>0)dp[ch-'a'][i]=min(dp[ch-'a'][i],dp[ch-'a'][j-1]+1);
              else dp[ch-'a'][i]=dp[ch-'a'][j-1]+1;
            }
          }
        }
      }
    }

    for(ll i=0;i<3;i++){
      if(dp[i][n-1]==0)dp[i][n-1]=INT_MAX;  
    }
    cout<<min(dp[0][n-1],min(dp[1][n-1],dp[2][n-1]))<<endl;
  }
}

```
