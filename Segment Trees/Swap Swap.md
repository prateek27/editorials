## SWAP SWAP
##### Problem Link : [Swap Swap](https://hack.codingblocks.com/contests/c/1001/1228)  

A greedy strategy works well for this problem. We start by bringing the largest value we are able to on the first position, then we bring the largest possible value on the second position, and so on. 
More exactly, we start with `K` swaps available. If `K ≥ N−1`, we can bring any element on the first position, so we should choose the maximum value. If this value is not unique, we should take the leftmost occurrence, so we have more moves available for the next steps.
Otherwise, if `K < N−1`, we should first find the maximum value from the next `K+1` elements, and then move its first occurrence on the first position. After choosing the first value, we update K and proceed to the next positions. 

A straight forward implementation of this solution runs in `O(N`<sup>2</sup>`)`. But we can optimise it by using data structures. We need to mark the elements we move, find the `K`<sup>th</sup> value we didn't move yet, and find the maximum value in a given interval. All these operations can be implemented using segment trees, for a total complexity of `O(NlogN)`.

_**Time Complexity:** O(N x LogN)_ where N is the size of the array.

Read below commented code for implementation details.
```C++
#include<bits/stdc++.h>
#include<unordered_set>
using namespace std;
#define fio ios_base::sync_with_stdio(false)
 
#define ll long long int

#define s(x) scanf("%lld",&x)
#define s2(x,y) s(x)+s(y)
#define s3(x,y,z) s(x)+s(y)+s(z)
 
#define p(x) printf("%lld\n",x)
#define p2(x,y) p(x)+p(y)
#define p3(x,y,z) p(x)+p(y)+p(z)
#define F(i,a,b) for(ll i = (ll)(a); i <= (ll)(b); i++)
#define RF(i,a,b) for(ll i = (ll)(a); i >= (ll)(b); i--)
 
#define ff first
#define ss second
#define mp(x,y) make_pair(x,y)
#define pll pair<ll,ll>
#define pb push_back

ll inf = 1e18;
ll mod = 1e9 + 7 ;
ll gcd(ll a , ll b){return b==0?a:gcd(b,a%b);}

/****************************************************************************/

struct node{
  ll untouched; // =0 for those elements which have not been considered for lexicographic arrangement
  ll maxElem; // maximum untouched number in the range represented by that node
  ll maxElemPos; // index of the maxElem
  ll maxElemSwaps; // number of swaps needed to bring the maxElem in the beginning of the range represented by that node
};

node empty={0,-1,-1,0}; // empty node containing no element
node segTree[500000];
vector<ll>ar; // array to store the input numbers
ll n,k;

node merge(node leftChild, node rightChild){
  node ret;
  ret.untouched=leftChild.untouched+rightChild.untouched;
  ret.maxElem=max(leftChild.maxElem,rightChild.maxElem);
  ret.maxElemPos = 
    (leftChild.maxElem>=rightChild.maxElem)?leftChild.maxElemPos:rightChild.maxElemPos;

  // Note that if right child has the maximum element, then the number of swaps needed will not
  // only comprise of the maxElemSwaps of the right child but also the untouched elements of the
  // left child because on merging the left and right child, the maximum element has to be brought
  // not only to the beginning of the right child range, but to the beginning of the entire range
  // and since the untouched elements have to be swapped as is, their number is added.
  ret.maxElemSwaps = 
     (leftChild.maxElem>=rightChild.maxElem)?leftChild.maxElemSwaps:leftChild.untouched+rightChild.maxElemSwaps;
  return ret;
}

void buildTree(ll treeNode,ll start,ll end){
  if(start>end)return;
  if(start==end){
    segTree[treeNode].untouched = 1; // initially, all numbers are untouched
    segTree[treeNode].maxElem = ar[start];
    segTree[treeNode].maxElemPos = start;
    segTree[treeNode].maxElemSwaps = 0;
    return;
  }

  ll mid=start+end;
  mid>>=1;
  buildTree(2*treeNode+1,start,mid);
  buildTree(2*treeNode+2,mid+1,end);
  segTree[treeNode]=merge(segTree[2*treeNode+1],segTree[2*treeNode+2]);
}

node getMax(ll treeNode,ll start,ll end,ll swaps){
  if(start>end or swaps<0)return empty;
  if(segTree[treeNode].maxElemSwaps<=swaps)return segTree[treeNode];

  ll mid=start+end;
  mid>>=1;
  node leftChild = getMax(2*treeNode+1,start,mid,swaps);
  node rightChild = getMax(2*treeNode+2,mid+1,end,swaps-segTree[2*treeNode+1].untouched);
  return merge(leftChild,rightChild);
}

void touchInd(ll treeNode,ll start,ll end,ll ind){
  if(start>end or ind<start or ind>end)return ;
  if(start==end){
    // Note that the empty node has untouched value to be zero i.e. the element is now 
    // considered to be takedn and will not be considered for further arrangement.
    segTree[treeNode] = empty; 
    return;
  }

  ll mid=start+end;
  mid>>=1;
  touchInd(treeNode*2+1,start,mid,ind);
  touchInd(treeNode*2+2,mid+1,end,ind);
  segTree[treeNode] = merge(segTree[2*treeNode+1],segTree[2*treeNode+2]);
  return;
}

ll query(ll &swaps){
  node maxNode = getMax(0,0,n-1,swaps);
  swaps -= maxNode.maxElemSwaps; // Since we have used those swaps needed to get the maximum element to the beginning,we subtract it from the remaining swaps as we have only a fixed number of swaps available
  ll ret = maxNode.maxElem;
  touchInd(0,0,n-1,maxNode.maxElemPos); // Once we have used the maxElem, it is no longer untouched, hence we have to touch it so it can no longer be considered for further arrangement
  return ret;
}

int main()
{
  // freopen("input.txt","r",stdin);
   // freopen("output.txt","w",stdout);
  ll t=1;
  // s(t);
  while(t--){
    
    s2(n,k);
    ar.resize(n);
    F(i,0,n-1)cin>>ar[i];
    buildTree(0,0,n-1);
    F(i,1,n)cout<<query(k)<<" ";
    cout<<endl;
  }
  
  return 0;
 }
```
