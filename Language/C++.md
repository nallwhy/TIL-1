# C++ 

## 입출력

### 출력 

```c++
#include <iostream>

int main() {
    int num=20;
    std::cout << "Hello World!" <<std::endl;
    std::cout << "Hello " << "World!" <<std::endl;
    std::cout << num << ' ' << 'A',
    std::cout << ' ' << 3.14 << std::endl;
    return 0;
}
```

* C++에서는 표준 헤더파일의 선언 시 확장자를 생략

### 입력

단일 입력 

```c++
#include <iostream>

int main() {
    int val1;
    std::cout << "First: ";
    std::cin >> val1;
    
    int val2;
    std::cout << "Second: ";
    std::cin >> val2;
    
    int result = val1 + val2;
    std::cout << "Result: " << result << std::endl;
    
    return 0;
}
```

연속 입력

```c++
#include <iostream>

int main() {
    int val1, val2;
    int result = 0;
    std::cout << "Input: ";
    std::cin >> val1 >> val2;
    result = val1 + val2;
    std::cout << "Result: " << val1 + val2 << std::endl;
    
    return 0;
}
```

배열 기반의 문자열 입력

```c++
#include <iostream>

int main() {
    char name[100];
    
    std::cout << "Name: ";
    std::cin >> name;
    
    std::cout << "Your name is " << name << std::endl;
    
    return 0;
}
```

## C++ 특징

- 함수 오버로딩 가능
- 매개변수에 디폴트 값 설정 가능 
	- `int add(int num=6, int num=3) {...}`
	- 함수 선언부에서 표현해야 함

## 인라인 함수

```c++
#include <iostream>

inline int SQUARE(int x) {
    return x * x;
}

int main() {
    std::cout << SQUARE(5) << std::endl;
    return 0;
}
```

## Namespace

```c++
#include <iostream>

namespace BestCom {
    void SimpleFunction() {
        std::cout << "BestCom" << std::endl;
    }
}

namespace ProgCom {
    void SimpleFunction() {
        std::cout << "ProgCom" << std::endl;
    }
}

int main() {
    BestCom::SimpleFunction();
    ProgCom::SimpleFunction();
    return 0;
}
```

using으로 namespace 명시

```c++
#include <iostream>
using namespace std;

int main() {
    int num = 20;
    cout << "Hello World" << endl;
    cout << num << endl;
    return 0;
}
```

namespace 별칭

```c++
#include <iostream>
using namespace std;

namespace AAA {
    namespace BBB {
        namespace CCC {
            int num1;
            int num2;
        }
    }
}

int main() {
    AAA::BBB::CCC::num1 = 20;
    AAA::BBB::CCC::num2 = 30;
    
    namespace ABC = AAA::BBB::CCC;
    cout << ABC::num1 << endl;
    cout << ABC::num2 << endl;
    return 0;
}
```

## 표준 string 클래스

```c++
#include <iostream>
#include <string>
using namespace std;

int main() {
    string str1 = "I like ";
    string str2 = "apple";
    string str3 = str1 + str2;
    
    cout << str1 << endl;
    cout << str2 << endl;
    cout << str3 << endl;
    
    str1 += str2;
    if(str1 == str3) {
        cout << "Same" << endl;
    } else {
        cout << "Not same" << endl;
    }
    
    string str4;
    cout << "Input: ";
    cin >> str4;
    cout << "Result: " << str4 << endl;
    return 0;
}
```

## getline

문자열 입력받기

istream& getline (istream& is, string& str, char delim);
istream& getline (istream& is, string& str);

```c++
#include <iostream>
#include <string>
using namespace std;

int main() {
    string str1;
    cout << "Input: ";
    getline(cin, str1);
    cout << str1 << endl;
    return 0;
}
```

## 형변환

- string to int: atoi(char *) -> atoi(str.c.str());
- string to int: stoi(string)
- int to string: to_string(int)

## vertor

- 동적 배열
- #include <verctor>

**생성자**

```c++
// 빈 컨테이너
vector<int> v1; 
// 기본값으로 초기화된 n개의 원소를 가짐
vector<int> v2(10);  
// x값으로 초기화된 n개의 원소를 가짐
vector<int> v3(10, 1);
// v3의 복사본
vector<int> v4(v3);
// 일반 배열처럼 생성
vector<int> v5 = {1, 2, 3, 4};
```

**메서드**

```c++
vector<int> v = {1, 2, 3, 4, 5, 6};

// 끝에 3추가
v.push_back(3);	//  {1, 2, 3, 4, 5, 6, 3}
// 마지막 원소 제거
v.pop_back(); //  {1, 2, 3, 4, 5, 6}
// 모든 원소 제거
v.clear(); // {}
v.push_back(1); // {1}
// 크기를 n으로 변경하고 확장되는 공간의 값은 기본값으로 초기화함
v.resize(5); // {1, 0, 0, 0, 0}
// 원소 개수
v.size();	// 5
// 비어있는지 확인
v.empty();	// 0
// 첫 번째 원소 참조
v.front();  // 1
// 마지막 원소 참조
v.back(); // 0
```