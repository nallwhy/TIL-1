# Divide & Conquer (분할정복) 

## 1. 도입

주어진 문제를 둘 이상의 부분 문제로 나눈 뒤 각 문제에 대한 답을 재귀 호출을 이용해 계산하고, 각 부분 문제의 답으로부터 전체 문제의 답을 계산해냄. 일반적인 재귀 호출과 다른 점은 문제를 한 조각과 나머지 전체로 나누는 대신 거의 같은 크기의 부분 문제로 나눔.

**구성 요소**

- Divide: 문제를 더 작은 문제로 분할하는 과정.
- Merge: 각 문제에 대해 구한 답을 원래 문제에 대한 답으로 병합하는 과정.
- Base case: 더이상 답을 분할하지 않고 곧장 풀 수 있는 매우 작은 문제.

**분할 정복을 적용해 해결할 수 있는 문제의 필요 조건**

- 문제를 둘 이상의 부분 문제로 나누는 자연스러운 방법.
- 부분 문제의 답을 조합해 원래 문제의 답을 계산하는 효율적인 방법.

**장점**

- 같은 작업을 더 빠르게 처리해줌.

### 예제: 수열의 빠른 합과 행렬의 빠른 제곱

**1부터 n까지의 합 구하기**

```
n은 짝수라고 가정
fastSum() = 1 + 2 + ... + n 
		   = (1 + 2 + ... + n/2) + ((n/2 + 1) + ... + n)
```

첫 번째 부분 문제는 fastSum(n/2)로 나타내지고, 두 번째 부분 문제는 `1부터 n가지의 합`꼴로 표현해야 함.

```
(n/2 + 1) + ... + n = (n/2 + 1) + (n/2 + 2) + ... + (n/2 + n/2) 
			      = n/2 * n/2 + (1 + 2 + 3 + ... + n/2) 
			      = n/2 *. n/2 + fastSum(n/2)
```

정리하면,

`fastSum(n) = 2 * fastSum(n/2) + n^2/4 (n이 짝수일 때)`

n이 홀수인 경우에는 짝수인 n-1까지의 합을 재귀 호출로 계산하고 n을 더해 답을 구해야 함.

```c++
int fastSum(int n) {
	// base case
	if(n == 1) return 1;
	if(n % 2 == 1) return fastSum(n-1) + n;
	return 2 * fastSum(n/2) + (n/2) * (n/2);
}
```

### 시간 복잡도 분석

함수가 호출되는 횟수에 비례. fastSum()은 호출될 때마다 최소한 두 번에 한 번 꼴로 n이 절반으로 줄어듬. 그러므로 실행 시간은 `O(lgn)`.

### 행렬의 거듭제곱

행렬의 곱셈의 실행 시간: `O(n^3)`

행렬 A의 거듭 제곱 A^m의 실행 시간: `O(n^3 * m)` 

**분할 정복을 이용해 구현**

```c++
class SquareMatrix;
// n*n 크기의 항등 행렬을 반환하는 함수
SquareMatrix identity(int n);
//A^m 반환
SquareMatrix pow(const SquareMatrix& A, int m) {
	// base case
	if(m == 0) return identity(A.size());
	if(m % 2 > 0) return pow(A, m-1) * A;
	SquareMatrix half = pow(A, m/2);
	return half * half;
}
```

### 나누어 떨어지지 않을 때의 분할과 시간 복잡도

m이 홀수일 때, A^m = A*A^(m-1)로 나누지 않고, 좀더 절반에 가깝게 나누는게 좋겠다고 생각할 수도 있지만, 오히려 A^m를 찾기 위해 계산할 부분 문제 수가 늘어나서 안좋음. 중복으로 계산하는 일이 많아짐. 거의 m-1번을 곱하는 것과 다를바가 없음.

절반으로 나누는 알고리즘이 큰 효율 저하를 불러오는 이유는 바로 여러 번 중복되어 계산되면서 시간을 소모하는 부분 문제들이 있기 때문. 이런 속성을 부분 문제가 중복된다(overlapping subproblems)고 부르며, 후에 동적 계획법이 고안된 계기가 됨.

### 예제: Merge sort and Quick sort

**Merge sort**

- 주어진 수열을 가운데에서 쪼개 비슷한 크기의 수열 두 개로 만든 뒤 이들을 재귀 호출을 이용해 각각 정렬함. 그 후 정렬된 배열을 하나로 합침으로써 정렬된 전체 수열을 얻음.
- 분할 시에는 상수 시간 `O(1)`만에 수행하고, 병합 과정 시에 `O(n)`의 시간이 걸림.

**Quick sort**

- 배열을 단순하게 가운데에서 쪼개는 대신, 병합 과정이 필요 없도록 한쪽의 배열에 포함된 수가 다른 쪽 배열의 수보다 항상 작도록 배열을 분할. 
- 파티션이라고 부르는 단계를 도입. 배열에 있는 수 중 임의의 기준 수를 지정한 후 기준보다 작거나 같은 숫자를 왼쪽, 더 큰 숫자를 오른쪽으로 보내는 과정.
- 분할 시 `O(n)`의 시간이 걸림.

### 시간 복잡도 분석

**Merge sort**

