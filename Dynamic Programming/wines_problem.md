## Wines Problem
Refer Tutorial for explanation.

```c
#include<iostream>
using namespace std;

int dp[100][100] = {0};

int winesProfit(int *wines,int i,int j,int year){
	//Base Case 
	if(i>j){
		return 0;
	}
	if(dp[i][j]!=0){
		return dp[i][j];
	}
	
	int op1 = wines[i]*year + winesProfit(wines,i+1,j,year+1);
	int op2 = wines[j]*year + winesProfit(wines,i,j-1,year+1);
	return dp[i][j]= max(op1,op2);
}

int main(){

	int wines[] = {2,3,5,1,4};
	int n = 5;
	cout<<winesProfit(wines,0,n-1,1);
	return 0;
}
```
