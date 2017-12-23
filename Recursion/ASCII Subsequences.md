## ASCII SUBSEQUENCES
##### Problem Link : [ASCII Subsequences](https://hack.codingblocks.com/contests/c/139/355)  

This question is same as the "Subsequences" question just instead of two function calls, three function calls will be made :
- One with the same string.
- One with a letter added.
- One with the ascii code of the letter added to the string.

_**Time Complexity:** O(3<sup>N</sup>)_ where N = String Length.

Read below commented code for implementation details.
```C++

#include<iostream>
#include<string>
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
    // The 3 function calls in the same order as explained above.
    subseq(s, i+1);
    subseq(s + str[i], i+1);
    subseq(s + to_string((int)str[i]), i+1); // str[i] is type casted to integer which gives the ASCII value which is then converted to string for concatenation.
    }

int main()
    {
    cin>>str;
    cout<<pow(3, str.size())<<"\n";
    subseq("", 0);
    return 0;
    }

```
