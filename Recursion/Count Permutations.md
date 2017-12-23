## COUNT PERMUTATIONS
##### Problem Link : [Count Permutations](https://hack.codingblocks.com/contests/c/139/401)

For every element in the string, swap it with itself and all the other elements in the string. And store all these strings formed in a set to avoid redundancies. Eventually use a for-each loop to print the set elements. Best part is that the set itself stores all the strings lexicographically.

_**Time Complexity:** O(N!)_ where N = String Length.

Read below commented code for implementation details.
```C++

#include<iostream>
#include<cstring>
#include<set>
#include<algorithm>
using namespace std;

set<string> s;

void permutation(char *str, int x)
    {
    if(strlen(str)-1 == x) // This implies that the swaps have been completed for this string.
        s.insert(str);
    else
        {
        for(int i=x; i<strlen(str); i++)
            {
            swap(str[x], str[i]);
            permutation(str, x+1);
            swap(str[x], str[i]); // Swap back to bring the letters in their place before the next iteration.
            }
        }
    }

int main()
    {
    char str[10];
    cin>>str;
    permutation(str, 0);
    for(string x:s) // A for each loop to print all elements in the set.
        cout<<x<<"\n";
    return 0;
    }

```
