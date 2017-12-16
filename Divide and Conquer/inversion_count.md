### Inversion Count

Refer Tutorial for Explanation

```c
#include<iostream>
using namespace std;

int merge(int *a,int s,int e){
	int mid = (s+e)/2;
	int i = s;
	int j = mid+1;
	int k = s;
	int temp[1000];

	int invs = 0;
	
	while(i<=mid && j<=e){
		if(a[i]<=a[j]){
			temp[k++] = a[i++];
		}
		else{
			temp[k++] = a[j++];
			//This is important
			invs += (mid - i + 1);
		}
	}

	while(i<=mid){
		temp[k++] = a[i++];
	}
	while(j<=e){
		temp[k++] = a[j++];
	}
	//Copy the elements to original array a
	for(int i=s;i<=e;i++){
		a[i] = temp[i];
	}
	return invs;
}

int invCount(int *a,int i,int j){
	if(i>=j){
		return 0;
	}
	//Othewise
	int mid = (i+j)/2;
	int ans = invCount(a,i,mid) + invCount(a,mid+1,j) + merge(a,i,j);
	return ans;
}




int main(){

	int a[ ] = {5,3,1,2};
	int n = 4;

	cout<<invCount(a,0,n-1)<<endl;
	return 0;
}

```