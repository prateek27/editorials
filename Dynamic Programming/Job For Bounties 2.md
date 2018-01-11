## Job For Bounties II
##### Problem Link : [Job For Bounties II](https://hack.codingblocks.com/contests/c/141/963)
Here The problem would have been fairly easy if there were no deadlines assosciated with the jobs.If that were the case a simple recurrence would give us the answer.
But the deadlines in this question makes our task a little difficult.

CLAIM:Before beginning the main part of our dynamic programming algorithm, we will sort the
jobs according to deadline, so that d1<= d2<= · · ·<=dn = d, where d is the largest deadline.

We can prove this by taking few examples.
For Example -Let there be 2 jobs with following description
Reward   - 2	100
Time     - 6	5
Deadline - 11   5

Now we have to tell the maximum bounties that can be earned in 11 units of time.
If we do not sort the jobs by deadline,then maximum profit we can get is 100 by doing job2 and skipping job1.
However if we sort the jobs according to the deadlines,then we can get a mximum profit of 102.

DP RECURRENCE:Let dp(i,t) represent maximum bounties obtaind from a feasible schedule in which only jobs from {1,...,i} are scheduled,and all scheduled jobs finish by time t}.
	• dp(0,t) = 0 for all t, 0 = t = T.
	• Let 1 = i = n, 0 = t = T. Define t'= min{t, di} - ti.Clearly t' is the latest possible
	time that we can schedule job i, so that it ends both by its deadline and by time t.
	Then we have:
If t'< 0, then dp(i,t) = dp(i-1,t).
If t'= 0, then dp(i,t) = max{A(i-1,t),dp(i-1,t')+rewardi}.

_**Time Complexity:** O(N*T)_ where N is the number of jobs and T is the total time units.

```C++

# include <iostream>
# include <bits/stdc++.h>
using namespace std;
# define ll long long
ll dp[1001][100001];
struct node
{
	ll duration;
	ll reward;
	ll deadline;
};
vector<struct node>arr(1001);
bool compare(struct node &x,struct node &y)
{
	if(x.deadline<=y.deadline)
		return true;
	else
		return false;
}

int main()
{
	ll t,n;
	cin>>t>>n;
	for(ll i=1;i<=n;i++)
	    cin>>arr[i].reward;
	for(ll i=1;i<=n;i++)
		cin>>arr[i].duration;
	for(ll i=1;i<=n;i++)
		cin>>arr[i].deadline;
	sort(arr.begin()+1,arr.begin()+n+1,compare);
	ll dp[n+1][t+1];
	for(ll j=0;j<=t;j++)
		dp[0][j]=0;
	for(ll i=1;i<=n;i++)
	{
		for(ll j=0;j<=t;j++)
		{
			int k=min(j,arr[i].deadline)-arr[i].duration;
			if(k<0)
			    dp[i][j]=dp[i-1][j];
			else
			    dp[i][j]=max(dp[i-1][j],arr[i].reward+dp[i-1][k]);
		}
	}
	cout<<dp[n][t]<<endl;
}
```

