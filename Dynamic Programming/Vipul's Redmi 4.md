## VIPUL'S REDMI 4
##### Problem Link : [Vipul's Redmi 4](https://hack.codingblocks.com/admin/preview/1238)  

In the allotted time, we can see some photos from the beginning and some from the end since we can not skip any photo and move ahead. There are 4 cases:

1. We always go right.
2. We always go left.
3. We initially go right, then reverse direction, go through all visited photos and continue going left.
4. We initially go left, then reverse direction, go through all visited photos and continue going right.

First two cases can be easily coded. Third and fourth cases can be done with the method of two pointers. Note that if we see one more picture to the right, we spend more time on the right side and the number of photos seen to the left will decrease.
Also note that we should finish watching pics on one side before moving to the other side as it would simply be a waste of time if we keep rolling from one side to another, since we had to maximise the number of pics.

_**Time Complexity:** O(N)_ where N = Number of pics in Vipul's phone

Read below commented code for implementation details.
```C++
#include<bits/stdc++.h>

using namespace std;

#define ll long long int

#define s(x) scanf("%lld",&x)
#define s2(x,y) s(x)+s(y)
#define s3(x,y,z) s(x)+s(y)+s(z)

#define p(x) printf("%lld\n",x)
#define p2(x,y) p(x)+p(y)
#define p3(x,y,z) p(x)+p(y)+p(z)
#define fori(i,max) for(ll i=0;i<(ll)(max) ;(i)++)
#define for2i(i,min,max) for(ll i=min;i<(ll)(max) ;(i)++)

#define ff first
#define ss second
#define mp(x,y) make_pair(x,y)
#define plb pair<ll,bool>
#define pb push_back
#define mx 500090

ll rt[mx+5];
ll lt[mx+5];

int main()
{
  // freopen("input.txt","r",stdin);
  // freopen("output.txt","w",stdout);
  ll n,a,b,t;
  cin>>n>>a>>b>>t;
  string st;
  cin>>st;
  st=' '+st;
  
  // rt[i] contains total time needed to move from pic 1 to pic i (including the swiping time(a), observing time(1),rotating time(b))
  for(ll i=2;i<=n;i++)
    rt[i]=rt[i-1]+a+1+b*(st[i]=='w');
    
    // lt[i] contains total time needed to move from pic n+1 (pic 1) to pic i (including the swiping time(a), observing time(1),rotating time(b))
  for(ll i=n;i>=2;i--)
    lt[i]=lt[i+1]+a+1+b*(st[i]=='w');
    
  // subtract the time needed to observe and rotate(if needed) the first pic (pic 1) currently opened
  t -= 1+b*(st[1]=='w');  
    
  ll ans=0;

  // Go till pic i to the right from first pic and then move back till pic j to the left
  for(ll i=1,j=2;i<=n;i++){
    if(rt[i]>t)break;
    
    // Here we are trying to find the upper bound for j till which we can see the pics in the remanining time
    while(j<=n && rt[i]+lt[j]+(i-1)*a > t)j++ ; // Note the factor (i-1)*a constitutes the time needed to swipe over the already seen pics since we are swiping back
    
    ans=max(ans,i+n-j+1);
   }

   // Go till pic i to the left from first pic and then move back till pic j to the right
   for(ll i=n+1,j=n;i>=2;i--)
   {
    if(lt[i]>t)break;
    
    // Here we are trying to find the upper bound for j till which we can see the pics in the remanining time
    while(j>=1 && lt[i]+rt[j]+(n-i+1)*a > t)j-- ;
     
    ans=max(ans,n-i+1+j); 
   }
   cout<<min(ans,n)<<endl;
}

```
