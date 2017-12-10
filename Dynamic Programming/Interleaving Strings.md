## Interleaving Strings
##### Problem Link : [Interleaving Strings](https://hack.codingblocks.com/contests/c/1001/722)


If n is the size of str1 and m be the size of str2, then for str3 to be the string formed by interleaving of str1 and str2, the last character of str3 should match the nth character of str1 or mth character of str2. 
Similarly, for 'every' first i+j characters of str3 to be the interleaving string formed from first i characters of str1 and first j characters of str2, (i+j)th character of str3 should match with ith character of str1 or jth character from str2.

_**Time Complexity:** O(R+C)_

Here is the bottom  up solution for this problem.

```C++
#include<bits/stdc++.h>
#define mod 1000000007
#define pp pair<ll,ll>
#define mp make_pair
#define ll long long
#define pb push_back
#define ff first
#define ss second
using namespace std;

int isInterLeave(string A, string B, string C) {
    int m = A.length();
    int n = B.length();
    int dp[m+10][n+10];
    memset(dp,0,sizeof(dp));
    dp[0][0] = 1;
    for(int i=1;i<=m;i++){
        if(A[i-1] == C[i-1])
            dp[i][0] = dp[i-1][0];//prefix(length i) of c matches prefix(length i) of a
    }
    for(int j=1;j<=n;j++){
        if(B[j-1] == C[j-1])
            dp[0][j] = dp[0][j-1];//prefix(length j) of c matches prefix(length j) of b
    }
    for(int i=1;i<=m;i++){
        for(int j=1;j<=n;j++){
            if(A[i-1] == C[i+j-1])//char from A mathces char in C
                dp[i][j] = dp[i][j] || dp[i-1][j];
            if(B[j-1] == C[i+j-1])//char from B mathces char in C
                dp[i][j] = dp[i][j] || dp[i][j-1];
        }
    }
    return dp[m][n];
}


int main(){
//	freopen("input3.txt","r",stdin);
//	freopen("output.txt","w",stdout);
//	ios::sync_with_stdio(0);
	
	string A,B,C;
	cin>>A>>B>>C;
	cout<<isInterLeave(A,B,C);
	
	return 0;
}

```


