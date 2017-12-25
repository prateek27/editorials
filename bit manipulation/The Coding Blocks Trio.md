## THE CODING BLOCKS TRIO
##### Problem Link : [The Coding Blocks Trio](https://hack.codingblocks.com/contests/c/1001/1206)  

Here, we need to consider all the subsets of size **3** of the given list of strings, and check whether all the strings in each subset have at least one vowel common. We can follow the top down dynamic approach for the same. First we will use bits to manage count of vowel in each string.

We will assign particular bit position for each vowel, say for a = 1<sup>st</sup> bit, e = 2<sup>nd</sup> bit and so on.. and for each string, we will consider **5** bits, and will set them if a string a vowel.

```C++
int vount_vowels =0;
say for string "ae":
count_vowels = count_vowels | (1<<1) for a.
count_vowels = count_vowels | (1<<2) for e.
```

and after that we will apply dynamic programming approach with states **id, mask, count**:

>`id` = represents the index of the string considered in the subset

>`mask` = represents the number of vowels common in the considered strings.

>`count` = number of strings considered earlier for the subset.

For each string, we can consider it in the current subset or can leave and can move on other string. We will store the result calculated up to a for each state, to avoid the re calculation.

_**Time Complexity:** O( N x 2^V-1 x G)_ where N = Maximum Number Of students,V = Number of Vowels, G = Group Size

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

// Method that returns the bit position to be set if ch is a particular vowel
ll getPos(char ch){
  switch(ch){
    case 'a': return 0;
    case 'e': return 1;
    case 'i': return 2;
    case 'o': return 3;
    case 'u': return 4;
    default: return -1;
  }
}

// vs is a vector of strings to contain the input string
vector<string> vs;
// bits is a vector of integer where bits[i] will store the integer in which only those
// bits will be set for which the corresponding vowel is present in vs[i].
vector<int> bits;
// Number of names to be input for a given test case
ll n;

// dp[index][mask][done] = Number of ways to choose 'done' number of students from index i 
// to index n-1 with the chosen students satisfying the 'mask'. i.e. the mask will have only 
// those bits set for which each of the chosen student so far have the same vowel in all 
// of their names
ll dp[10005][40][5];

ll findWays(ll ind, ll mask, ll done){
  if(ind==n){
    // if mask is zero, it means the chosen students have no vowel in common and
    // done must be 3 since we need to make a group of 3 students
    if(mask>0 and done==3) return 1;
    return 0;
  } 
  if(dp[ind][mask][done]!=-1)return dp[ind][mask][done];
  ll ans = 0;
  // Choice 1: Ignore the student at index id to be included in the subset
  ans = findWays(ind+1,mask,done);
  // Choice 2: Choose this student if the group formed so far has less than 3 students
  if(done<=2) ans += findWays(ind+1,(mask & bits[ind]),done+1);
  return dp[ind][mask][done]=ans;
}

int main()
{
  // freopen("input.txt","r",stdin);
   // freopen("output.txt","w",stdout);
  ll t=1;
  s(t);
  while(t--){
    vs.clear();
    bits.clear();
    memset(dp,-1,sizeof(dp));
    s(n);

    // vs.resize(n);
    // bits.resize(n,0ll);

    vs = vector<string>(n);
    bits = vector<int>(n,0);

    F(i,0,n-1){
      cin>>vs[i];
      ll sz = vs[i].size();
      F(j,0,sz-1){
        ll pos = getPos(vs[i][j]);
        if(pos>=0)bits[i]=bits[i]|(1<<pos);
      }
    }
    ll ans = findWays(0,31,0);
    p(ans);
  }
  return 0;
 }
```
