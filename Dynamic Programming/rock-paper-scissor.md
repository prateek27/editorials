## Rock,Paper,Scissor - HackerBlocks
Refer Tutorial for explanation.

```c
#include <bits/stdc++.h>
#include<stdio.h>
using namespace std;
#define F(i,a,b) for(int i = a; i <= b; i++)
#define ll double

double dp[105][105][105];

void set_dp()
{
	F(i,0,102)
		F(j,0,102)
			F(k,0,102)
				dp[i][j][k] = -1.0;
}

double f1(int r,int s,int p)
{
	if(r == 0 || s == 0)
		return 0.0;
	if(p == 0)
		return 1.0;
	if(dp[r][s][p]!=-1.0)
		return dp[r][s][p];
	double tot = r*s + r*p + s*p;
	
	double ret = 0.0;
	
	ret += f1(r-1,s,p) * ((r*p)/tot); // paper kills rock
	ret += f1(r,s-1,p) * ((r*s)/tot); // rock kills scissor
	ret += f1(r,s,p-1) * ((s*p)/tot); // scissor kills paper
	return dp[r][s][p] = ret;
}


double f2(int r,int s,int p)
{
	if(p == 0 || s == 0)
		return 0.0;
	if(r == 0)
		return 1.0;
	if(dp[r][s][p]!=-1.0)
		return dp[r][s][p];
	double tot = r*s + r*p + s*p;
	double ret = 0.0;
	ret += f2(r-1,s,p) * ((r*p)/tot); // paper kills rock
	ret += f2(r,s-1,p) * ((r*s)/tot); // rock kills scissor
	ret += f2(r,s,p-1) * ((s*p)/tot); // scissor kills paper
	return dp[r][s][p] = ret;
}

double f3(int r,int s,int p)
{
	if(r == 0 || p == 0)
		return 0.0;
	if(s == 0)
		return 1.0;
	if(dp[r][s][p]!=-1.0)
		return dp[r][s][p];
	double tot = r*s + r*p + s*p;
	double ret = 0.0;
	ret += f3(r-1,s,p) * ((r*p)/tot); // paper kills rock
	ret += f3(r,s-1,p) * ((r*s)/tot); // rock kills scissor
	ret += f3(r,s,p-1) * ((s*p)/tot); // scissor kills paper
	return dp[r][s][p] = ret;
}
int main() 
{
	int t;
	cin>>t;
	while(t--)
	{
		int r,s,p;
		cin>>r>>s>>p;
		set_dp();
		double ans1 = f1(r,s,p);
		set_dp();
		double ans2 = f2(r,s,p);
		set_dp();
		double ans3 = f3(r,s,p);
		
		
		cout<<fixed<<setprecision(9)<<ans1<<" "<<ans2<<" "<<ans3<<endl;
	}
	return 0;
}
```
