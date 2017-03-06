# 백준 알고리즘 강의 정리

## 시간 복잡도

- 입력의 크기에 따라 걸리는 시간을 나타냄
- 가장 큰 값을 입력했을 때 Big-O가 1억 근처면 1초 정도라고 함 
- 최악의 경우에 걸리는 시간을 나타내야 함

### 1초가 걸리는 입력의 크기

- O(N) - 1억 
- O(NlgN) - 5백만
- O(N^2) - 1만
- O(N^3) - 500 
- O(2^N) - 20
- O(N!) - 10

### 대부분의 시간 복잡도의 의미

- O(1) - 단순 계산 (A+B와 같은 연산, 배열에 접근하는 연산)
- O(lgN) - N개를 절반으로 계속해서 나눔
- O(N) - 1중 for문
- O(NlgN) 
- O(N^2) - 2중 for문
- O(N^3) - 3중 for문
- O(2^N) - 크기가 N인 집합의 부분 집합
- O(N!) - 크기가 N인 순열

## 입출력 문제 풀이

### 1. [Hello World](https://www.acmicpc.net/problem/2557)
	
제출시 어떤 형태로 넣어야하는지 몰라서 삽질했었음

```c++
#include<iostream>

int main() {
    std::cout<<"Hello World!";
    return 0;
}
```

### 2. [A+B](https://www.acmicpc.net/problem/1000)

```c++
#include<iostream>

int main() {
    int num1, num2;
    std::cin>>num1>>num2;
    std::cout<<num1+num2;
    return 0;
}
```

### 3. [A+B - 2](https://www.acmicpc.net/problem/2558)	

```c++
#include<iostream>

int main() {
    int num1, num2;
    std::cin>>num1>>num2;
    std::cout<<num1+num2;
    return 0;
}
```

### 3. [A+B - 3](https://www.acmicpc.net/problem/10950)	

예제 출력 형태(줄바꿈 등)을 세심하게 신경써야 함

```c++
#include<iostream>

int main() {
    int size;
    std::cin>>size;
    while(size--) {
        int num1, num2;
        std::cin>>num1>>num2;
        std::cout<<num1+num2<<'\n';
    }
    return 0;
}
```

