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

제출 시 예제 출력 형태(줄바꿈 등)을 세심하게 신경써야 함

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

### 4. [A+B - 4](https://www.acmicpc.net/problem/10951)

입력이 몇 개인지 모르는 경우 아래처럼 입력을 받을 수 있음. 이게 가능한 이유는 찾아보니 EOF를 받으면 while문이 종료되기 때문이라고 함 - [stackoverflow](http://stackoverflow.com/questions/5360129/the-question-on-while-cin)

```c++
#include <iostream>

int main() {
    int num1, num2;
    while(std::cin>>num1>>num2) {
        std::cout<<num1+num2<<'\n';
    }
    return 0;
}
```

### 5. [A+B - 5](https://www.acmicpc.net/problem/10952)

```c++
#include<iostream>

int main() {
	int num1, num2;
	while(std::cin>>num1>>num2 && (num1 != 0 && num2 != 0)) {
		std::cout<<num1+num2<<'\n';
	}			
	return 0;
}
```

### 6. [A+B - 6](https://www.acmicpc.net/problem/10953)

```c++
##include <iostream>
#include <string>
using namespace std;

int main() {
    int size;
    cin >> size;
    while(size--) {
        string str1, str2;
        getline(cin, str1, ',');
        getline(cin, str2);
        cout << stoi(str1) + stoi(str2) << endl;
    }
    return 0;
}
```

### 7. [A + B - 7](https://www.acmicpc.net/problem/11021)

```c++
#include <iostream>
using namespace std;

int main() {
    int size;
    cin >> size;
    for(int i=1; i<=size; i++) {
        int num1, num2;
        cin >> num1 >> num2;
        cout << "Case #" << i << ": " << num1 + num2 << endl;
    }
    return 0;
}
```

### 8. [A + B - 8](https://www.acmicpc.net/problem/11022)

```c++
#include <iostream>
using namespace std;

int main() {
    int size;
    cin >> size;
    for(int i=1; i<=size; i++) {
        int num1, num2;
        cin >> num1 >> num2;
        cout << "Case #" << i << ": " << num1 << " + " << num2 << " = " << num1 + num2 << endl;
    }
    return 0;
}
```

## 기타 문제 풀이

### 1. [N찍기](https://www.acmicpc.net/problem/2741)

처음 답안: 시간 초과로 실패

```c++
#include <iostream>
using namespace std;

int main() {
    int size;
    cin >> size;
    for(int i=1; i<=size; i++) {
        cout << i << endl;
    }
    return 0;
}
```

검색해보니 endl는 버퍼를 flush하는 부수적인 효과 때문에 느리다고 함. 그래서 아래처럼 수정하니 성공!

```c++
#include <iostream>
using namespace std;

int main() {
    int size;
    cin >> size;
    for(int i=1; i<=size; i++) {
        cout << i << '\n';
    }
    return 0;
}
```

### 2. [찍기N](https://www.acmicpc.net/problem/2742)

```c++
#include <iostream>
using namespace std;

int main() {
    int size;
    cin >> size;
    for(int i=size; i>0; i--) {
        cout << i << '\n';
    }
    return 0;
}
```

### 3. [구구단](https://www.acmicpc.net/problem/2739)

```c++
#include <iostream>
using namespace std;

int main() {
    int num;
    cin >> num;
    for(int i=1; i<10; i++) {
        cout << num << " * " << i << " = " << num * i << '\n';
    }
    return 0;
}
```

### 4. [2007년](https://www.acmicpc.net/problem/1924)

```c++
#include <iostream>
using namespace std;

int main() {
    char date[][7] = {"SUN", "MON", "TUE", "WED", "THU", "FRI", "SAT"};
    int countOfMonth[12] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
    
    int month, day;
    cin >> month >> day;
    
    int total = 0;
    
    for(int i=0; i<month-1; i++) {
        total += countOfMonth[i];
    }
    total += day;
    cout << date[total%7];
    
    return 0;
}
```

### 5. [합](https://www.acmicpc.net/problem/8393)

```c++
#include <iostream>
using namespace std;

int main() {
    int num;
    int result = 0;
    
    cin >> num;
    for(int i=1; i<=num; i++) {
        result += i;
    }
    cout << result;
    return 0;
}
```

### 6. [최소, 최대](https://www.acmicpc.net/problem/10818)

```c++
#include <iostream>
using namespace std;

int main() {
    int size;
    int min = 0, max = 0;
    cin >> size;
    
    while(size--) {
        int input;
        cin >> input;
        if(min == 0) min = input;
        if(max == 0) max = input;
        
        if(input < min) {
            min = input;
        } else if(input > max) {
            max = input;
        }
    }
    
    cout << min << " " << max;
    
    return 0;
}
```

코드가 더럽다고 생각해서 다른 사람들의 코드를 봤더니 대체로 algorithm 헤더 파일의 sort 메서드를 이용함

```c++
#include <iostream>
#include <algorithm>

using namespace std;

int arr[1000000];

int main() {
    int size;
    cin >> size;
    
    for(int i=0; i<size; i++) {
        cin >> arr[i];
    }
    
    sort(arr, arr + size);
    
    cout << arr[0] << arr[size-1];
    
    return 0;
}
```

다른 방법으로는 나처럼 min, max 초기값을 지정하는게 아니라 문제에서 제시해준 입력의 최소, 최대값으로 초기화 해주었음!

## 별찍기

### 1. [별찍기 - 1](https://www.acmicpc.net/problem/2438)

```c++
#include <iostream>

using namespace std;

int main() {
    int line;
    cin >> line;
    
    for(int i=1; i<=line; i++) {
        for(int j=0; j<i; j++) {
            cout << "*";
        }
        cout << "\n";
    }
    
    return 0;
}
```
### 2. [별찍기 - 2](https://www.acmicpc.net/problem/2439)

```c++
#include <iostream>

using namespace std;

int main() {
    int line;
    cin >> line;
    
    for(int i=1; i<=line; i++) {
        for(int j=line-i; j>0; j--) {
            cout << " ";
        }
        for(int j=0; j<i; j++) {
            cout << "*";
        }
        cout << "\n";
    }
    
    return 0;
}
```

더 깔끔하게 된 코드 

```c++
#include <iostream>

using namespace std;

int main() {
    int line;
    cin >> line;
    
    for(int i=line; i>=1; i--) {
        for(int j=1; j<=line; j++) {
            if(j >= i)
                cout << "*";
            else
                cout << " ";
        }
        cout << "\n";
    }
    
    return 0;
}
```

### 3. [별찍기 - 3](https://www.acmicpc.net/problem/2440)

```c++
#include <iostream>

using namespace std;

int main() {
    int line;
    cin >> line;
    
    for(int i=line; i>=1; i--) {
        for(int j=i; j>=1; j--) {
            cout << "*";
        }
        cout << "\n";
    }
    
    return 0;
}
```

### 4. [별찍기 - 4](https://www.acmicpc.net/problem/2441)

```c++
#include <iostream>

using namespace std;

int main() {
    int line;
    cin >> line;
    
    for(int i=line; i>=1; i--) {
        for(int j=line; j>=1; j--) {
            if(i >= j)
                cout << "*";
            else
                cout << " ";
        }
        cout << "\n";
    }
    
    return 0;
}
```

### 5. [별찍기 - 5](https://www.acmicpc.net/problem/2442)

```c++
#include <iostream>

using namespace std;

int main() {
    int n;
    cin >> n;
    
    for(int line=1; line<=n; line++) {
        for(int space=1; space<=n-line; space++) {
            cout << " ";
        }
        
        for(int star=1; star<=line*2-1; star++) {
            cout << "*";
        }
        
        cout << "\n";
    }
    
    return 0;
}
```

### 6. [별찍기 - 7](https://www.acmicpc.net/problem/2444)

```c++
#include <iostream>

using namespace std;

int main() {
    int n;
    cin >> n;
    
    for(int line=1; line<=n; line++) {
        for(int space=n-line; space>=1; space--) {
            cout << " ";
        }
        for(int star=1; star<=line*2-1; star++) {
            cout << "*";
        }
        cout << "\n";
    }
    
    for(int line=1; line<n*2-1; line++) {
        for(int space=1; space<=line; space++) {
            cout << " ";
        }
        for(int star=(n-line)*2-1; star>=1; star--) {
            cout << "*";
        }
        cout << "\n";
    }
    
    return 0;
}
```

완전 신박했던 풀이 

```c++
#include <iostream>

using namespace std;

int main() {
    int n;
    cin >> n;
    int k = 1;
    
    for(int i=1; i>0; i+=k) {
        for(int j=1; j<=n-i; j++) {
            cout << " ";
        }
        for(int j=1; j<=i*2-1; j++) {
            cout << "*";
        }
        
        cout << "\n";
        if(i == n) {
            k = -1;
        }
    }
    return 0;
}
```