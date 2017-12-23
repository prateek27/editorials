## TOWER OF HANOI
##### Problem Link : [Tower of Hanoi](https://hack.codingblocks.com/contests/c/139/358)  

Basic Recursion Question. Just three steps are to be recursively executed :
- Move (n-1) discs from the source tower to the helper tower (where n is the number of discs in the source tower).
- Move the n<sup>th</sup> disc(last in source tower) to the destination tower.
- Move the (n-1) discs (currently in helper tower) to the destination tower.

_**Time Complexity:** O(2<sup>N</sup>)_ where N = Number of discs in the source tower.

Read below commented code for implementation details.
```C++

#include<iostream>
using namespace std;

int count=0;

// This function prints the movement of the discs and counts them simultaneously
void move(int x, int t1, int t2)
    {
    cout<<"Move "<<x<<"th disc from T"<<t1<<" to T"<<t2<<"\n";
    ++count;
    }

void hanoi(int f, int n, int t1, int t2, int t3)
    {
    if(n==1)
        move(f, t1, t2);
    else
        {
        hanoi(f, n-1, t1, t3, t2); // Step 1 as explained above.
        move(n, t1, t2); // Step 2
        hanoi(f, n-1, t3, t2, t1); //Step 3
        }
    }

int main()
    {
    int n;
    cin>>n;
    hanoi(1, n, 1, 2, 3);
    cout<<count;
    return 0;
    }

```
