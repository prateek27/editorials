## N-Queens : Backtracking Approach

```c
#include<iostream>
using namespace std;

bool isSafe(int board[20][20],int r,int c,int n){

	//Same Col
	for(int i=0;i<r;i++){
		if(board[i][c]==1){
			return false;
		}
	}
	//Check for Left Diag
	int x = r;
	int y = c;
	while(x>=0 && y>=0){
		if(board[x][y]==1){
			return false;
		}
		x--; y--;
	}

	//Check for Right Diag
	x = r;
	y = c;
	while(x>=0 && y<n){
		if(board[x][y]==1){
			return false;
		}
		x--; y++;
	}
	//Safe Positon
	return true;

}

bool solveNQueen(int n,int board[20][20],int row){
	if(row==n){
		//Print Board
		for(int i=0;i<n;i++){
			for(int j=0;j<n;j++){
				cout<<board[i][j]<<" ";
			}
			cout<<endl;

		}
		cout<<endl;
		return false;
	}

	//Try to place the queen in the current row and call for remaining board
	for(int col=0;col<n;col++){
		if(isSafe(board,row,col,n)){
			
			board[row][col] = 1;

			bool solveHuaKya = solveNQueen(n,board,row+1);
			
			if(solveHuaKya){
				return true;
			}
			//If remaining board is not solved, then you have erase the current queen
			//Backtracking
			board[row][col] = 0;
		}
	}
	//Return False after trying all positions
	return false;



}

int main(){

	int board[20][20] = {0};
	int n;
	cin>>n;
	solveNQueen(n,board,0); 

	return 0;
}

```