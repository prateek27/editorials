## SUM IT UP
##### Problem Link : [Sum It Up](https://hack.codingblocks.com/contests/c/1001/912)  

This question is pretty intuitive. Just consider a particular number to be taken or not for the needed combination. Either you can take the given number of the array to the already run combination, or you start a totally new combination from the given number or don't consider the given number at all.
Maintain a set to avoid duplicate combinations and finally sort them up to print the answer in the needed format.

_**Time Complexity:** O(3<sup>N</sup>)_ where N = Number of elements in the array

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

// ans is a vector which will contain distinct combinations of numbers summing up to the desired value
vector<vector<int> >ans;

// mp is set which will contain the already found combinations to prevent duplicates.
set<vector<int> > mp;

// B is the required target sum
int B;

// st =  will contain the combination of numbers taken so far
// req = is the required sum which is still to be found
// i = is the current index of the array A to be considered
void solve(vector<int> &A,int i,int req,vector<int> st){
    if(i>A.size() or req<0)return ;
    
    // if req is zero, means st contains the combination of numbers whose sum equals B
    if(req==0){
        if(!st.empty() and mp.find(st)==mp.end()){ // only consider st if it is distinct
            mp.insert(st);
            ans.push_back(st);
        }
    }
    
    if(i==A.size())return;
    
    // Case 1: Add the given number A[i] to the combination considered so fat
    st.push_back(A[i]);
    solve(A,i+1,req-A[i],st);
    st.pop_back();
    
    // Case 2: Don't consider the number A[i] for the combination at all
    solve(A,i+1,req,st);
    
    // Case 3: Start a totally new combination with A[i]
    vector<int> st2;
    st2.push_back(A[i]);
    solve(A,i+1,B-A[i],st2);
}


vector<vector<int> > combinationSum(vector<int> &A, int B) {
    ans.clear();
    mp.clear();
    ::B=B;
    if(A.size()==0 or B==0)return ans;
    sort(A.begin(),A.end());
    vector<int> st;
    solve(A,0,B,st);

    // sort it as the needed set of combinations needs to be sorted
    sort(ans.begin(),ans.end());
    return ans;
}

int main()
{
	// freopen("input.txt","r",stdin);
 	// freopen("output.txt","w",stdout);
	int t=1;
	//s(t);
	while(t--){
		int n;
		cin>>n;
		vector<int>ar(n);
		F(i,0,n-1)cin>>ar[i];
		int t;
		cin>>t;
		vector<vector<int>> ans = combinationSum(ar,t);
		for(auto vec:ans){
			for(int num:vec){
				cout<<num<<" ";
			}
			cout<<endl;
		}
	}
}

```
