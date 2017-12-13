## Sereja and LCM
#### Problem Link : [Sereja and LCM](https://hack.codingblocks.com/contests/c/1001/824)

We have to find the possible number of arrays: A[1], A[2], A[3],…,A[N]
such that A[i] >=1 and A[i] <= M and _**LCM(A[1], A[2], …, A[N])**_ is
divisible by D.
We have to find the sum of the answers with D = L, L+1, …,R modulo
10^9 + 7.
A/Q we have to find the number of array whose LCM is a multiple of a
given number(say **‘x’**).
Using negation **calculate the number of arrays whose LCM is not**
**a multiple of x (say ‘y’)**.
Hence, ans = (possible array with m numbers) - y.
_**Note**_: The maximum value of the array elements can be 1000, the
maximum number of distinct prime factors possible is **4** (2 * 3 * 5 * 7 *
11 > 1000).
Let the prime factors of **x** be **p,q,r,s**
```
x = (p^a) * (q^b) * (r^c) * (s^d)
p^a: P
q^b: Q
r^c: R
s^d: S
```
To calculate y:
<ul>
	<li>None of element of array have any prime factor      that **x** has OR</li>
	<li>it may have some of it missing</li>
</ul>
So, calculate the number of arrays such that either <b>(P or its multiple
are not present) OR (Q or its multiple are not present) OR (R or
its multiple are not present) OR (S or its multiple are not
present).</b>
```
y = |not(P) U not(Q) U not(R) U not(S)|
```
**Applying Principle of Inclusion-Exclusion Principle**
```
A = power(m - m/P,n) + power(m - m/Q,n) + power(m - m/R,n
) + power(m - m/S,n);
B = power(m - m/P - m/Q + m/(P*Q), n) + power(m - m/Q - m
/R + m/(Q*R), n).....
C = power(m- m/P -m/Q - m/R + m/(P*Q) + m/(Q*R) + m/(P*R)
- m/(P*Q*R),n)......
D = powmod(m - m/P - m/Q - m/R - m/S + m/(P*Q) + m/(Q*R)
+ m/(P*R) + m/(R*S) + m/(P*S) + m/(Q*S) - m/(P*Q*R)- m/(Q
*R*S) - m/(P*Q*S) - m/(P*R*S) + m/(P*Q*R*S),n);
```
Final Answer will be:
```
y = A - B + C - D
ans = m ^ n - y
= m ^ n - A + B - C + D
```
**Code:**

```C++
#include<iostream>
#include<stdio.h>
#include<algorithm>
using namespace std;
#define MOD 1000000007LL
typedef long long ll;
int prime[1010],F[1010][5],cnt = 0;
//Fast power method
//Returns a^b mod M in log(b)
ll powmod(ll a,ll b){
	if(!a)
		return 0;
	if(b == 1)
		return a;
	ll res = 1;
	while(b){
		if(b & 1)
			res *= (a % MOD);
		res %= MOD;
		a *= a;
		if(a > MOD)
			a %= MOD;
		b >>= 1;
	}
	return res % MOD;
}
int main(){
	//Prime sieve
	//a[i]-->true indicates that i is prime
	bool a[1010];
	for(int i = 0;i<=1000;i++)
		a[i] = 1;
	for(int i = 2;i*i <= 1000;i++){
		if(a[i]){
			for(int j = 2*i;j<=1000;j+=i)
				a[j] = 0;
		}
	}
	//building the primes array which contains all   
	//the prime numbers upto 1000 in increasing
	// order.
	for(int i = 2;i<=1000;i++){
		if(a[i]){
			prime[cnt++] = i;
		}
	}
	//cnt stores the count of total prime numbers 
	//upto 1000
	for(int i = 0;i<=1000;i++){
		for(int j = 0;j<5;j++){
			F[i][j] = 1;
		}
	}
	//Finding all the prime factors with their power
	//upto 1000
	//F[i][j] = p indicates that j-th prime has a total
	//value of p in factorisation of i
	//Ex: 18 = 2*3*3
	//F[18][0] = 2
	//F[18][1] = 9
	for(int i = 1;i<=1000;i++){
		int num = i,in = 0;
		for(int f = 0;f<cnt;f++){
			int p = 1;
			while((num % prime[f]) == 0){
				p *= prime[f];
				num /= prime[f];
			}
			if(p > 1)
				F[i][in++] = p;
		}
	}
	int t,L,R,m,n;
	scanf("%d",&t);
	while(t--){
		scanf("%d %d %d %d",&n,&m,&L,&R);
		ll res = 0,A = 0,B = 0,C = 0,D = 0;
		int p,q,r,s;
		for(int i = L;i<=R;i++){
			p = F[i][0];
			q = F[i][1];
			r = F[i][2];
			s = F[i][3];

			A = powmod(m - m/p,n) + powmod(m - m/q,n) +   powmod(m - m/r,n) + powmod(m - m/s,n);
			A %= MOD;
			B = powmod(m - m/p - m/q + m/(p*q), n) + powmod(m - m/q - m/r + m/(q*r), n) + powmod(m - m/r - m/s + m/(r*s), n) + powmod(m - m/p - m/r + m/(p*r), n) + powmod(m - m/p - m/s + m/(p*s), n) + powmod(m - m/q - m/s + m/(q*s), n);
			B %= MOD;
			C = powmod(m- m/p -m/q -m/r +m/(p*q)+ m/(q*r)+m/(p*r) - m/(p*q*r),n)+ 
			powmod(m-m/p-m/q-m/s+m/(p*q)+m/(
			p*s)+m/(q*s)-m/(p*q*s),n) + powmod(m-m/s-m/q-m/r+m/(s*q)+
			m/(s*r)+m/(q*r)-m/(s*q*r),n) + powmod(m-m/p-m/r-m/s+m/(
			p*r)+m/(p*s)+m/(r*s)-m/(p*r*s),n);
			C %= MOD;
			D = powmod(m-m/p-m/q-m/r-m/s+m/(p*q)+m/(q*r)+
			m/(p*r)+m/(r*s)+m/(p*s)+m/(q*s) - m/(p*q*r)-m/(q*r*s) - m
			/(p*q*s) - m/(p*r*s) + m/(p*q*r*s),n);
			D %= MOD;
			//cout<<i<<" "<<(powmod(m,n) - A + B - C + D
			+ 2*MOD) % MOD<<endl;
			res += (powmod(m,n) - A + B - C + D + 2*MOD)
			% MOD;
			res %= MOD;
		}
	printf("%lld\n",res);
	}
	return 0;
}
```