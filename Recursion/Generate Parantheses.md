## GENERATE PARANTHESES
##### Problem Link : [Generate Parantheses](https://hack.codingblocks.com/contests/c/139/910)

A recursive function is called with two parameters, namely open and close which store the number of open and close parantheses pending respectively for a particular call. 2 calls are made from this function :
- A call with concatenating an open parantheses.
- If any close are pending, one is concatenated.

_**Time Complexity:** O(2<sup>N</sup>)_ where N = Number of parantheses to be used.

Read below commented code for implementation details.
```C++

#include<iostream>
using namespace std;
void para(string s, int open, int close)
    {
    if(open==0) // All the open parantheses have been concatenated.
        {
        cout<<s;
        while(close--) // All the close parantheses pending are concatenated.
            cout<<")";
        cout<<"\n";
        return;
        }
    if(close)
        para(s+")", open, close-1);
    para(s+"(", open-1, close+1);
    }
int main()
    {
    int n;
    cin>>n;
    para("", n, 0); // Function is called with two parameters, open and close respectively.
    }

```
