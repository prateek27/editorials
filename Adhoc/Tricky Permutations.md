## TRICKY PERMUTATIONS
##### Problem Link : [Tricky Permutations](https://hack.codingblocks.com/contests/c/1001/737)  

There are various ways to solve this question. However the most efficient way in terms of both space and time is presented below without the use of any advanced data structure.
Here we have to avoid the printing of any duplicate permuatation without using any data structure like maps or stl.
 Refer to the below commented code for implementation.

_**Time Complexity:** O(N!)_ where N = Length of the string

```C++
		
  #include<bits/stdc++.h>
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

  vector<string > res;

  void solve(string ar,int id){

    // Base Condition: Just passed the string ar into the res vector.
    if(id==ar.size()){
      res.push_back(ar);
      return;
    }

    for(int i=id;i<ar.size();i++){
      // This condition is responsible to prevent any duplicate permutations
      // If the element to be swapped is same as the element at index "id", then
      // do not swap as it won't produce any new permutation.
      if(i!=id and ar[i]==ar[id])continue;

      // Just swap the element at index i with the element at index id.
      // Note that since the string is already sorted from index i to ar.size()
      // this swapping will only produce the lexicographically next permutation
      // for the iteration i.
      swap(ar[id],ar[i]);

      // Now the string ar is modified lexicographically till index 'id', now just
      // solve it for the rest of the string from index id+1.
      solve(ar,id+1);
    }

  }

  vector<string> permute(string &A) {
    res.clear();
    // Sort the given string, then only the resultant permutations will be in 
    // a lexicographic order
    sort(A.begin(), A.end());

    // Call this recursive function with first parameter as string and second parameter
    // as the index to be focused.
    solve(A,0);

    // Now the res contains all the distinct permutations for the string A. Return it.
    return res;
  }


  int main()
  {
    // freopen("input.txt","r",stdin);
    //   freopen("output.txt","w",stdout);
    ll t=1;
    //s(t);
    while(t--){
      string str;
      cin>>str;
      vector<string> res = permute(str);
      for(ll i=0;i<res.size();i++)cout<<res[i]<<endl;
    //   for(string str:res)cout<<str<<endl;
    }
  }



```
