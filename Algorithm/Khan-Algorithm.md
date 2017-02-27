# 칸 아카데미 알고리즘 강좌 정리

[https://ko.khanacademy.org/computing/computer-science/algorithms](https://ko.khanacademy.org/computing/computer-science/algorithms)

## 숫자 맞추기

### 방법 1: 선형 검색(linear search)

- 차례대로 검색
- 확실하지만 추측 횟수가 너무 많음 
- 전체 개수가 n일 때, 최대 n 추측, 평균 추측 수 n/2

### 방법 2: 이진 검색(binary search)

- 후보 범위를 하나로 좁힐 때까지 찾고자 하는 항목을 포함하고 있는 리스트를 반으로 나누는 과정을 계속 반복함
- 순차적인 항목 리스트에서 원하는 항목을 찾기에 효율적임
- 가장 많이 사용하는 경우는 배열에서 어떤 항목을 찾을 때

## 길 찾기 문제

### 해결방법

1. 목표 네모에서 시작. 목표 지점에서 목표 지점까지는 0단계가 걸리므로 목표 지점에 숫자 0 표시
2. 미로에서 목표 지점에서 정확히 1단계 멀리있는 모든 네모를 찾아서 숫자 1이라고 표시
3. 목표 지점에서 정확히 2단계 떨어져 있는 모든 네모를 찾음. 이 네모들은 1로 표시했던 네모에서 1단계 떨어져 있고, 아직 표시되지 않은 네모임. 이 네모들을 숫자 2로 표시
4. 목표 지점에서 정확히 3단계 떨어져 있는 모든 네모를 찾음. 이 네모들은 2로 표시했던 네모에서 1단계 떨어져 있고, 아직 표시되지 않은 네모임. 이 네모들을 숫자 3로 표시
5. 이런 식으로 목표 지점에서 거리가 커지는 순서대로 미로 안의 네모들을 계속 표시. 숫자 k로 네모를 표시한 후 k로 표시된 네모들에서 한 단계 멀리 있고 아직 표시가 되어 있지 않은 모든 네모들을 숫자 k+1라고 표시함
6. 출발 지점에 네모를 표시하는 순간이 옴. 그러면 출발 지점에서 경로를 따라 네모의 숫자가 항상 줄어드는 방향으로 네모들을 선택하여 목표 지점으로의 경로를 찾아냄

## 이진 검색 

### 의사코드(pseudocode)

1.  min=0, max=n-1
2. guess=min와 max의 평균값(정수이므로 내림)
3. 만약 array[guess]가 target이라면 끝. return guess
4. 만약 array[guess]<target이면 min=guess+1
5. 만약 array[guess]>target이면 max=guess-1
6. 2번으로 돌아감

### array에 target이 없는 경우를 고려하는 의사코드

1. min=0, max=n-1
2. 만약 max<min이면 멈춤. target는 array에 존재하지 않는 것임. return -1
3. guess=min와 max의 평균값(정수이므로 내림)
4. 만약 array[guess]가 target이라면 끝. return guess
5. 만약 array[guess]<target이면 min=guess+1
6. 만약 array[guess]>target이면 max=guess-1
7. 2번으로 돌아감

### javascript 코드

```js
/* Returns either the index of the location in the array,
  or -1 if the array did not contain the targetValue */
var doSearch = function(array, targetValue) {
	var min = 0;
	var max = array.length - 1;
    var guess;
    var count = 0;
    
    while(min <= max) {
        count++;
		guess = Math.floor((min + max) / 2);
		println("guess is " + guess);
		if(array[guess] === targetValue) {
		    println("count is " + count);
			return guess;
		} else if(array[guess] < targetValue) {
			min = guess + 1;
		} else {
			max = guess - 1;
		}
	}

	return -1;
};

var primes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 
		41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97];

var result = doSearch(primes, 73);
println("Found prime at index " + result);

Program.assertEqual(doSearch(primes, 73), 20);
Program.assertEqual(doSearch(primes, 61), 17);
Program.assertEqual(doSearch(primes, 4), -1);
```