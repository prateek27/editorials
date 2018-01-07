## SITTING ARRANGEMENT AT CODING BLOCKS
##### Problem Link : [Sitting Arrangement At Coding Blocks](https://hack.codingblocks.com/admin/preview/1246)  

Besides all these `K` newly added students, the subset can also contain some of the initial ones. So, in order to maximize the size of the subset, we need to maximize the number of initial students in the solution.

Suppose we fix the largest index `r` of an initial student in the subset. We should find the smallest possible index `l` of the leftmost student. In order to find out if a pair `(l, r)` is valid, we should check if the number of students that need to be placed between these two initial ones is not greater than `K`. If we denote by x<sub>i</sub> the coordinate of the `i`<sup>th</sup> student in the input, then we need to check if **(x<sub>r</sub> - x<sub>l</sub>) - (r - l ) ≤K**.

A brute force consists of checking all the possible `(l,r)` pairs. This solution has a complexity of **O(N<sup>2</sup>)**

In order to optimize the brute force, notice that if `r` increases, then `l` also increases (or at least stays the same). This means we can employ a two pointer technique. Initially we set both `l` and `r` to point to the first student. Then we start incrementing `r`. As we do this, at each step we increment `l` as long as we have to - we can perform the same check as in the brute force above.

Throughout the algorithm, both the `l` and `r` are never decremented. Each of them will perform a single array pass, so the complexity is `O(N)`.


_**Time Complexity:** O(N)_ where N is the size of the array

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


ll inf = 1e6+1;
ll mod = 1e9 + 7 ;
ll gcd(ll a , ll b){return b==0?a:gcd(b,a%b);}

int main()
{
    // freopen("input.txt","r",stdin);
     // freopen("output.txt","w",stdout);
    ll t=1;
    //s(t);
    while(t--){
        ll n,k;
        s2(n,k);
        vector<ll>ar(n,0);
        for(ll i=0;i<n;i++)cin>>ar[i];
          // Keep two pointers
        ll lf=0,rt=0;
      // Best stores the maximum subset of consecutive integers found so far
        ll best=k;
        do{
          // while the gap between the coordinates represented by ar[lf] and ar[rt] is greater than k,increment the left pointer
            while(ar[rt]-ar[lf]-(rt-lf) > k)lf++;
            best=max(best,rt-lf+1+k);
            // Increment the right pointer anyway
            rt++;
        }while(rt<n);
        cout<<best<<endl;
    }
}

```
