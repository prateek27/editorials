## Select, Copy And Paste
#### Problem Link : [Select, Copy And Paste](https://hack.codingblocks.com/contests/c/1001/920)

For N < 7, the output is N itself.
Ctrl V can be used multiple times to print current buffer .

The idea is to compute the optimal string length for N keystrokes by using a simple insight. The sequence of N keystrokes which produces an optimal string length will end with a suffix of Ctrl-A, a Ctrl-C, followed by only Ctrl-V's (For N > 6).

The idea is to compute the optimal string length for N keystrokes by using a simple insight. The sequence of N keystrokes which produces an optimal string length will end with a suffix of Ctrl-A, a Ctrl-C, followed by only Ctrl-V's (For N > 6).

_**Time Complexity**: O(N<sup>2</sup>)_

Below is the dynamic programming solution for the same.

```C++
#include<bits/stdc++.h>
#define ll long long
using namespace std;

// this function returns the optimal length string for N keystrokes
int findoptimal(ll N){	
    // The optimal string length is N when N is smaller than 7
    if (N <= 6)
        return N;
 
    // An array to store result of subproblems
    ll screen[N+10];
 
    ll b;  // To pick a breakpoint
 
    // Initializing the optimal lengths array for uptil 6 input
    // strokes.
    ll n;
    for (n=1; n<=6; n++)
        screen[n] = n;
 
    // Solve all subproblems in bottom manner
    for (n=7; n<=N; n++)
    {
        // Initialize length of optimal string for n keystrokes
        screen[n] = 0LL;
 
        // For any keystroke n, we need to loop from n-3 keystrokes
        // back to 1 keystroke to find a breakpoint 'b' after which we
        // will have ctrl-a, ctrl-c and then only ctrl-v all the way.
        for (b=n-3; b>=1; b--)
        {
            // if the breakpoint is at b'th keystroke then
            // the optimal string would have length
            // (n-b-1)*screen[b-1];
            screen[n] = max(screen[n], (n-b-1)*screen[b]);
        }
    }
 
    return screen[N];
}

int main(){
//	freopen("input3.txt","r",stdin);
//	freopen("output3.txt","w",stdout);

    ll N;
    cin>>N;
	cout<<findoptimal(N)<<"\n";
    return 0;   
}
```