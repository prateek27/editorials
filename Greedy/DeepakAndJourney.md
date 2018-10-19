This is a simple greedy problem. We just need to buy petrol from the minimum possible rates checkpoints. Lets say we are at checkpoint X. Now there are X+1 checkpoints up to this checkpoint. We can easily see that for buying petrol for going to checkpoint X+1 , we just need to find the minimum possible C[i] where i can be from 1 to X+1 . Thus, we can just compare the minimum petrol rate till now(lets say denote it by (mi) at each of the checkpoint with the checkpoint rate i.e. C[i] . Now two possible cases arise:

- First Case: mi <= C[i],  No need to update mi i.e. petrol for this checkpoint has to be bought with the rate .

- Second Case: mi > C[i] , We need to update mi and petrol for this checkpoint has to be bought with the new rate i.e. C[i] .

After iterating through all checkpoints following the above algorithm will lead to minimum travel cost.

Following is the C++ implementation of the above logic:
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
  int tc;
  cin>>tc; // Take input for test cases
  while(tc--)
  {
    int n;
    cin>>n; // Take input of n i.e. total check points
    long long c[n+1],l[n+1]; // For simplicity prepare arrays of size n+1 to convert it to 1 based indexing
    for(int i=1;i<=n;i++)
      cin>>c[i]; //Take input for array C
    for(int i=1;i<=n;i++)
      cin>>l[i]; //Take input for array L
    long long mi = 1000000LL; // Variable to take care of the minimum cost of petrol upto a checkpoint X
    long long ans = 0;
    for(int i=1;i<=n;i++)
    {
      mi = min(mi,c[i]); // Compare the cose of petrol between cost at current checkpoint and the minimum cost till now
      ans+=mi*l[i]; 
    }
    cout<<ans<<endl;
  }
  return 0;
}
```
Following is the JAVA implementation of the above logic:
```java
import java.util.*;

public class DeepakandJourney {
	public static void main(String[] args) {
		Scanner scn = new Scanner(System.in);
		int t = scn.nextInt();  // Take input for test cases
		while(t-- > 0) {
			int n = scn.nextInt();  // Take input of n i.e. total check points
			long[] C = new long[n+1]; // For simplicity prepare arrays of size n+1 to convert it to 1 based indexing
			long[] L = new long[n+1];
			for(int i = 1; i <= n; i++) {
				C[i] = scn.nextInt(); //Take input for array C
			}
			for(int i = 1; i <= n; i++) {
				L[i] = scn.nextInt(); //Take input for array L
			}
			long mi = Long.MAX_VALUE; // Variable to take care of the minimum cost of petrol upto a checkpoint X
			long ans = 0;
			for(int i=1;i<=n;i++) {
				mi = Math.min(mi, C[i]); // Compare the cose of petrol between cost at current checkpoint and the minimum cost till now
				ans+= (mi*L[i]);
			}
			System.out.println(ans);
		}
	}
}
```
