## Crossig Blocks Game
##### Problem Link :[Crossing Blocks Game]()

Initially,we can calculate the total number of ways to cross ai number of blocks.
This can be done easily using dp

dp[i]=dp[i-1]+dp[i-2];

Once this is calculated,we have to just add dp[a[i]] to the score of the person who chooses ith number.
Important point to note here is that we cant apply dynamic programming optimal strategy to play this game.
This is because player can only see first and last numbers in the sequence.In order to apply optimal strategy,players should have been able to see the entire sequence.Therefore we will apply greedy approach and choose the maximum ai out of first and last numbers(since dp[a[i]] is strictly increasing).
Note that the case when both first and last numbers are equal,then problem can have multiple solutions.

_**Time Complexity:** O(N)

```C++
# include <iostream>
# include <bits/stdc++.h>
using namespace std;
int a[41];
int calculate(int arr[],int i,int j,int player,int score[])
{
    if(i==j)
    {
        score[player]+=a[arr[i]];
        return 0;
    }
    if(arr[i]>arr[j])
    {
        score[player]+=a[arr[i]];
        calculate(arr,i+1,j,(player+1)%2,score);
    }
    else if(arr[i]<=arr[j])
    {
        score[player]+=a[arr[j]];
        calculate(arr,i,j-1,(player+1)%2,score);
    }
    return 0;
}
int main()
{
	//int a[21];
	a[1]=1;
	a[2]=2;
	for(int i=3;i<=40;i++)
		a[i]=a[i-1]+a[i-2];
	int t;
	cin>>t;
	while(t--)
	{
		int n;
		cin>>n;
		int arr[n+1];
		int sum=0;
		for(int i=1;i<=n;i++)
		{
			cin>>arr[i];
			sum+=a[arr[i]];
		}
		int score[2];
		score[0]=score[1]=0;
		calculate(arr,1,n,0,score);
		if(score[0]>score[1])
			cout<<"First"<<endl;
		else if(score[0]==score[1])
			cout<<"Tie"<<endl;
		else
			cout<<"Second"<<endl;
	}
}
```
