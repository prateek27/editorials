## GSS7(SPOJ)
##### Problem Link :[GSS7(SPOJ)]()

##### PreRequisite try to solve these 2 problems:
1. [Maximum Sum Query](https://hack.codingblocks.com/contests/c/203/310)
2. [Tree Decomposition](https://hack.codingblocks.com/contests/c/203/1100)

Once you solve these 2 problems,only then you will be able to solve this problem.Problem 1 is just a basic segment tree question that is very similar to
this problem,except that we are given an array instead of a tree.
Problem 2 will teach you how to do point updates and range queries on trees using HLD.
This problem will teach you how to perform Range Queries along with lazy propagation using HLD.

```C++
# include <iostream>
# include <bits/stdc++.h>
using namespace std;
# define ll long
struct node
{
	ll maxsum,totalsum,prefix,suffix;
};
 
const ll N=100000+5;
const ll SEG_MAX=4*100000+5;
const ll isValueOnEdge=0;
const ll lazyDefault=100000;
 
vector<vector<ll>>adj;
vector<ll>node_cost(N);
vector<struct node>segArr(SEG_MAX);
vector<ll>lazytree(SEG_MAX);
ll posSegTree[N];
ll n;
 
class SegmentTree
{
	void merge(int pos,int i,int j)
	{
		segArr[pos].totalsum=segArr[2*pos].totalsum+segArr[2*pos+1].totalsum;
		segArr[pos].prefix=max(segArr[2*pos].prefix,segArr[2*pos].totalsum+segArr[2*pos+1].prefix);
		segArr[pos].suffix=max(segArr[2*pos+1].suffix,segArr[2*pos+1].totalsum+segArr[2*pos].suffix);
		segArr[pos].maxsum=max(max(segArr[2*pos].maxsum,segArr[2*pos+1].maxsum),segArr[2*pos].suffix+segArr[2*pos+1].prefix);
	}
public:
	ll to,from,val;
	void createSegTree(ll s,ll e,ll pos)
	{
		if(s==e)
		{
			segArr[pos].maxsum=node_cost[posSegTree[s]];
			segArr[pos].totalsum=segArr[pos].maxsum;
			segArr[pos].prefix=segArr[pos].maxsum;
			segArr[pos].suffix=segArr[pos].maxsum;
			return;
		}
		ll mid=(s+e)/2;
		createSegTree(s,mid,2*pos);
		createSegTree(mid+1,e,2*pos+1);
		merge(pos,2*pos,2*pos+1);
	}
	struct node query(ll s,ll e,ll pos)
	{
		if(lazytree[pos]!=lazyDefault)
		{
			segArr[pos].totalsum=lazytree[pos]*(e-s+1);
			if(lazytree[pos]>0)
			{
				segArr[pos].prefix=segArr[pos].totalsum;
				segArr[pos].suffix=segArr[pos].totalsum;
				segArr[pos].maxsum=segArr[pos].totalsum;
			}
			else
			{
				segArr[pos].prefix=lazytree[pos];
				segArr[pos].suffix=lazytree[pos];
				segArr[pos].maxsum=lazytree[pos];
			}
			if(s!=e)
            {
                lazytree[2*pos]=lazytree[2*pos+1]=lazytree[pos];
            }
			lazytree[pos]=lazyDefault;
		}
		if(from<=s && to>=e)
            return segArr[pos];
		if(from>e || to<s)
		{
			struct node temp;
			temp.totalsum=0;
			temp.prefix=-10005;
			temp.suffix=-10005;
			temp.maxsum=-10005;
			return temp;
		}
		ll mid=(s+e)/2;
		struct node left=query(s,mid,2*pos);
		struct node right=query(mid+1,e,2*pos+1);
		struct node temp;
		temp.totalsum=left.totalsum+right.totalsum;
		temp.prefix=max(left.prefix,left.totalsum+right.prefix);
		temp.suffix=max(right.suffix,right.totalsum+left.suffix);
		temp.maxsum=max(max(left.maxsum,right.maxsum),left.suffix+right.prefix);
		return temp;
	}
	void update_segtree(ll s,ll e,ll pos)
	{
		if(s>e)
			return;
		if(lazytree[pos]!=lazyDefault)
		{
			segArr[pos].totalsum=lazytree[pos]*(e-s+1);
			if(lazytree[pos]>0)
			{
				segArr[pos].prefix=segArr[pos].totalsum;
				segArr[pos].suffix=segArr[pos].totalsum;
				segArr[pos].maxsum=segArr[pos].totalsum;
			}
			else
			{
				segArr[pos].prefix=lazytree[pos];
				segArr[pos].suffix=lazytree[pos];
				segArr[pos].maxsum=lazytree[pos];
			}
			if(s!=e)
                lazytree[2*pos]=lazytree[2*pos+1]=lazytree[pos];
			lazytree[pos]=lazyDefault;
		}
		if(from>e || to<s)
			return;
		if(from<=s && to>=e)
		{
			segArr[pos].totalsum=val*(e-s+1);
			if(val>0)
			{
				segArr[pos].prefix=segArr[pos].totalsum;
				segArr[pos].suffix=segArr[pos].totalsum;
				segArr[pos].maxsum=segArr[pos].totalsum;
			}
			else
			{
				segArr[pos].prefix=val;
				segArr[pos].suffix=val;
				segArr[pos].maxsum=val;
			}
			if(s!=e)
			{
				lazytree[2*pos]=val;
				lazytree[2*pos+1]=val;
			}
			return;
		}
		ll mid=(s+e)/2;
		update_segtree(s,mid,2*pos);
		update_segtree(mid+1,e,2*pos+1);
		merge(pos,2*pos,2*pos+1);
	}
};
 
class HeavyLight
{
	ll parent[N],heavy[N],depth[N];
	ll root[N];
	ll segtreePos[N];
	ll queryRes;
	SegmentTree segTree;
	ll dfs_hld(ll v)
	{
		ll size=1;
		ll maxsubtree=0;
		for(ll i=0;i<adj[v].size();i++)
		{
			ll u=adj[v][i];
			if(u!=parent[v])
			{
				parent[u]=v;
				depth[u]=depth[v]+1;
				ll childTreeSize=dfs_hld(u);
				if(childTreeSize>maxsubtree)
				{
					maxsubtree=childTreeSize;
					heavy[v]=u;
				}
				size+=childTreeSize;
			}
		}
		return size;
	}
public:
	void buildChains()
	{
		memset(heavy,-1,n*sizeof(ll));
		parent[0]=-1;
		depth[0]=0;
		dfs_hld(0);
		for(ll v=0,pos=0;v<n;v++)
		{
			if(parent[v]==-1 || heavy[parent[v]]!=v)
			{
				ll j=v;
				while(j!=-1)
				{
					root[j]=v;
					posSegTree[pos]=j;
					segtreePos[j]=pos++;
					j=heavy[j];
				}
			}
		}
		segTree.createSegTree(0+isValueOnEdge,n-1,1);
	}
	struct node queryChain(ll l,ll r)
	{
		segTree.from=l;
		segTree.to=r;
		struct node temp=segTree.query(0+isValueOnEdge,n-1,1);
		return temp;
	}
	ll queryPath(ll u,ll v)
	{
		struct node left,right;
		left.totalsum=right.totalsum=0;
		left.prefix=right.prefix=-10005;
		left.suffix=right.suffix=-10005;
		left.maxsum=right.maxsum=-10005;
		while(root[u]!=root[v])
		{
			if(depth[root[u]]>depth[root[v]])
			{
				struct node temp=queryChain(segtreePos[root[u]],segtreePos[u]);
				left.maxsum=max(max(left.maxsum,temp.maxsum),left.prefix+temp.suffix);
                left.prefix=max(temp.prefix,temp.totalsum+left.prefix);
                left.suffix=max(left.suffix,left.totalsum+temp.suffix);
                left.totalsum+=temp.totalsum;
                u=parent[root[u]];
			}
			else
			{
				struct node temp=queryChain(segtreePos[root[v]],segtreePos[v]);
				right.maxsum=max(max(temp.maxsum,right.maxsum),temp.suffix+right.prefix);
                right.prefix=max(temp.prefix,temp.totalsum+right.prefix);
                right.suffix=max(right.suffix,right.totalsum+temp.suffix);
                right.totalsum+=temp.totalsum;
				v=parent[root[v]];
			}
		}
		if(depth[u]>depth[v])
		{
			if(!isValueOnEdge || u!=v)
			{
				struct node temp=queryChain(segtreePos[v]+isValueOnEdge,segtreePos[u]);
				left.maxsum=max(max(left.maxsum,temp.maxsum),left.prefix+temp.suffix);
                left.prefix=max(temp.prefix,temp.totalsum+left.prefix);
                left.suffix=max(left.suffix,left.totalsum+temp.suffix);
                left.totalsum+=temp.totalsum;
			}
		}
		else
		{
			if(!isValueOnEdge || u!=v)
			{
				struct node temp=queryChain(segtreePos[u]+isValueOnEdge,segtreePos[v]);
				right.maxsum=max(max(temp.maxsum,right.maxsum),temp.suffix+right.prefix);
                right.prefix=max(temp.prefix,temp.totalsum+right.prefix);
                right.suffix=max(right.suffix,right.totalsum+temp.suffix);
                right.totalsum+=temp.totalsum;
                  
		    }
		}
		queryRes=max(max(left.maxsum,right.maxsum),left.prefix+right.prefix);
		if(queryRes<0)
			return 0;
		else
			return queryRes;
	}
 
	void update_node(ll treeNode,ll value)
	{
		segTree.from=segtreePos[treeNode];
		segTree.to=segtreePos[treeNode];
		segTree.val=value;
		segTree.update_segtree(0+isValueOnEdge,n-1,1);
	}
	void update_chain(ll l,ll r,ll value)
	{
		segTree.from=l;
		segTree.to=r;
		segTree.val=value;
		segTree.update_segtree(0+isValueOnEdge,n-1,1);
	}
	void update_range(ll u,ll v,ll value)
	{
		while(root[u]!=root[v])
		{
			if(depth[root[u]]>depth[root[v]])
				swap(u,v);
			update_chain(segtreePos[root[v]],segtreePos[v],value);
			v=parent[root[v]];
		}
		if(depth[u]>depth[v])
			swap(u,v);
		if(!isValueOnEdge || u!=v)
			update_chain(segtreePos[u]+isValueOnEdge,segtreePos[v],value);
	}
};
 
int main()
{
    HeavyLight hld;
	cin>>n;
	adj.resize(n);
	for(ll i=0;i<n;i++)
		cin>>node_cost[i];
	for(ll i=0;i<n-1;i++)
	{
		ll u,v;
		cin>>u>>v;
		adj[u-1].push_back(v-1);
		adj[v-1].push_back(u-1);
	}
	hld.buildChains();
	fill(lazytree.begin(),lazytree.end(),lazyDefault);
    ll q;
	cin>>q;
	while(q--)
	{
		ll type;
		cin>>type;
		if(type==1)
		{
			ll a,b;
            cin>>a>>b;
			printf("%ld\n",hld.queryPath(a-1,b-1));
		}
		else
		{
			ll a,b,c;
			cin>>a>>b>>c;
			hld.update_range(a-1,b-1,c);
		}
	}
 
}
```