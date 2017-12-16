## Problem Name : Matrix Queries-I

### Topic : Segment Trees

#### Difficulty : Medium

### EXPLANATION

Since the matrices may not be invertible, we cannot store prefix products for range queries. Thus we need a data structure. We can use segment tree for range queries. Each input matrix is stored leaf nodes of the tree. Combining two nodes can be done like :-<br>

$$Val_{node}$$ = $$Val_{node->left}*Val_{node->right}$$ mod $$r$$. Where $$Val_{node}$$ is a 2*2 matrix.

#### Time Complexity : O($$sqrt(K)$$)(Approx).

```c++
#include <bits/stdc++.h>
#define nn 100100
#define ll long long int
#define inf 1000000000000000000ll
 
using namespace std;

int t[nn<<2][2][2],a[nn][2][2];
int n,r,q;

void multiply(int a[2][2],int b[2][2],int r[2][2]) // r=a*b
{
    r[0][0]=(a[0][0]*b[0][0]+a[0][1]*b[1][0])%r;
    r[0][1]=(a[0][0]*b[0][1]+a[0][1]*b[1][1])%r;
    r[1][0]=(a[1][0]*b[0][0]+a[1][1]*b[1][0])%r;
    r[1][1]=(a[1][0]*b[0][1]+a[1][1]*b[1][1])%r;
}

void build(int node,int st,int en)
{
    if(st==en)
    {
        for(int i=0;i<2;i++)
            for(int j=0;j<2;j++)
                t[node][i][j]=a[st][i][j];
        return;
    }
    int mid=st+en>>1;
    build(node*2+1,st,mid);
    build(node*2+2,mid+1,en);
    multiply(t[node*2+1],t[node*2+2],t[node]);
}

void query(int node,int st,int en,int l,int r,int res[2][2])
{
    if(st>en || l>en || st>r)
        return;
    if(l<=st && en<=r)
    {
        for(int i=0;i<2;i++)
            for(int j=0;j<2;j++)
                res[i][j]=t[node][i][j];
        return;
    }
    int mid=st+en>>1;
    int r1[2][2]={{1,0},{0,1}},r2[2][2]={{1,0},{0,1}};
    query(node*2+1,st,mid,l,r,r1);
    query(node*2+2,mid+1,en,l,r,r2);
    multiply(r1,r2,res);
}

int main()
{
    ios_base::sync_with_stdio(0);
    cin>>r>>n>>q;
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<2;j++)
        {
            for(int k=0;k<2;k++)
            {
                cin>>a[i][j][k];
            }
        }
    }
    build(0,0,n-1); //initializing segment tree
    while(q--)
    {
        int u,v;
        cin>>u>>v;
        u--,v--;
        int res[2][2]={{1,0},{0,1}};
        query(0,0,n-1,u,v,res); //querying in segment tree
        for(int j=0;j<2;j++)
        {
            for(int k=0;k<2;k++)
            {
                cout<<res[j][k]<<' ';
            }
            cout<<endl;
        }
        cout<<endl;
    }
    return 0;
}
```