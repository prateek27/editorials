## Magic Squares
#### Problem Link : [Magic Squares](https://hack.codingblocks.com/contests/c/1001/711)

The catch in the problem is that for 2 pairs of indexes **(i1,j1)** and **(i2,j2)**, all the elements enclosed by the square defined by **x=i1, x=i2, y=j1, y=j2** are greater than or equal to **mat[i1][j1]** and smaller than or equal to **mat[i2][j2]**.
Now, one can search in every row for the first element greater than or equal to **l** using binary search. Consider the index of that element be **(x,y)**. Linear search can be done on principal diagonal starting from **(x,y)** till we find a number greater than **r** and the length of the diagonal will be equal to the maximum length of side of the square for that row. This process is repeated for every row and maximum of all results for each row is the answer.
The **time complexity** becomes _O(q*n*log(m)*min(n,m))_.
But if we start the linear search from the already found maximum value, the time complexity reduces by factor of _min(n,m)_.

_**Time Complexity: **  O((q*n*log(m))+min(n,m))_

Following is the C++ code for the above approach.

```C++
#include<algorithm>
#include<cstdio>
 
using namespace std;
 
int N, M, Q, L, U;
 
int main() {
//	freopen("output.txt","r",stdin);
//	freopen("input.txt","w",stdout);
    
        scanf("%d %d", &N, &M);
        int plot[N][M];
        for(int i = 0; i < N; i++)
            for(int j = 0; j < M; j++)
                scanf("%d", &plot[i][j]);
        scanf("%d", &Q);
        for(int i = 0; i < Q; i++) {
            scanf("%d %d", &L, &U);
            int curr_max = 0;
            for(int j = 0; j < N; j++) {
                int* lb = lower_bound(plot[j], plot[j] + M, L);//gives the pointer to first element in the row which is greater than or equal to L
                int min_ind = lb - plot[j];
                for(int k = curr_max; k < N; k++) {//increases the diagonal by one and check if that element is samller than or equal to r
                    if(j + k >= N || min_ind + k >= M || plot[j + k][min_ind + k] > U) break;
                    if(k + 1 > curr_max) curr_max = k + 1;
                }
                //we iterate starting from curr_max time complexity is O(n*log(m)+min(n,m))
            }
            printf("%d\n", curr_max);
        }
    return 0;
}
```
