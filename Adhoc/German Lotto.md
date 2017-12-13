## German Lotto
#### Problem Link : [German Lotto](https://hack.codingblocks.com/contests/c/1001/822)

Since the set **S** is sorted in ascending order, we can use 6 nested loops as in the following code to print all possible permutations of Lotto games in lexicographical order.
Each successive loop iterates over the numbers greater than the number chosen in its parent loop maintaining the lexicographical order.

_**Time complexity: ** O(n<sup>6</sup>)_

```C++
#include <iostream>
using namespace std;

int main() {
    
//        freopen("input2.txt","r",stdin);
//        freopen("output2.txt","w",stdout);
        
        int a[50],n;
        cin>>n;
        for(int i=0;i<n;i++)
        	cin>>a[i];
        
        for(int i=0;i<n-5;i++){
            
            for(int j=i+1;j<n-4;j++){
                
                for(int k=j+1;k<n-3;k++){
                    
                    for(int l=k+1;l<n-2;l++){
                        
                        for(int m = l+1;m<n-1;m++){
                            
                            for(int o= m+1;o<n;o++){
                                
                                cout<<a[i]<<" "<<a[j]<<" "<<a[k]<<" "<<a[l]<<" "<<a[m]<<" "<<a[o]<<endl;
                            }
                            
                        }
                        
                    }
                }
            }
        }
        return 0;
        
    
}
```