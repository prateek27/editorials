## HOODIES AT CODING BLOCKS
##### Problem Link : [Hoodies At Coding Blocks](https://hack.codingblocks.com/admin/preview/1244)  

Just store the students of each course in a different queue. At any moment, the actual queue will contain 4 divisions, each for a particular course and that can be simulated as different queues. Whenever a new student joins an existing student from his course, he will always get to put his name right after his friend in front, hence the same timestamp as his friend's, but just behind him in the queue. Therefore we maintain a timestamp for each course. However if a student does not find any existing student from the same course, he will join the queue at last, hence the largest timestamp will be allocated to that course's queue.

Finally just find the queue with the minimum timestamp , provided there is a student in the queue and then pop that student from his queue.If the queue becomes empty, simply reset the timestamp of that queue to indicate no student.

_**Time Complexity:** O(Q)_ where Q = Number of queries

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

int main()
{
  // freopen("input.txt","r",stdin);
  // freopen("output.txt","w",stdout);
  ll q;
  s(q);
  // tot[0] will contain the overall timestamp
  // tot[i] will contain the timestamp for the ith course for i>0
  ll tot[5];
  memset(tot,-1,sizeof(tot));
  tot[0]=0;

  // qu[i] will contain the students of the ith course in the order of their entry 
  queue<ll>qu[5];

  while(q--){
    char ch;
    cin>>ch;
    if(ch=='E'){
      ll x,r;
      s2(x,r);
      // Increase the overall timestamp
      tot[0]++;
      // Push the child in the xth course's queue
      qu[x].push(r);

      // If the student is the first student in the course, set the timestamp of the xth course to that of the overall time stamp
      if(tot[x]==-1)
        tot[x]=tot[0];
    }
    else{
      ll id=0;
      ll mini=INT_MAX;
      // Simply compute the course with the minimum timestamp 
      for(ll i=1;i<=4;i++)
        if(tot[i]!=-1 and tot[i]<mini){ mini=tot[i]; id=i; }
      
      cout<<id<<" "<<qu[id].front()<<endl;

      // Ppp the child from the course with the least timestamp
      qu[id].pop();
      // If that was the only child in the course, reset the timestamp of that course to -1.
      if(qu[id].empty())tot[id]=-1;
    }
  }
}
```
