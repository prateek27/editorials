## SMART KEYPAD - ADVANCED
##### Problem Link : [Smart Keypad - Advanced](https://hack.codingblocks.com/contests/c/139/97)

All the outputs from the question "Smart Keypad - I" are stored in a vector v.

For each name in searchIn[], for each generated string from vector v, if string is in name, the name is printed.

_**Time Complexity:** O(l<sup>N</sup> * M)_ where N = Number of strings in table[], l = Length of string in table, and M = Number of strings in searchIn[].

Read below commented code for implementation details.
```C++

#include<iostream>
#include<vector>
using namespace std;

string str, table[] = { " ", ".+@$", "abc", "def", "ghi", "jkl" , "mno", "pqrs" , "tuv", "wxyz" };
string searchIn [] = {
            "prateek", "sneha", "deepak", "arnav", "shikha", "palak",
            "utkarsh", "divyam", "vidhi", "sparsh", "akku"
    };
vector<string> v;
void smart(string s, int x)
    {
    if(!str[x])
        {
        // cout<<s<<"\n";
        v.push_back(s);
        return;
        }
    int i=0;
    while(table[(str[x]-'0')][i])
        {
        smart(s+table[(str[x]-'0')][i], x+1); // same as Smart Keypad - I
        ++i;
        }
    }

int main()
    {
    cin>>str;
    smart("", 0);
    for(string s:searchIn) // For every name in searchIn
        {
        for(string ye:v) // For every generated string in vector v
            {
            if(s.find(ye)!=string::npos) // If the name contains the generated string
                cout<<s<<"\n";
            }
        }
    return 0;
    }

```
