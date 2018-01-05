## LIMITED BUDGET PARTY
##### Problem Link : [Limited Budget Party](https://hack.codingblocks.com/admin/preview/1239)  

We have to find the sub-array from the given array `Cost[]`, such that sum of all the elements of that sub-array gives the value equal to `X`. If such an array exists then we have to print `YES`,else `NO`.

We have to do it in linear time. We will be using two pointers method to solve this problem.
We will take two counters `c1` and `c2` and a variable `sum` which will store the sum of all the elements of cost between indexes `c1` and `c2`. 

Initially `c1` and `c2` is set to index zero. Later on, we will keep incrementing `c2` unless the sum is less than `X`. As soon as the total sum becomes equal to `X`, we will stop there and print `YES`.
If the sum becomes greater than `X`, we will start incrementing the previous counter `c1` and start decreasing the value of `Cost[c1]` from the total sum, and will keep doing it till the sum does not becomes less than `X`.

_**Time Complexity:** O(N)_ where `N` is the size of the `Cost` array.

Read below commented code for implementation details.
```C++
#include <string>
#include <cstdarg>
#include <utility>

#include <queue>
#include <stack>
#include <set>
#include <list>
#include <vector>
#include <queue>
#include <bitset>
#include <map>

#include <functional>
#include <sstream>
#include <algorithm>
#include <iostream>

#include <cstddef>
#include <cstring>
#include <cctype>
#include <cmath>
#include <cstdio>


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

int main()
{
  // freopen("input.txt","r",stdin);
  // freopen("output.txt","w",stdout);
  ll t;
  s(t);
  while(t--)
  {
    ll n,x;
    s2(n,x);
    ll *a=new ll[n+1];
    
    fori(i,n)
    {
      s(a[i]);
    }
    bool flag=0;

    // left counter
    ll c1=0;

    // right counter
    ll c2=0;

    // sum contains the sum of all the array elements between left and right counter index
    ll sum=0;
    
    for(ll i=0;i<n;i++)
    {
      c2=i;
      // New element added at the index c2 to the segment
      sum+=a[c2];
      if(sum==x)
      {
        flag=1;break;
      }
      
      // Now remove elements from the back if sum becomes greater than X
      if(sum>x)
      {
        while(sum>=x && c1<=c2)
        {
        
        // All elements removed. Reset everything
          if(c1==c2)
          {
            sum=0;
            c1++;
            c2++;
            break;
          }
          
          // Remove the last element from the segment
          sum -= a[c1];
          // Increment the last pointer
          c1++ ;
        
          if(sum==x)
          {
            flag=1;break;
          }
        }
        if(flag==1)break;
      }
      
    }
    if(sum<x || flag==0)cout<<"NO\n";
    else cout<<"YES\n";
  }
  return 0;
}
```
