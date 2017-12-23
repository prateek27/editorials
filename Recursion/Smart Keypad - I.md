## SMART KEYPAD - I
##### Problem Link : [Smart Keypad - I](https://hack.codingblocks.com/contests/c/139/96)  

Basically, we need to take a letter from those strings in table whose indices are given as input.

We start with and empty string and recursively add one letter from each string whose indices are given in the input string.

_**Time Complexity:** O(l<sup>N</sup>)_ where N = Number of strings in table[], and l = Average length of string in table.

Read below commented code for implementation details.
```C++

#include<iostream>
using namespace std;

string str, table[] = { " ", ".+@$", "abc", "def", "ghi", "jkl" , "mno", "pqrs" , "tuv", "wxyz" };

void smart(string s, int x)
    {
    if(!str[x])
        {
        cout<<s<<"\n";
        return;
        }
    int i=0;
    while(table[(str[x]-'0')][i]) // This loop calls smart() for each letter in the string at the current index.
        {
        smart(s+table[(str[x]-'0')][i], x+1); // For every, letter concatenated in the string at one index, same function is called for concatenation from the string at the next index.
        ++i;
        }
    }

int main()
    {
    cin>>str;
    smart("", 0);
    return 0;
    }

```
