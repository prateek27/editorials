## TREE DECOMPOSITION
##### Problem Link : [Tree Decomposition](https://hack.codingblocks.com/contests/c/1001/1100)  

Before reading this code, please refer to these two links:
1. [Anudeep's Blog on HLD](https://blog.anudeep2011.com/heavy-light-decomposition/)

2. [Finding LCA using segment trees](https://www.geeksforgeeks.org/find-lca-in-binary-tree-using-rmq/)

as these are used to implement this code. Without reading these, the code can not be understood.

_**Time Complexity:** O((log N)<sup>2</sup>)_ where N is the number of nodes in the tree

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

vector<vector<pll>>adj;
vector<pair<pll,ll>>edgeList;
vector<ll>subtreeSize;
vector<ll>parent;
vector<ll>chainHead;
vector<ll>nodesChain;
vector<ll>baseArray;
vector<ll>nodesPosInBaseArray;
vector<ll>segTree;

vector<ll> eulerArray;
vector<ll> level;
vector<ll> firstOccurence;
vector<pll> segTreeForLCA;

ll curChain;
ll basePtr;
const ll logN = 1000;

// Finds the parent and the subtree size of every node to be used in HLD
void dfs(ll node,ll par=-1){
  parent[node]=par;
  subtreeSize[node]=1ll;

  for(ll i=0;i<adj[node].size();i++){
    ll child = adj[node][i].first;
    if(child==par)continue;
    dfs(child,node);
    subtreeSize[node]+=subtreeSize[child];
  }
}

void HLD(ll node,ll par=-1,ll cost=-1){
    if(chainHead[curChain]==-1)chainHead[curChain]=node;
    nodesChain[node]=curChain;
    nodesPosInBaseArray[node]=basePtr;
    baseArray[basePtr++]=cost;

    ll specialChildSubTreeSize=0ll;
    ll specialChild=-1;
    ll specialChildCost=-1;

    for(ll i=0;i<adj[node].size();i++){
      ll child = adj[node][i].first;
      if(child==par)continue;
      if(subtreeSize[child]>specialChildSubTreeSize){
        specialChildSubTreeSize = subtreeSize[child];
        specialChild = child;
        specialChildCost = adj[node][i].second;
      } 
    }

    if(specialChild!=-1)HLD(specialChild,node,specialChildCost);

    for(ll i=0;i<adj[node].size();i++){
      ll child = adj[node][i].first;
      if(child==par or child==specialChild)continue;
      curChain++;
      HLD(child,node,adj[node][i].second);
    }
}

// Construct segment tree to find the maximum cost in the range
void makeTree(ll node,ll start, ll end){
  if(start==end){
    segTree[node]=baseArray[start];
    return;
  }
  ll leftNode = (node<<1)+1;
  ll rightNode = leftNode+1;
  ll mid = (start+end)/2;
  makeTree(leftNode,start,mid);
  makeTree(rightNode,mid+1,end);
  segTree[node] = max(segTree[leftNode],segTree[rightNode]);
}

// Updating the edge cost
void pointUpdate(ll node, ll start, ll end, ll point, ll val){
  if(start==point and end==point){
    segTree[node]=val;
    baseArray[point]=val;
    return ;
  }
  if(start>point or end<point)return;
  if(start>end)return;

  ll leftNode = (node<<1)+1;
  ll rightNode = leftNode+1;
  ll mid = (start+end)/2;
  pointUpdate(leftNode,start,mid,point,val);
  pointUpdate(rightNode,mid+1,end,point,val);
  segTree[node] = max(segTree[leftNode],segTree[rightNode]);
}

// Finding the maximum cost in the range
ll rangeQuery(ll node,ll start, ll end, ll init, ll fin){
  if(start>end or start>fin or end<init)return -1;
  if(start>=init and end<=fin)return segTree[node];
  ll leftNode = (node<<1)+1;
  ll rightNode = leftNode+1;
  ll mid = (start+end)/2;
  ll leftMax = rangeQuery(leftNode,start,mid,init,fin);
  ll rightMax = rangeQuery(rightNode,mid+1,end,init,fin);
  return max(leftMax,rightMax);
}

// Generating the euler array and level array for computing LCA in logN time
void generateEulerArrayAndLevelArray(ll node, ll par, ll lev){
    eulerArray.pb(node);
    level.pb(lev);
    for(ll i=0;i<adj[node].size();i++){
        ll child = adj[node][i].first;
        if(child==par)continue;
        generateEulerArrayAndLevelArray(child,node,lev+1);
        eulerArray.pb(node);
        level.pb(lev);
    }
}

// Computing the index of a node in the eulerArray where they occured for the first time
void findFirstOccurence(){
    for(ll i=0;i<eulerArray.size();i++){
        if(firstOccurence[eulerArray[i]]==-1) firstOccurence[eulerArray[i]]=i;
    }
}

// This segment tree stores the minimum level and the index of the minimum level in the level array between two indices
void makeSegTreeForLca(ll node,ll start, ll end){
    if(start>end)return;

    if(start==end){
        segTreeForLCA[node]={level[start],start};
        return;
    }
    ll leftNode = (node<<1)+1;
    ll rightNode = leftNode+1;
    ll mid = (start+end)/2;
    makeSegTreeForLca(leftNode,start,mid);
    makeSegTreeForLca(rightNode,mid+1,end);
    if(segTreeForLCA[leftNode].first<segTreeForLCA[rightNode].first){
        segTreeForLCA[node]=segTreeForLCA[leftNode];
    }
    else segTreeForLCA[node]=segTreeForLCA[rightNode]; 
}

// return the minimum level and the index of the minimum level in the level array between init and fin indexes
pll RMQ(ll node,ll start, ll end, ll init, ll fin){
      if(start>end)return {inf,inf};
      if(start>fin or end<init)return {inf,inf};
      if(start>=init and end<=fin)return segTreeForLCA[node];
      ll leftNode = (node<<1)+1;
      ll rightNode = leftNode+1;
      ll mid = (start+end)/2;
      pll leftMin = RMQ(leftNode,start,mid,init,fin);
      pll rightMin = RMQ(rightNode,mid+1,end,init,fin);
      if(leftMin.first<rightMin.first)return leftMin;
      return rightMin;
}

void preProcessForLCAUsingRMQ(){
    
    // Step 1. Do Euler's tour assuming 0 to be the root
    generateEulerArrayAndLevelArray(0,-1,0);

    // Step 2. Find the first occurence of every node in the euler array
    findFirstOccurence();

    ll sz = level.size();
    segTreeForLCA=vector<pll>(4*sz+1,{inf,inf});
    // Step 3. makeSegmentTree using levelArray
    makeSegTreeForLca(0,0,sz-1);
}

// computes the LCA of nodes u and v in log N time
ll LCA(ll u,ll v){
    ll sz = level.size();
    ll occurU = firstOccurence[u];
    ll occurV = firstOccurence[v];
    if(occurU > occurV)swap(occurU,occurV);
    return eulerArray[RMQ(0,0,level.size()-1,occurU,occurV).second];
}

// Returns the maximum edge cost for nodes between u and v where v is at some upper level relative to v
ll queryUp(ll u, ll v){
  if(u==v)return 0;
  ll uchain;
  // vchain stores the chain number of the node v 
  ll vchain = nodesChain[v];
  ll ans=-1;
  while(1){
    // uchain stores the chain number of the node u
    uchain = nodesChain[u];
    if(uchain==vchain){
      if(u==v)break;
      ll uPosInBase = nodesPosInBaseArray[u];
      ll vPosInBase = nodesPosInBaseArray[v];
      if(uPosInBase>vPosInBase)swap(uPosInBase,vPosInBase);
      ans = max(ans,rangeQuery(0,0,baseArray.size()-1,uPosInBase+1,vPosInBase)); // +1 because for storing the cost of an edge (u,v), the baseArray stores the edge cost at index v, not u
      break;
    }
    ll uChainHead = chainHead[uchain];
    ll uPosInBase = nodesPosInBaseArray[u];
    ll uChainHeadPositionInBase = nodesPosInBaseArray[uChainHead];
    ans = max(ans,rangeQuery(0,0,baseArray.size()-1,uChainHeadPositionInBase,uPosInBase));
    u=parent[uChainHead];
  }
  return ans;
}

void change(ll ind,ll newWeight){
  ll u = edgeList[ind].first.first;
  ll v = edgeList[ind].first.second;
  if(parent[v]==u) swap(u,v); // Making sure that v is always the parent of u because for storing the cost of an edge (u,v), the baseArray stores the edge cost at index v, not u. Hence this order matters.
  ll nodesPosition = nodesPosInBaseArray[u];
  pointUpdate(0,0,baseArray.size()-1,nodesPosition,newWeight);
}

void query(ll u, ll v){
  ll lca = LCA(u,v);
  ll ans = queryUp(u,lca);
  ll temp = queryUp(v,lca);
  p(max(ans,temp));
}

int main() { 

// freopen("input.txt","r",stdin);
// freopen("output.txt","w",stdout);   
ll t=1;   
s(t);   
while(t--){
     ll n;
     s(n);
     // Stores edge information position wise
     edgeList=vector<pair<pll,ll>>(n);     
     // Adjacency list
     adj=vector<vector<pll>>(n);
     // Stores parent of every node
     parent=vector<ll>(n);
     // Stores subtree size of every node
     subtreeSize=vector<ll>(n,1);
     // chainHead[i] stores the node number which is the head of the ith chain
     chainHead=vector<ll>(n,-1);  
     // nodesChain[i] stores the chain of which ith node is the member of
     nodesChain=vector<ll>(n,-1);
     // baseArray contains edges cost and very importantly, it stores the cost contiguously for edges which are members of a particular chain together
     // i.e. no two edges of the same chain will be separated by the edge of another chain
     // This is mainly done so that segment tree can be constructed later on
     baseArray=vector<ll>(n,-1);     
     // nodesPosInBaseArray[i] stores the index in baseArray at which ith node is stored
     nodesPosInBaseArray=vector<ll>(n,-1);
     // Segment tree in which every node contains the maximum cost of the edge of the range of the nodes that segmentTreeNode stores
     segTree=vector<ll>(4*n+1,0);     
     firstOccurence=vector<ll>(n,-1);
     eulerArray=vector<ll>();     
     level=vector<ll>();


    F(i,1,n-1){
      ll u,v,w;
      s3(u,v,w);
      u--; 
      v--;
      edgeList[i]={{u,v},w};
      adj[u].pb({v,w});
      adj[v].pb({u,w});
    }


    curChain=0;
    basePtr=0;
    dfs(0); // assuming 0 to be the root of the tree, dfs sets the subtree size and parent of every node in the tree
    HLD(0);
    makeTree(0,0,basePtr-1);
    preProcessForLCAUsingRMQ();

    while(1){
      char need[10];
      scanf("%s",&need);
      if(need[0]=='D')break;
      if(need[0]=='Q'){
        ll u,v;
        s2(u,v);
        u--; v--;
        query(u,v);
      }
      else if(need[0]=='C'){
        ll ind,newWeight;
        s2(ind,newWeight);
        change(ind,newWeight);
      }
    }
  }
}

```
