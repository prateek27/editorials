## N-Queen using Backtracking and Bitmasks
Refer tutorial for explanation.


```c
#include<iostream>
#include<cmath>
using namespace std;

int n;
int ans =0, DONE;
///rowmask denote which positions(colms) in all rows are filled
/// ld, rd denotes unsafe positions along diagonals for the current row
/// done is vector of all 11111 ( n times 1 )
/// safe denotes the cols which are safe in the current row

/// Most optimisized n queen code !



void solve(int rowMask,int ld,int rd,int i,int board[20][20]){

    if(rowMask==DONE){ ans++; 
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                cout<<board[i][j]<<" ";
            }
            cout<<endl;
        }

        return; 
    }

    int safe = DONE&(~(rowMask|ld|rd));
    while(safe){
        //p denotes the column
        int p = safe &(-safe);

        //Place the queen
        int c = n-log2(p)-1; 
        board[i][c] = 1;
        safe = safe - p;
        solve(rowMask|p, (ld|p)<<1, (rd|p)>>1,i+1,board);
        //Remove the queen - Backtrack
        board[i][c] = 0;
    }
}


int main()
{
    cin>>n;
    int board[20][20] = {0};
    DONE = ((1<<n) - 1);
    solve(0,0,0,0,board);
    cout<<ans<<endl;

}

```