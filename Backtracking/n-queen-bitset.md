## N-Queen using Bitsets
Refer Tutorial for explanation.

```c
#include<iostream>
#include<bitset>
#include<ctime>
using namespace std;

bitset<30> col,d1,d2;

void solve(int r,int n,int &ans){
    if(r==n){ ans++; return;}

    for(int c=0;c<n;c++){
        if( !col[c] && !d1[r-c+n-1] && !d2[r+c]){
            col[c] = d1[r-c+n-1] = d2[r+c] = 1;
            solve(r+1,n,ans);
            col[c] = d1[r-c+n-1] = d2[r+c] = 0;
        }
    }
}

int main(){
    int n;
    cin>>n;

    clock_t st = clock();

    int ans = 0;
    solve(0,n,ans);
    cout<<ans<<endl;
    cout<< clock()  - st<<endl;
return 0;
}
```
