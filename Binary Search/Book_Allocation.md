## Read Min Pages
##### Problem Link : [Read Min Pages]()

Similar as Painter's Partion Problem, Refer Youtube Tutorial for explanation.

_**Time Complexity:** O(N*logN)_

**Concept :** Binary Search



```C
#include<iostream>
using namespace std;

bool isValid(int *books,int n,int k,int current_ans){

	int s=1;
	int currently_alloted = 0;

	for(int i=0;i<n;i++){

		if(books[i]>current_ans){
			return false;
		}

		if(currently_alloted + books[i]<=current_ans){
			currently_alloted = currently_alloted + books[i];
		}
		else{
			s++;
			if(s>k){
				return false;
			}
			currently_alloted = books[i];
		}
	}
	return true;



}

int findMinPages(int *books,int n,int k){

	//Search space
	int s = 0; //Book with maximum pages
	int e = 210;//sum of all books

	int mid;
	int ans = INT_MAX;

	while(s<=e){
		//Mid should be considered like this in order to avoid Integer overflow that can be caused.
		// If both s and e are INT_MAX. If we take mid = (s+e)/2 , this will cause overflow and give WA.
		mid = s + (e-s)/2;

		if(isValid(books,n,k,mid)){
			ans = 
			mid;
			e = mid - 1;
		}
		else{
			s = mid + 1;
		}
	}
	return ans;
}

int main(){

	int books[] = {10,20,30,40,60,70};
	int n = 6;
	int k = 3;

	cout<<findMinPages(books,n,k)<<endl;

}
```
