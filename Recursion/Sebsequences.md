## SUBSEQUENCES
##### Problem Link : [Subsequences](https://hack.codingblocks.com/contests/c/139/353)  

Number of Subsequences will be simply equal to 2<sup>N</sup> because we for every letter there are two possibilities: wheather it'll be present in the subsequence or it'll be absent.

A recursive function will be passed with an empty string in which (for every letter of the actual string) two fuction calls will be made : One with the same string another with a letter added to the same string.

_**Time Complexity:** O(2<sup>N</sup>)_ where N = String Length.

Read below commented code for implementation details.
```C++

#include<iostream>
#include<cmath>
using namespace std;

string str;

void subseq(string s, int i)
    {
    if(!str[i])
        {
        cout<<s<<" ";
        return;
        }
    subseq(s, i+1); // A function call without adding a letter.
    subseq(s + str[i], i+1); // A function call with adding a letter.
    }

int main()
    {
    cin>>str;
    cout<<pow(2, str.size())<<"\n";
    subseq("", 0);
    return 0;
    }

```
