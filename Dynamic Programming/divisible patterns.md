## Divisible Patterns
##### Problem Link :[Divisible Patterns](https://hack.codingblocks.com/contests/c/141/1001)

## PreRequisite:Z-Algorithm
Once we find the set of indexes using Z-algorithm,then approaching the problem further seems quite easy.It seems just a variation of the following problem: Find out the number of ways to choose some numbers from the arr A such that the sum of chosen numbers is divisible by all the numbers from 1 to 9 modulo 10^9+7.
How to approach this problem?
Let dp[i][j] represent number of ways such that we get modulo sum as j by considering numbers from indexes 1 to i
Then,
dp[i][j]=dp[i-1][j]+dp[i-1][(j-arr[i])%M]

Why same approach cant be used in original problem..?
Since inverse modulo is not always possible,we cant use the same approach while calculating modulo product.
That is,
If dp[i][j] represent number of ways such that we get modulo product j by multiplying numbers from indexes 1 to i
Then,
dp[i][j]=dp[i-1][j]+dp[i-1][(j/arr[i])%M];
This recurrnce is correct but is not possible to calculate it,because inverse may not always exist.

##How to approach original problem..?
We can see that LCM of all numbers from 1..9 is 2520=(2 * 2 * 2) * (3 * 3) * (5) * (7)
Any number divisible by 2520 should have atleast 3 two,2 three,1 five, 1 seven.
Therefore we can mantain dp[end][two][three][five][seven];
Since we only want to know ways to form numbers which are divisible by 2520,hence the dimensions of our Dp will be 2*4*3*2*2
The rest is easy to understand from code.Note:"end" dimension is used to distinguish betweeen number of ways until previous element with number of ways until current element.

_**Time Complexity:** O(N * 4 * 3 * 2 * 2)_

```C++
# include <iostream>
# include <bits/stdc++.h>
using namespace std;
# define ll long long
# define lcm 2520
# define mod 1000000007
string text,pattern;
vector<ll>z(20002);
vector<ll>position;
ll dp[2][4][3][2][2];

ll calculateZ(string s)
{
	z[0]=0;
	ll left,right;
	left=right=0;
	for(ll k=1;k<s.length();k++)
	{
		if(k>right)
		{
			left=right=k;
			while(right<s.length() && s[right]==s[right-left])
				right++;
			z[k]=right-left;
			right--;
		}
		else
		{
			ll k1=k-left;
			if(z[k1]<right-k+1)
				z[k]=z[k1];
			else if(z[k1]>right-k+1)
				z[k]=right-k+1;
			else
			{
				left=k;
				while(right<s.length() && s[right]==s[right-left])
					right++;
				z[k]=right-left;
				right--;
			}
		}
	}
}
int main()
{
	cin>>pattern>>text;
	string s=pattern+"$"+text;
	//cout<<s<<endl;
	calculateZ(s);
	for(ll i=0;i<z.size();i++)
	{
		if(z[i]==pattern.length())
			position.push_back(i-pattern.length()-1);
	}
	for(ll i=0;i<position.size();i++)
    {
        position[i]++;
        //cout<<position[i]<<endl;
    }
    ll n=position.size()-1;
    memset(dp,0,sizeof(dp));
    ll curr=0;
    ll prev=1;
    for(ll i=0;i<=n;i++)
    {
        for(ll j=0;j<4;j++)
        for(ll k=0;k<3;k++)
        for(ll p=0;p<2;p++)
        for(ll q=0;q<2;q++)
            dp[curr][j][k][p][q]=dp[prev][j][k][p][q];
    	ll two,three,five,seven;
    	if(position[i]%8==0) two=3;
    	else if(position[i]%4==0) two=2;
   	    else if(position[i]%2==0) two=1;
    	else two=0;
    	if(position[i]%9==0) three=2;
    	else if(position[i]%3==0) three=1;
    	else three=0;
    	if(position[i]%5==0) five=1;
    	else five=0;
        if(position[i]%7==0) seven=1;
    	else seven=0;
    	dp[curr][two][three][five][seven]=(dp[curr][two][three][five][seven]+1)%mod;
    	for(ll j=0;j<4;j++)
    	{
    		for(ll k=0;k<3;k++)
    		{
    			for(ll p=0;p<2;p++)
    			{
    				for(ll q=0;q<2;q++)
    				{
    					ll ntwo=min(3ll,j+two);
    					ll nthree=min(2ll,k+three);
    					ll nfive=min(1ll,p+five);
    					ll nseven=min(1ll,q+seven);
    					dp[curr][ntwo][nthree][nfive][nseven]=(dp[curr][ntwo][nthree][nfive][nseven]+dp[prev][j][k][p][q])%mod;
					}
    			}
    		}
    	}
    	curr=!curr;
    	prev=!prev;
    }
    cout<<dp[prev][3][2][1][1]<<endl;

}
```
