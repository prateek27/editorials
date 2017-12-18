## CODES OF THE STRING
##### Problem Link : [Codes of the String](https://hack.codingblocks.com/contests/c/139/346)

A recursive function is called with an empty string which is (per function call) either added with the ascii alphabet of single digit or ascii alphabet of the next two digits (if they are less than 27).

_**Time Complexity:** O(2<sup>N</sup>)_ where N = Number of digits in the given string.

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
        if(s.size() == str.size()) // this is just to print the first possibility, where all the single digits are considered.
            cout<<s;
        else
            cout<<", "<<s;
        return;
        }

    subseq(s + (char)((str[i]-'0')+96), i+1); // First Call, in which the string is concatenated with the ascii alphabet of the single digit.
    if(str[i+1]) // if two digits further exist in this string.
        {
        if(stoi(str.substr(i,2)) <= 26) // if the number formed by the two digits is <= 26
            subseq(s + (char)(stoi(str.substr(i,2))+96), i+2); // string concatenated with the respective alphabet.
        }
    }

int main()
    {
    cin>>str;
    cout<<"[";
    subseq("", 0);
    cout<<"]";
    return 0;
    }

```
