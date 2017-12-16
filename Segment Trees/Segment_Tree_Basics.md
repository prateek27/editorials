## Segment Trees Basics

Refer tutorial for explanation.

```C++
#include<iostream>
using namespace std;

int query(int *tree,int ss,int se,int qs,int qe,int index){
    ///Complete Overlap
    if( ss>=qs && se<= qe){
        return tree[index];
    }
    ///No Overlap
    if( qe < ss || qs > se){
        return INT_MAX;
    }
    ///Partial Overlap
    int mid = (ss+se)/2;
    int leftAns = query(tree,ss,mid,qs,qe,2*index);
    int rightAns = query(tree,mid+1,se,qs,qe,2*index+1);

    return min(leftAns,rightAns);
}
void updateNode(int *tree,int ss,int se,int i,int increment,int index){
    /// i is Out of Bounds
    if(i > se || i<ss){
        return;
    }
    ///Leaf Node
    if(ss==se){
        tree[index] += increment;
        return;
    }
    ///Call Both left and Right
    int mid = (ss+se)/2;
    updateNode(tree,ss,mid,i,increment,2*index);
    updateNode(tree,mid+1,se,i,increment,2*index+1);
    tree[index] = min(tree[2*index],tree[2*index+1]);
    return;
}

void updateRange(int *tree,int ss,int se,int l,int r,int inc,int index){
    ///Out Of Bounds
    if(l>se || r<ss){
        return;
    }
    /// Leaf Node
    if(ss==se){
        tree[index]+= inc;
        return;
    }
    ///Partial Overlap
    int mid = (ss+se)/2;
    updateRange(tree,ss,mid,l,r,inc,2*index);
    updateRange(tree,mid+1,se,l,r,inc,2*index+1);
    tree[index] = min(tree[2*index],tree[2*index+1]);
    return;
}


int buildTreeHelper(int a[],int s,int e,int *tree,int index){
    ///Base Case
    if(s==e){
            tree[index] = a[s];
            return a[s];
    }
    ///Rec Case
    int mid = (s+e)/2;
    int left = buildTreeHelper(a,s,mid,tree,2*index);
    int right = buildTreeHelper(a,mid+1,e,tree,2*index+1);

    tree[index] = min(left,right);
    return tree[index];
}

int* buildTree(int a[],int n){
    int *segmentTree = new int[4*n+1];
    buildTreeHelper(a,0,n-1,segmentTree,1);

    return segmentTree;
}

int main(){
    int a[] = {1,2,0,4,3,5};

    int t;
    cin>>t;

    int*tree = buildTree(a,6);
    //updateNode(tree,0,5,2,6,1);
    updateRange(tree,0,5,0,5,3,1);
    while(t--){
        int qs,qe;
        cin>>qs>>qe;

        cout<<"Query Ans "<< query(tree,0,5,qs,qe,1);
        cout<<endl;


    }


return 0;
}

```