병합 과정이 수행 시간에 영향을 미침. 아래 단계로 내려갈수록 부분 문제의 수는 두 배로 늘고, 각 부분 문제의 크기는 반으로 줄어들기 때문에, 한 단계 내에서 모든 병합에 필요한 총 시간은 O(n)으로 항상 일정. 각 단계의 수에 n을 곱하면 병합 정렬에 필요한 전체 시간을 얻을 수 있는데, 문제 크기는 항상 절반씩 나누어지기 때문에 필요한 단계의 수는 `O(n*lgn)`.

**Quick sort**

파티션 과정이 수행 시간에 영향을 미침. 병합 정렬과 달리 분할된 부분 문제가 비슷한 크기로 나눠지지 않고 부분 문제의 크기가 하나씩만 줄어들 수도 있어 시간 복잡도를 분석하기 까다로움. 이런 최악의 경우의 시간 복잡도는 `O(n^2)`. 평균적으로는 부분 문제가 절반에 가깝게 나눠질 때는 병합 정렬과 같은 `O(n*lgn)`.

### 예제: 카라츠바의 빠른 곱셈 알고리즘

**두 큰 수를 곱하는 O(n^2) 시간 알고리즘**

```c++
// num[]의 자릿수 올림 처리
void normalize(vector<int>& num) {
	num.push_back(0);
	// 자릿수 올림 처리
	for(int i=0; i<num.size(); i++) {
		if(num[i] < 0) {
			int borrow = (abs(num[i]) + 9) / 10;
			num[i+1] -= borrow;
			num[i] += borrow * 10;
		} else {
			num[i+1] += num[i] / 10;
			num[i] %= 10;
		}
	}
	while(num.size() > 1 && num.back() == 0) num.pop_back();
}
// 각 배열에는 각 수의 자릿수가 1의 자리에서부터 시작해 저장되어 있음
// ({3, 2, 1}, {6, 5, 4}) = 123 * 456 = 56088 = {8, 8, 0, 6, 5}
vector<int> multiply(const vector<int> & a, const vector<int>& b) {
	vector<int> c(a.size() + b.size() + 1, 0);
	for(int i=0; i<a.size(); i++)
		for(int j=0; j<b.size(); j++)
			c[i+j] += a[i] * b[j];
	normalize(c);
	return c;	
}
```

**카라츠바의 빠른 곱셈 알고리즘**

1두 수를 각각 절반으로 쪼갬. 만약 a와 b가 각각 10자리 수라면 a1과 b1은 첫 5자리, a0과 b0은 그 다음 5자리를 저장함.

``` 
a = a1 * 10^5 + a0
b = b1 * 10^5 + b0
```

a*b를 네 개의 조각을 이용해 표현함. 

```
a*b = (a1 * 10^5 + a0) * (b1 * 10^5 + b0) 
= a1 * b1 * 10^5 + (a1 * b0 + a0 * b1) * 10^5 + a0 * b0
```

큰 정수 두 개를 한 번 곱하는 대신, 절반 크기로 나눈 작은 조각을 네 번 곱하는데 이럴 경우 여전히 전체 수행 시간이 `O(n^2)`임. 그래서 4번이 아닌 세 번의 곱셈으로 이 값을 계산함.

```
a * b = a1 * b1 * 10^5 + (a1 * b0 + a0 * b1) * 10^5 + a0 * b0

z2 = a1 * b1
z0 = a0 * b0
z1 = a1 * b0 + a0 * b1 = (a0 + a1) * (b0 + b1) - z0 - z2;
```

```c++
// a += b*(10^k)
void addTo(vector<int>& a, const vector<int>& b, int k);
// a -= b 구현. a>=b를 가정
void subFrom(vector<int>& a, const vector<int>& b);
// 두 긴 정수의 곱을 반환
vector<int> karatsuba(const vector<int>& a, const vector<int>& b) {
	int an = a.size(), bn = b.size();
	// a가 b보다 짧을 경우 둘을 바꿈
	if(an < bn) return karatsuba(b, a);
	// base case 1: a나 b가 비어 있는 경우
	if(an == 0 || bn == 0) return vector<int>();
	// base case 2: a가 비교적 짧은 경우 O(n^2) 곱셈으로 변경
	if(an <= 50) return multiply(a,b);
	int half = an / 2;
	// a와 b를 밑에서 half 자리와 나머지로 분리
	vector<int> a0(a.begin(), a, begin() + half);
	vector<int> a1(a.begin() + half, a.end());
	vector<int> b0(b.begin(), b.begin() + min<int>(b.size(), half));
	vector<int> b1(b.begin() + min<int>(b.size, half), b.end());
	// z2 = a1 * b1
	bector<int> z2 = karatsuba(a1, b1);
	// z0 = a0 * b0
	vector<int> z0 = karatsuba(a0, b0);
	// a0 = a0 + a1; b0 = b0 + b1
	addTo(a0, a1, 0); addTo(b0, b1, 0);
	// z1 = (a0 * b0) - z0 - z2
	vector<int> z1 = karatsuba(a0, b0);
	subFrom(z1, z0);
	subFrom(z1, z2);
	// result = z0 + z1 * 10^half + z2 * 10^(half*2)
	vector<int> result;
	addTo(result, z0, 0);
	addTo(result, z1, half);
	addTo(result, z2, half + half);
	return result;
}
```

## 시간 복잡도 분석

`O(n^lg3)`이지만 복잡하기 때문에 입력의 크기가 작은 경우 `O(n^2)` 알고리즘보다 느림.

// todo 나머지 부분은 이해를 못했음..ㅠㅠ