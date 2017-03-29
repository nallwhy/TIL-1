# 프로그램 대회에서 사용하는 C, C++, STL

## C++

### cin, cout

scanf와 printf와 같은 입출력 함수. 차이점은 데이터 타입을 포맷 문자열을 통해 명시해 줄 필요 없음. 

scanf와 printf 보다 느림. main 함수에서 cin과 cout을 사용하기 전에 `ios_base::sync_with_stdio(flase);`을 쓰면 빨라지나 printf, scanf와 cin, cout을 섞어 쓸 수 없게됨.

### getline

```c++
string s;
getline(cin, s);
```

한 줄 전체 다를 입력받기 위해 사용하는 함수.

### setprecision

유효숫자 자리수를 지정할 수 있음.

```c++
#include <iomanip>#include <iostream>using namespace std;int main() {	double f = 3.14159265358979;	cout << setprecision(5) << f << '\n'; 	// 3.1415	cout << setprecision(8) << f << '\n';	// 3.1415927	cout << setprecision(10) << f << '\n';	// 3.141592654	return 0;}
```

setprecision(5)이면 6자리에서 반올림을 하는데 보통 문제에서는 소수점 몇째자리까지 출력 혹은 정답과의 오차가 10^-4까지 허용한다라는 식임. 

유효숫자가아닌 소수점 몇째자리까지 출력할 건지 결정하는 것이 필요함.

```c++
#include <iomanip>#include <iostream>using namespace std;int main() {	double f = 3.14159265358979;	cout << fixed << setprecision(5) << f << '\n'; 	// 3.14159	cout << fixed << setprecision(8) << f << '\n';	// 3.14159265	cout << fixed << setprecision(10) << f << '\n';	// 3.1415926536	return 0;}
```

### endl vs '\n'

줄바꿈 시에 endl은 줄바꿈을 출력하고 스트림을 flush 시키는 기능도 포함되어 있어  '\n' 보다 느림.

[연습문제](https://www.acmicpc.net/problem/2741)

```c++
#include <iostream>
using namespace std;
int main() {
    int n;
    cin >> n;
    for(int i=1; i<=n; i++) cout << i << '\n';
    return 0;
}
```

cout보다는 printf가 더 빠름.

## C++11

### auto

- 컴파일러가 타입을 추론해서 타입을 결정함. 
- 선언시 변수의 타입을 명확하게 알 수 있어야 함. 
- 중간에 타입을 바꿀 수 없음.

컴파일 에러가 나는 코드

```c++
auto a,b; // 선언시 명확한 타입을 알 수 없음
cin >> a >> b;
cout << a + b << '\n";
```

올바른 코드

```c++
auto a = 0,b =0 ;
cin >> a >> b;
cout << a + b << '\n";
```

iterator 사용 시 편리함

```c++
// auto를 사용하지 않는 경우
map<pair<int, int>, vector<pair<int, string>>> d;
for(map<pair<int, int>, vector<pair<int, string>>>::iterator it = d.begin(); it != d.end(); it++) { ... }
// 위와 같은 코드를 아래처럼 줄일 수 있음
map<pair<int, int>, vector<pair<int, string>>> d;
for(auto it = d.begin(); it != d.end(); it++) { ... }
```

### Range-based for

다른 언어의 for each문에 해당하는 문법.

```c++
vector<int> a = {1, 2, 3, 4, 5};
for(int i=0; i<a.size(); i++) {
	cout << a[i] << ' ';
}
cout << '\n';

for(int x : a) {
	cout << x << ' ';
}
cout << '\n';
```

& 연산자를 사용하여 값 복사가 아닌 참조로 동작하는 방법. vector가 엄청 큰 경우 for를 돌리는 것보다 값 복사하는 시간이 포함되어 느려짐.

```c++
vector<pair<int, int>> a = {{1, 2}, {3, 4}, {5, 6}};
for(int i=0; i<a.size(); i++) {
	cout << a[i].first + a[i].second << ' ';
}
cout << '\n';

for(auto &p : a) {
	cout << p.first + p.second << ' ';
}
cout << '\n';
```

초기화 리스트를 이용하는 경우.

```c++
int sum = 0;
for(auto x : {1, 2, 3, 4}) {
	sum += x;
}
cout << "sum = " << sum << '\n';
```

배열을 이용하는 경우.

```c++
int a[] = {1, 2, 3, 4, 5};
sum = 0;
for (auto x : a) {
	sum += x;
}
cout << "sum = " << sum << '\n';
```

문자열을 출력하는 경우.

```c++
const char cstr[] = "string";
sum = 0;
for(auto x : cstr) {
	sum += 1;
}
cout << "sum = " << sum << '\n';	// 배열 마지막에 있는 널 문자 때문에 7출력

string str = "string";
sum = 0;
for(auto x : str) {
	sum += 1;
}
cout << "sum = " << sum << '\n';	// 6출력
```

### 초기화 리스트

```c++
vector<int> a;
a.push_back(1);
a.push_back(3);
a.push_back(5);

vector<int> a = {1, 3, 5};	// 배열처럼 초기화하면서 선언 가능
```

위처럼 선언하는 것은 STL 컨테이너에서만 적용되는게 아니라 직접 만든 구조체에도 순서만 맞으면 가능함. 추가로 여러가지 인터페이스에서도 가능.

```c++
struct Person {
	string name;
	int age;
}

set<int> s = {1, 2, 3, 4, 5};
map<int, string> m = { {20, "a"}, {10, "hi"} };
Person p = {"you", 20};
map<int, vector<pair<int, int>>> m2 = {
	{10, {{1, 2}, {3, 4}}},
	{20, {{5, 6}, {7, 8}, {9, 10}}}
};
```

### 람다 함수

구성: [캡쳐](함수 인자) {함수 내용}

```c++
int sum(int x, int y) {
	return x+y;
}
cout << sum(1, 2) << '\n';

cout << [](int x, int y) {
	return x+y;
}(1, 2) << '\n';
```

람다 함수도 변수로 만들 수 있음.

```c++
auto sum2 = [](int x, int y) {
	return x+y;
};
cout << sum2(1, 2) << '\n';
```

[연습문제](https://www.acmicpc.net/problem/2555)

```c++
#include <iostream>
using namespace std;
int main() {
    auto print = [] {
        cout << "10/14" << '\n';
    };
    print();
    return 0;
}
```

**캡쳐**

람다 함수의 스코프를 결정함.

[연습문제](https://www.acmicpc.net/problem/10871)

```c++
int n, y;
cin >> n >> x;
auto is_less = [&] (int number) {
// 함수 밖에 선언되어 있는 변수를 사용하고 싶을 때 캡쳐에 &를 넣어줌
	return number < x; 
};
```

```c++
#include <iostream>
using namespace std;
int main() {
    int n, x;
    cin >> n >> x;
    auto is_less = [&](int n) {
        return n < x;
    };
    for(int i=0; i<n; i++) {
        int a;
        cin >> a;
        if(is_less(a)) {
            cout << a << ' ';
        }
    }
    return 0;
}
```

- 캡쳐에 &을 넣으면 선언하는 시점에서 바깥에 있는 변수를 모두 사용가능.
- &x와 같이 어떤 변수를 사용할 것인지도 적을 수 있음.
- &는 참조고, =는 값 복사임.
- 여러 개는 ,를 이용할 수 있음.

```c++
int x = 10;
int y = 20;

auto f = [&x, y]() {
	x += 10;
	// y += 10;	// 주석 해제시 컴파일 에러. auto f = [&x, y]() mutable { 로 선언하면 컴파일은 되지만 실행 결과는 동일함.
};

cout << "x = " << x << ", y = " << y << '\n'' // x = 10, y = 20 출력
f();
cout << "x = " << x << ", y = " << y << '\n'' // x = 20, y = 20 출력
f();
cout << "x = " << x << ", y = " << y << '\n'' // x = 30, y = 20 출력
```

**변수 타입**

함수의 변수 타입은 #include <functional>에 선언되어 있음. function<리턴타입(콤마로 구분한 인자의 타입들)>을 적어주면 됨.

```c++
function<void()> print = [] {};
function<void(int)> print2 = [] (int x) {};
function<int(int, int)> sum = [] (int x, int y) {
	return x+y;
};
```

[연습문제](https://www.acmicpc.net/problem/10807)

재귀함수 호출시에는 auto를 사용할 수 없음.

```c++
#include <iostream>
#include <functional>
using namespace std;
int main() {
    int n;
    cin >> n;

// f 함수가 함수 밖에 있으므로 캡쳐에 &를 넣어줘야 함
    function<int(int)> f = [&](int n) {
        if(n <= 1) return n;
        else return f(n-1) + f(n-2);
    };
    
    cout << f(n) << '\n';
    return 0;
}
```

[연습문제2](https://www.acmicpc.net/problem/10869)

```c++
vector<function<int(int, int)>> d;
d.push_back([](int x, int y) {
	return x+y;
});
d.push_back([](int x, int y) {
	return x-y;
});
d.push_back([](int x, int y) {
	return x*y;
});
d.push_back([](int x, int y) {
	return x/y;
});
d.push_back([](int x, int y) {
	return x%y;
});

for(auto &f : d) {
	cout << f(a, b) << '\n';
}
```

## STL Container

### STL(Standard Template Library) 구성

- 알고리즘 (주로 사용하는 알고리즘들이 있음)
- 컨테이터 (자료를 저장하는 컨테이터)
- 함수
- 이터레이터

### pair

pair를 사용하면 두 자료형 T1과 T2를 묶을 수 있음. 항상 두 개를 묶으며, 첫 번째 자료는 first, 두 번째 자료는 second로 접근할 수 있음.

`#include <utility>`에 있으나, algorithm, vector와 같은 헤더파일에서 이미 include하고 있기에 따로 할 필요는 없음.

make_pair를 이용하거나, 생성자를 이용해서 만들 수 있음.

pair 선언 방법

```c++
pair<int, int> p1;
cout << p1.first <<  '  ' << p1.second << '\n'; // 초기값으로 각각 0이 들어가있음

p1 = make_pair(10, 20);
cout << p1.first <<  '  ' << p1.second << '\n'; // 10 20 출력

p1 = pair<int, int>(30, 40);
cout << p1.first <<  '  ' << p1.second << '\n'; // 30 40 출력

pair<int, int> p2 (50, 60);
cout << p2.first <<  '  ' << p2.second << '\n'; // 50 60 출력
```

항상 두 가지만 저장할 수 있기 때문에 네 가지를 저장하고 싶으면 pair에 pair를 만드는 방법을 사용할 수 있음.

```c++
pair<pair<int, int>, pair<int, int>> p = make_pair(make_pair(10, 20), make_pair(30, 40));

cout << p.first.first << ' ' << p.first.second << ' ';
cout << p.second.first << ' ' << p.second.second << ' ';
// 10 20 30 40 출력
```

하지만 이렇게 사용하는 경우 헷갈리기 쉽기에 아래의 컨테이너를 사용함.

### tuple

- pair와 비슷하지만 여러 개를 묶을 수 있음.
- get을 이용해서 인덱스로 접근해야 함.
- #include <tuple>에 정의되어 있음.

```c++
tuple<int, int, int> t1 = make_tuple(1, 2, 3);

cout << get<0>(t1) << ' ';
cout << get<1>(t1) << ' ';
cout << get<2>(t1) << '\n';
// cout << get<i>(t1) << '\n'; 이런식으로 get 함수 안에 변수를 넣을 수 없음.
```

### tie

변수를 묶어두는 형태를 만들어줌. pair에도 사용할 수 있음.

```c++
auto t = make_tuple(10, 20, 30);

int x = get<0>(t);
int y = get<1>(t);
int z = get<2>(t);

cout << x << ' ' << y << ' ' << z << '\n';	// 10 20 30 출력

x = y = z = 0;

tie(x, y, z) = t; // 순서대로 각 변수에 값이 들어감
cout << x << ' ' << y << ' ' << z << '\n';	// 10 20 30 출력
```

변수값을 무시해야 하는 경우에는 ignore를 사용.

```c++
auto t = make_tuple(1, 2, 3);

int x, y;
tie(x, y, ignore) = t;
cout << x << ' ' << y << '\n';  // 1 2 출력
```

tie를 이용하면 swap 함수를 완전 간단히 구현할 수 있음.

```c++
tie(a, b) = make_pair(b, a);
```

### vector

- 길이를 변경할 수 있는 배열.
- #include<vector>에 정의되어 있음.

```c++
vector<int> v1; // 길이가 0
vector<int> v2(10); // 길이가 10
vector<int> v3(15, 1); // 1이 15개 있는 형태
vector<int> v4 = {1, 2, 3, 4, 5}; // 초기화 리스트를 이용한 선언 방법
```

vector 안에 vector나 pair를 넣을 수 있음.

```c++
vector<pair<int, int>> v5;
vector<pair<int, int>> v6 = {{1, 2}, {3, 4}};
vector<vector<int>> v7;

int n = 10, m = 20;
vector<vector<int>> v8(n, vector<int>(m)); // int v8[n][m] 와 동일
```

vector의 값 변경하기.

```c++
vector<int> a = {1,2, 3, 4, 5};
a.push_back(6); // [1, 2, 3, 4, 5, 6]
a. pop_back(); // [1, 2, 3, 4, 5]

a.clear(); // []
a.resize(5); // [0, 0, 0, 0, 0]

a.clear(); // []
a.push_back(1); // [1]
a.push_back(2); // [1, 2]

a.resize(5); // [1, 2, 0, 0, 0]
a.resize(3); // [1, 2, 0]
```

vector 크기 구하기.

```c++
vector<int> a = {1, 2, 3, 4};
cout << "size = " << a.size() << '\n'; // 4 출력
cout << "empty = " << a.empty() << '\n'; // 0 출력
```

vector 안의 값 가져오기.

```c++
vector<int> a = {1, 2, 3};
cout << "front = " << a.front() << '\n'; // 1 출력
cout << "a[1] = " << a[1] << '\n'; // 2 출력
cout << "back = " << a.back() << '\n'; // 3 출력
for(int &x : a) {
	cout << x << ' '; // 1 2 3 출력
}

for(vector<int>::iterator it = a.begin(); it != a.end(); it++) {
	cout << *it << ' '; 	// 1, 2, 3 출력 
	// iterator는 포인터와 비슷하기에 * 연산자 사용
}

for(auto it = a.begin(); it != a.end(); it++) {
	cout << "a[" << (it - a.begin()) << "] = " << *it << '\n';
	// 포인터와 비슷함으로 빼는 것으로 인덱스 구할 수 있음
}
```

pair나 tuple이 들어가 있는 경우.

```c++
vector<pair<int, int>> a;
a.emplace_back(1, 2);
a.push_back({3, 4});
a.push_back(make_pair(5, 6));

for(auto &x : a) {
	cout << x.first << ' ' << x.second << '\n';
}

for(auto &x : a) {
	cout << x.first << ' ' << x.second << '\n'; 
}

for(auto it = a.begin(); it != a.end(); it++) {
	cout << it->first << ' ' << it->second << '\n';
}
```

vector 중간에 값을 삽입하기.

```c++
vector<int> a = {1, 2, 3};  // [1, 2, 3]

auto it = a.begin();
a.insert(it, 4); // [4, 1, 2, 3]

it = a.begin() + 1;
a.insert(it, 5, 0); // [4, 0, 0, 0, 0, 0, 1, 2, 3]

it = a.begin() + 2;
vector<int> b = {10, 20};
a.insert(it, b.begin, b.end); // [4, 0, 10, 20, 0, 0, 0, 0, 1, 2, 3]
```

중간 삽입 시간은 배열이기 때문에 삽입하는 위치보다 뒤에 있는 원소들을 삽입하는 개수만큼 뒤로 밀어야하기 때문에 `O(n)`임.

vector 중간값을 삭제하기.

```c++
vector<int> a = {1, 2, 3, 4, 5}; // [1, 2, 3, 4, 5]
a.erase(a.begin() + 2); // [1, 2, 4, 5]
a.erase(a.begin() + 1, a.begin() + 3); // [1, 5]
```

중간 삭제 시간은 배열이기 때문에 뒤에 있는 원소들을 앞으로 옮겨야하기 때문에 `O(n)`임.

### deque

- queue가 두 개 붙어있는 형태. (double ended queue)
- queue와 달리 양쪽에서 push와 pop이 일어남.

```c++
deque<int> d;
d.push_back(1); // [1]
d.push_front(2); // [2, 1]
d.push_back(3); // [2, 1, 3]
d.pop_back(); // [2, 1]
d.pop_front(); // [1]
```

### list

- 이중 연결 리스트. 
- 프로그래밍 대회에서는 잘 사용하지 않음.
- 정렬시 algorithms에 들어있는게 아니라 list에 들어있는 내장 함수를 이용해야함.

```c++
list<int> l = {2, 1, -5, 4, -3, 6, -7};
l.sort(); // [-7, -5, -3, 1, 2, 4, 6]
l.sort(); // 순서를 바꿔줌 [6, 4, 2, 1, -3, -5, -7]
l.sort([](int &u, int &v) {
	return abs(u) < abs(v);
}); // [1, 2, -3, 4, -5, 6, -7]
l.reverse(); // 뒤집어줌 [-7, 6, -5, 4, -3, 2, 1]
```

list는 vector와 deque랑 sort나 reverse를 제외하고는 거의 같은 함수를 사용함.

list는 중간 insert, erase 시 `O(1)`이라는 시간이 걸리므로 중간 삽입, 삭제가 많을 경우 가장 효율적인 자료구조임.

[연습문제](https://www.acmicpc.net/problem/2346)

```c++
#include <iostream>
#include <list>
using namespace std;
int main() {
    int n;
    list<pair<int, int>> a;
    cin >> n;
    
    for(int i=1; i<=n; i++) {
        int x;
        cin >> x;
        a.push_back({x, i});
    }
    
    auto it = a.begin();
    for(int i=0; i<n-1; i++) {
        cout << (it->second) << ' ';
        int step = it->first;
        if(step > 0) {
            auto temp = it;
            ++temp;
            if(temp == a.end()) {
                temp = a.begin();
            }
            a.erase(it);
            it = temp;
            for(int i=1; i<step; i++) {
                ++it;
                if(it == a.end()) {
                    it = a.begin();
                }
            }
        } else if(step < 0) {
            step = -step;
            auto temp = it;
            if(temp == a.begin()) {
                temp = a.end();
            }
            --temp;
            a.erase(it);
            it = temp;
            for(int i=1; i<step; i++) {
                if(it == a.begin()) {
                    it = a.end();
                }
                --it;
            }
        }
    }
    
    cout << a.front().second << '\n';
    return 0;
}
```

[연습문제2](https://www.acmicpc.net/problem/1406)

```c++
#include <iostream>
#include <list>
#include <string>
using namespace std;
int main() {
    string s;
    cin >> s;
    list<char> editor(s.begin(), s.end());
    auto cursor = editor.end();
    int n;
    cin >> n;
    while(n--) {
        char cmd;
        cin >> cmd;
        if(cmd == 'L') {
            if(cursor != editor.begin()) {
                --cursor;
            }
        } else if (cmd == 'D') {
            if (cursor != editor.end()) {
                ++cursor;
            }
        } else if (cmd == 'B') {
            if (cursor != editor.begin()) {
                auto temp = cursor;
                --cursor;
                editor.erase(cursor);
                cursor = temp;
            }
        } else if (cmd == 'P') {
            char c;
            cin >> c;
            editor.insert(cursor, c);
        }
    }
    
    for (char c : editor) {
        cout << c;
    }
    cout << '\n';
    
    return 0;
}
```

list이기 때문에 커서 이동 시 걸리는 시간이 `O(1)`임. vector를 사용하면 `O(n)` 시간이 소요됨.

vector, list, deque는 순서가 있는 순차 자료구조임. set, map은 연관 컨테이너임.

### set

- vector, list 등과 다르게 순서가 없음.
- BST(Binary search tree)로 구현되어 있음. (Red Black Tree).
- 어떤 집합을 나타날 때 효율적인 자료구조.

집합은 순서가 없고 중복된 값을 저장 하지 않음.

```c++
set<int> s1;
set<int> s2 = {1, 2, 3, 4, 5};
set<int> s3 = {1, 1, 1, 1, 2, 2, 3, 3, 3, 3};

cout << "s1.size() = " << s1.size() << '\n' // 0출력
cout << "s2.size() = " << s2.size() << '\n' // 5출력
cout << "s3.size() = " << s3.size() << '\n' // 3출력

set<int, greater<int>> s4; 
```

set은 트리의 형태로 정렬되어 있음(오름차순). greater을 이용해 내림차순으로 정렬할 수 있음.

```c++
set<int> s;
s.insert(1);
s.insert(3);
s.insert(2);
cout << "s.size() = " << s.size() << '\n'; // 3 출력

// insert()는 <삽입한 위치, 삽입 성공여부>를 리턴함
// set에는 중복된 값이 저장되지 않기 때문에 성공여부를 리턴함
pair<set<int>::iterator, bool> result = s.insert(4);
cout << "result iterator = " << *result.first << '\n'; // 4 출력
cout << "result bool = " << *result.second << '\n'; // 1 출력

auto result 2 = s.insert(3);
cout << "result2 iterator = " << *result2.first << '\n'; // 3 출력
cout << "result2 bool = " << *result2.second << '\n'; // 0 출력
```

set은 BST이므로 삽입/탐색/삭제 모두 `O(lgn)` 시간이 걸림.

```c++
set<int> s = {1, 2, 3, 4, 5};
s.erase(s.begin()); [2, 3, 4, 5]
cout << "s.size() = " << s.size() << '\n'; // 4 출력

// set은 순서가 없어 []와 인덱스를 통해 접근할 수 없음 
auto it = s.begin();
it++;
cout << "*it = " << *it << '\n'; // 3 출력
it = s.erase(it); // [2, 4, 5]
cout << "*it = " << *it << '\n'; // 4 출력
cout << "s.size() = " << s.size() << '\n'; // 3 출력
```

set 순회하기. (for문을 이용하면 오름차순으로)

```c++
set<int> s = {5, 2, 4, 1, 3, 7, 6};
for(auto it = s.begin(); it != s.end(); it++) {
	cout << *it << ' '; // 1 2 3 4 5 6 7 출력
}
cout << '\n';
// 총 O(NlgN) 시간

for(auto x : s) {
	cout << x << ' ';
}
// 총 O(NlgN) 시간
```

[연습문제](https://www.acmicpc.net/problem/10867)

```c++
#include <iostream>
#include <set>
using namespace std;
int main() {
    set<int> a;
    int n;
    cin >> n;
    while(n--) {
        int x;
        cin >> x;
        a.insert(x);
    }
    
    for(auto x : a) {
        cout << x << ' ';
    }
    return 0;
}
```

특정 값 포함 여부 알아내기. 시간은 `O(lnN)`

```c++
void print(set<int> &s, set<int>::iterator it) {
	// find에서 값이 없는 경우 end를 리턴함
	if(it == s.end()) {
		cout << "end\n";
	} else {
		cout << *it << '\n';
	}
}
set<int> s = {7, 5, 3, 1};

auto it = s.find(1); // set<int>::iterator 타입이 리턴
print(s, it); // 1 출력

it = s.find(2);
print(s, it); // end 출력

it = s.find(3);
print(s, it); // 3 출력

it = s.find(4);
print(s, it); // end 출력
```

특정 값 포함 여부 알아낼 때 count를 이용하면 더 간단하게 리턴값이 0, 1로 옴.

```c++
set<int> s = {7, 1, 5, 3};

for(int i=1; i<=9; i++) {
	cout << "s.count(" << i << ") = " << s.count(i) << '\n';
}
```

[연습문제](https://www.acmicpc.net/problem/10815)

```c++
#include <iostream>
#include <set>
using namespace std;
int main() {
    set<int> card;
    int n;
    cin >> n;
    while(n--) {
        int x;
        cin >> x;
        card.insert(x);
    }
    int m;
    cin >> m;
    while(m--) {
        int x;
        cin >> x;
        cout << card.count(x) << ' ';
    }
    return 0;
}
```

로 하니까 시간 초과.. ㅠㅠ 그래서 iostream 말고 cstdio로 바꿔서 구현해보니 성공! 이쯤되면 왜 C++로 해야하는지 점점 의문..ㅎ...
 
```c++
 #include <cstdio>
#include <set>
using namespace std;
int main() {
    set<int> card;
    int n;
    scanf("%d", &n);
    while(n--) {
        int x;
        scanf("%d", &x);
        card.insert(x);
    }
    int m;
    scanf("%d", &m);
    while(m--) {
        int x;
        scanf("%d", &x);
        printf("%d ", card.count(x));
    }
    return 0;
}
```

**multiset**

set과 완벽히 똑같은데, 같은 수를 여러 개 저장할 수 있음.

[연습문제](https://www.acmicpc.net/problem/10816)

// todo 아래 코드로는 시간 초과남. 다른 방법 찾아야 함.

```c++
#include <cstdio>
#include <set>
using namespace std;
int main() {
    multiset<int> card;
    int n;
    scanf("%d", &n);
    while(n--) {
        int x;
        scanf("%d", &x);
        card.insert(x);
    }
    int m;
    scanf("%d", &m);
    while(m--) {
        int x;
        scanf("%d", &x);
        printf("%d ", card.count(x));
    }
    return 0;
}
```

### map

- key와 value로 이루어진 자료구조.
- BST로 이루어지므로 접근은 `O(lgN)` 시간이 걸림.
- 배열처럼 사용할 수 있는데 []안에 꼭 정수가 아니여도 된다는 장점.

```c++
map<int, int> d1;
map<int, int> d2 = {{1, 2}, {3, 4}, {5, 6}}; 
// d2[1] = 2, d2[3] = 4 ...

cout << "d1.size() = " << d1.size() << '\n'; // 0 출력
cout << "d2.size() = " << d2.size() << '\n'; // 3 출력

d1[10] = 20;
cout << "d1[10] = " << d1[10] << '\n'; // 20 출력
cout << "d2[1] = " << d2[1] << '\n'; // 2 출력
cout << "d2[2] = " << d2[2] << '\n'; // 0 출력
cout << "d2[3] = " << d2[3] << '\n'; // 4 출력
cout << "d2[4] = " << d2[4] << '\n'; // 0 출력
cout << "d2[5] = " << d2[5] << '\n'; // 6 출력
```

map 사용시 주의점..

```c++
map<int, int> d1;
map<int, int> d2;

for(int i=0; i<=9; i+=2) {
	d1[i] = i*i;
	d2[i] = i*i;
}

cout << "d1.size() = " << d1.size() << '\n'; // 5 출력
cout << "d2.size() = " << d2.size() << '\n'; // 5 출력

// map은 없는 것에 접근하면 만들어버림
for(int i=0; i<=10; i++) {
	if(d1[i]) {
		cout << i << ' ';
	}
}
cout << '\n';
for(int i=1; i<=10; i++) {
	if(d2.count(i)) {
		cout << i << ' ';
	}
}
cout << '\n';

cout << "d1.size() = " << d1.size() << '\n'; // 10 출력
cout << "d2.size() = " << d2.size() << '\n'; // 5 출력
```

pair처럼 접근 가능.

```c++
map<int, int> d = {{1, 2}, {3, 4}, {5, 6}};

for(auto it = d.begin(); it != d.end(); it++) {
	cout << (it->first) << ' ' << (it->second) << '\n';
}

for(auto p : d) {
	cout << p.first << ' ' << p.second << '\n';
}
```

[연습문제](https://www.acmicpc.net/problem/1076)

```c++
#include <iostream>
#include <map>
#include <string>
using namespace std;
int main() {
    map<string, int> d = {
        {"black", 0}, {"brown", 1}, {"red", 2},
        {"orange", 3}, {"yellow", 4}, {"green", 5},
        {"blue", 6}, {"violet", 7}, {"grey", 8},
        {"white", 9}
    };
    
    string a, b, c;
    cin >> a >> b >> c;
    
    long long ans = (long long)(d[a]*10 + d[b]);
    for (int k=0; k<d[c]; k++) {
        ans *= 10LL;
    }
    
    cout << ans << '\n';
    return 0;
}
```

[연습문제2](https://www.acmicpc.net/problem/1764)

### stack

vector나 deque 등을 이용해서 새롭게 만든 배열이므로 컨테이너 어댑터라 함. 

```c++
stack<int> s1; // deque으로 구현
stack<int, list<int>> s2; // 원하는 자료형을 뒤에 써주면 됨

deque<int> d = {1, 2, 3, 4, 5};
stack<int> s3(d); [1, 2, 3, 4, 5]
```

stack 연산.

```c++
stack<int> s;
for(int i=0; i<=5; i++) {
	s.push(i); // [1, 2, 3, 4, 5]
}

for(int i=0; i<2; i++) {
	cout << s.top() <, '\n'; // 5랑 4가 순차적으로 출력
	s.pop();
}

cout << "size = " << s.size() << '\n'; // 3 출력

for(int i=10; i>=6; i--) {
	s.push(i); // [1, 2, 3, 10, 9, 8, 7, 6]
}

cout << "size = " << s.size() << '\n'; // 8 출력
cout << "empty = " << s.empty() << '\n'; // 0 출력

while(!s.empty()) {
	cout << s.top() <, '\n'; // 6 7 8 9 10 3 2 1 출력
	s.pop();
}

cout << "size = " << s.size() << '\n'; // 0 출력
cout << "empty = " << s.empty() << '\n'; // 1 출력
```

pair를 넣을 때 emplace 사용.

```c++
stack<pair<int, int>> s;
s.push(make_pair(1, 2));
s.push({3, 4});
s.emplace(5, 6); // push와 pop 밖에 없어 push_back 같은게 필요 없음

while(!s.empty()) {
	auto p = s.top();
	cout << p.first << ' ' << p.second << '\n';
	s.pop();
}
```

### queue

선언하는 방법.

```c++
queue<int> q1;
queue<int, list<int>> p2;

deque<int> d {1, 2, 3, 4, 5};
queue<int> p3(d);
```

queue 연산.

```c++
queue<int> q;

for(int i=1; i<=5; i++) {
	q.push(i); // [1, 2, 3, 4, 5]
}

for(int i=0; i<2; i++) {
	cout << q.front() << ' ' << q.back() << '\n'; // 1 5 다음에 2 5 출력
	q.pop(); 
}

for(int i=6; i<=10; i++) {
	q.push(i);
	cout << "back = " << q.back() << '\n'; [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
}

while(!q.empty()) {
	cout << q.front() << ' ' << q.back() << '\n';
	q.pop();
}
```

emplace 이용하여 pair 추가.

```c++
queue<pair<int, int>> q;
q.push(make_pair(1,2));
q.push({3, 4});
q.emplace(5, 6);

while(!q.empty()) {
	auto p = q.front();
	cout << p.first << ' ' << p.second << '\n';
	q.pop();
}
```

### priority_queue

- 기존 queue는 선입선출인데 priority_queue은 우선순위, 크기가 큰 것부터 나옴.
- 최대 힙이 priority_queue임.

```c++
vector<int> a = {5, 2, 4, 1, 3};
priority_queue<int> q1;

for(int x : a) {
	q1.push(x);
}

while(!q1.empty()) {
	cout << q1.top() << ' '; // 5 4 3 2 1 순으로 출력
	q1.pop();
}
```

최대 힙을 최소 힙으로 바꾸고 싶을 때. 

```c++
vector<int> a = {5, 3, 4, 1, 2};
priority_queue<int> q1;

for(int x : a) {
	q1.push(-x); // [-5, -2, -4, -1, -3]
}

while(!q1.empty()) {
	cout << -q1.top() << ' '; // 1 2 3 4 5 순으로 출력
	q1.pop();
}
```

이 방법을 사용할 때에는 int의 최소값인 -2^31(=-2147483648) 일 때 원래 `if(x == -x)`가 절대 참이 될 수 없는데 이 경우에는 참이 됨. 음수를 붙여도 자기자신이 됨. 그러므로 수의 범위를 꼭 확인해야함.

greater를 이용해 최소 힙으로 나타내는 방법.

```c++
vector<int> a = {5, 2, 4, 1, 3};
priority_queue<int, vector<int>, greater<int>> q; 
for(int x : a) {
	q.push(x);
}
while(!q3.empty()) {
	cout << q3.top() << ' ';
	q.pop();
}
```

최대힙임을 보이기.

```c++
proiority_queue<int> q;
for(int x : {2, 4, 1, 3, 5}) {
	cout << "x = " << x << '\n';
	q.push(x);
	cout << "top = " << q.top() << '\n'; // 2 2 4 4 5 순으로 출력
}

while(!q.empty()) {
	cout << "top = " << q.top() << '\n'; // 5 4 3 2 1 순으로 출력
	q.pop();
}
```

### bitset

- vector<bool> 형태를 생각하면 됨.
- bool은 사실 1bit가 아닌 1byte를 사용하는데 bitset은 1bit만 사용함.
- and, or, not 비트 연산자 사용 가능.

```c++
bitset<8> b1; // 0, 0, 0, 0, 0, 0, 0
bitset<10> b2(88); // 0, 0, 0, 1, 0, 1, 1, 0, 0, 0
bitset<8> b3("10110"); // 0, 0, 0, 1, 0, 1, 1, 0
```

bitset도 컨테이너임으로 [] 사용 가능.

```c++
bitset<10> b(88); // 0, 0, 0, 1, 0, 1, 1, 0, 0, 0
for(int i=0; i<b.size(); i++) {
	cout << i << ": " << b[i] << '\n';
}
```

[]로 접근하는 것과 같은 test 함수.

```c++
bitset<10> b(88); // 0, 0, 0, 1, 0, 1, 1, 0, 0, 0
cout << b << '\n'; // 0001011000

cout << b.test(4) << '\n'; // 1
cout << b.test(5) << '\n'; // 0
```

set, reset, flip

```c++
b.set(0); // 0번째 위치를 1로 바꿈
cout << b << '\n'; // 0001011001

b.reset(3); // 3번째 위치를 0로 바꿈
cout << b << '\n'; // 0001010001

b.flip(2); // 2번째 위치를 0을 1로, 1을 0으로 바꿈
cout << b << '\n'; // 0001010101

bitset<10> b2(88); // 0, 0, 0, 1, 0, 1, 1, 0, 0, 0
cout << b << '\n'; // 0001011000

// 인자가 없는 경우 전체에 반영
b.flip(); 
cout << b << '\n'; // 1110100111

b.set();
cout << b << '\n'; // 1111111111

b.reset();
cout << b << '\n'; // 0000000000
```

bitset 연산.

```c++
bitset<10> b(88); // 0, 0, 0, 1, 0, 1, 1, 0, 0, 0
cout << b << '\n'; // 0001011000

cout << b.all() << '\n'; // false // 모든 bit가 1인지
cout << b.any() << '\n'; // true // bit가 1인 것이 1개 이상 인지
cout << b.none() << '\n'; // false // 모든 bit가 0인지
cout << b.count() << '\n'; // 3  // 1인 bit 개수
```

2진수와 비슷해 2진수 연산이 가능.

```c++
bitset<10> b1(88); // 0001011000
bitset<10> b2(47); // 0000101111

cout << (b1 & b2) << '\n'; // 0000001000
cout << (b1 | b2) << '\n'; // 0001111111
cout << (b1 ^ b2) << '\n'; // 0001110111
cout << ~(b1) << '\n'; // 1110100111

cout << (b1 << 2) << '\n'; // 0101100000
cout << (b2 >> 2) << '\n'; // 0000001011
```

[연습문제](https://www.acmicpc.net/problem/12813)

```c++
#include <iostream>
#include <bitset>
#include <string>
using namespace std;
int main() {
    string s1, s2;
    cin >> s1 >> s2;
    bitset<100000> a(s1), b(s2);
    cout << (a & b) << '\n';
    cout << (a | b) << '\n';
    cout << (a ^ b) << '\n';
    cout << (~a) << '\n';
    cout << (~b) << '\n';
    return 0;
}
```

bitset은<>사이에 변수가 들어갈 수 없음.

## string

선언 방법.

```c++
string s1; // 길이가 0인 문자열

char c[] = "c string"; // c언어 스타일의 문자열
string s2(c); // c언어 스타일의 문자열을 알아서 변환해줌
string s3 = c; // 대입 연산자 사용

c[1] = "\0';  // c 뒤에 널문자 삽입
string s4(c); // c만 들어감
string s5 = c; // c만 들어감

string s6(10, '!'); // !!!!!!!!!!
string s7 = "abcdefg"; 
```

문자열 입출력

```c++
string s1, s2;
cin >> s1 >> s2;
cout << s1 << ' ' << s2;
```

[연습문제](https://www.acmicpc.net/problem/1152)

```c++
#include <iostream>
#include <string>
using namespace std;
int main() {
    int cnt = 0;
    string s;

    while (cin >> s) {
        cnt += 1;
    }

    cout << cnt << '\n';
    return 0;
}
```

[연습문제2](https://www.acmicpc.net/problem/10820)

```c++
#include <iostream>
#include <string>
using namespace std;
int main() {
    string s;
    while (getline(cin, s)) {
        int lower = 0;
        int upper = 0;
        int number = 0;
        int space = 0;
        for (char x: s) {
            if (x >= 'a' && x <= 'z') {
                lower += 1;
            } else if (x >= 'A' && x <= 'Z') {
                upper += 1;
            } else if (x >= '0' && x <= '9') {
                number += 1;
            } else if (x == ' ') {
                space += 1;
            }
        }
        cout << lower << ' '<< upper << ' ';
        cout << number << ' ' << space << '\n';
    }
    return 0;
}
```

[연습문제3](https://www.acmicpc.net/problem/10821)

getline의 세 번째 인자는 구분자임.

```c++
#include <iostream>
#include <string>
using namespace std;
int main() {
    string s;
    int cnt = 0;
    while (getline(cin, s, ',')) {
        cnt += 1;
    }
    cout << cnt << '\n';
    return 0;
}
```

printf로 출력해야하는 경우 c_str 함수 이용.

```c++
string s = "abc";
printf("%s\n", s.c_str());
```

string은 size와 length 함수가 둘 다 있음. size은 unsigned int임. 그래서 사이즈가 0일 때 -1을 하면 unsigned int의 최대값이 출력됨.

```c++
string s = "book;
cout << s << ": " << s.size() << '\n'; // 4 출력
cout << s << ": " << s.length() << '\n'; // 4 출력

s = "";
cout << s << ": " << s.size() << '\n'; // 0 출력
cout << s << ": " << s.size-1() << '\n'; // 1844674407370955515615 출력
```

[연습문제](https://www.acmicpc.net/problem/2743)

빈 문자열 확인.

```c++
string s = "book";
cout << s << ": " << s.empty() << '\n'; // 0 출력
```

연산자를 이용한 문자열의 대소비교.

```c++
string s1 = "string";
string s2 = "string";

if(s1 == s2) {
 cout << "s1 == s2" << '\n';
} else if(s1 != s2) {
	cout << "s1 != s2" << '\n';
}
// 사전순으로 앞서는지 아닌지 확인
if(s1 < s2) {
	cout << "s1 < s2" << '\n';
} else if(s1 > s2) {
	cout << "s1 >s2" << '\n';
}
```

문자열 덧셈이 가능.

```c++
string a = "Hello";
string b = "World";

string hello_world = a + " " + b;
hello_world += "!";
hello_world.push_back('!');
cout << hello_world << '\n'; // Hello World!! 출력
```

여러가지 방법으로 추가하기. append를 이용하여 어떤 글자를 몇 개 추가할지 지정할 수 있음.

```c++
string a = "He";
a.append(2, 'l'); // "Hell"
a.append("o").append(1, ' ');  // "Hello "

string b = "";
const char *c = "World";
b.append(c); // "World"

string hello_world = a; // "Hello "
hello_world.append(b); // "Hello World";
hello_world.push_back('!'); // "Hello World!"
```

insert 함수 사용하기. 실제로는 잘 안씀.

```c++
string s = "e"; // "e"
s.insert(0, "H"); // "He"
s.insert(2, "o"); // "Heo"
s.insert(2, 2, "l").append(" "); // "Hello "
string world = "Half the World Away";
s.insert(6, world, 9, 5).push_back('!'); // "Hello World!"
```

stoi(string to int) 함수.

```c++
string str = "10";
int number = stoi(str);
printf(str, number); // 10 출력

number = stoi(str, 0, 2); // 0번 문자부터 2진수로 바꾼다
printf(str, number);  // 2 출력

str = "ffff";
number = stoi(str, 0, 16);
printf(str, number); // 65535 출력

str = "21 Guns";
number = stoi(str); // 알아서 숫자만 바꿔줌
print(str, number); // 21 출력

str = "3.141592";
number = stoi(str); // 정수로 바꿈
print(str, number); // 3 출력

str = "2147483648"; number = stoi(str); // int 범위를 넘어가므로 에러print(str, number);
str = "hello";number = stoi(str); // 숫자가 없어서 에러print(str, number);
```

[연습문제](https://www.acmicpc.net/problem/10822)

```c++
#include <iostream>
#include <string>
using namespace std;
int main() {
    string s;
    int sum = 0;
    while (getline(cin, s, ',')) {
        sum += stoi(s);
    }
    cout << sum << '\n';
    return 0;
}
```

[연습문제2](https://www.acmicpc.net/problem/10823)

string을 표준 입출력(cin, cout)처럼 사용하려면 istringstream을 사용.

```c++
#include <iostream>
#include <sstream>
#include <string>
using namespace std;
int main() {
    string s;
    string line;
    while (cin >> line) {
        s += line;
    }
    
    istringstream sin(s);

    string number;
    int sum = 0;

    while (getline(sin, number, ',')) {
        sum += stoi(number);
    }

    cout << sum << '\n';
    return 0;
}
```

기타 문자열 변환 함수들.

- unsigned long: stoul
- unsigned long long: stoull
- float: stof
- double: stod
- long double: stold
- long long: stoll

다른 자료형을 문자열로 바꾸기. 다양한 자료형이 다 가능함.

```c++
int n1 = 1;
int n2 = 2;

string s1 = to_string(n1);
string s2 = to_string(n2;

cout << s1 + ' ' + s2 << '\n'; // 1 2 출력
```

```c++
long long n1 = 2147483647;
long long n2 = 2147483647;

string s1 = to_string(n1);
string s2 = to_string(n2;

cout << s1 + ' ' + s2 << '\n'; // 2147483647 2147483647 출력
```

## STL Algorithm

### count

- count(begin, end, value): [begin, end)에 포함되어 있는 원소 중에서 value의 개수를 찾음.
- count_if(begin, end, p): [begin, end)에 포함되어 있는 원소 중에서 조건 p에 해당하는 것의 개수를 찾음.
- 시간복잡도: `O(N)`

```c++
vector<int> a = {1, 4, 1, 2, 4, 2, 4, 2, 3, 4, 4};for (int i=1; i<=5; i++) {cout << i << "의 개수: " << count(a.begin(), a.end(), i);cout << '\n';}
// 2 3 1 5 0개
```

```c++
vector<int> a = {1, 4, 1, 2, 4, 2, 4, 2, 3, 4, 4};int even = count_if(a.begin(), a.end(), [](int x) {return x % 2 == 0;});int odd = count_if(a.begin(), a.end(), [](int x) {return x % 2 == 1;});cout << "짝수의 개수: " << even << '\n'; cout << "홀수의 개수: " << odd << '\n';
```

[연습문제](https://www.acmicpc.net/problem/10807)

[연습문제2](https://www.acmicpc.net/problem/10808)

### find

- find(begin, end, value): [begin, end)에 포함되어 있는 원소 중에서 value의 이터레이터를 리턴함. 없는 경우 end를 리턴.
- find_if(begin, end, p): [begin, end)에 포함되어 있는 원소 중에서 조건 p에 해당하는 것의 이터레이터를 리턴함. 없는 경우 end를 리턴.
- 시간복잡도: `O(N)`
- value가 여러 개 있는 경우 제일 첫 번째 이터레이터를 리턴함.

```c++
vector<int> a = {1, 4, 1, 2, 4, 2, 4, 2, 3, 4, 4};for (int i=1; i<=5; i++) {	auto it = find(a.begin(), a.end(), i);	cout << i << "의 위치: ";	if (it == a.end()) {		cout << "찾을 수 없음";	} else {		cout << (it-a.begin());	}cout << '\n';}
```

```c++
vector<int> a = {1, 4, 1, 2, 4, 2, 4, 2, 3, 4, 4};auto even = find_if(a.begin(), a.end(), [](int x) {return x % 2 == 0;});auto odd = find_if(a.begin(), a.end(), [](int x) {return x % 2 == 1;});cout << "첫 번째 짝수: " << (even - a.begin()) << '\n';cout << "첫 번째 홀수: " << (odd - a.begin()) << '\n';
```

[연습문제](https://www.acmicpc.net/problem/10809)

### fill

- fill(begin, end, value): [begin, end)을 value로 채움.
- fill 대신 cstring에 선언되어있는 memset을 주로 사용함. memset은 0과 -1을 제외하면 동작하지 않음.
- 시간복잡도:  `O(N)`

```c++
vector<int> a = {1, 4, 1, 2, 4, 2, 4, 2, 3, 4, 4};for (int x : a) {	cout << x << ' ';}cout << '\n';fill(a.begin(), a.end(), 0);for (int x : a) {	cout << x << ' ';}cout << '\n';
```

[연습문제](https://www.acmicpc.net/problem/10810)

### reverse

- reverse(begin, end): [begin, end)의 순서를 역순으로 만듬.
- 시간복잡도:  `O(N)`

```c++
vector<int> a = {1, 4, 1, 2, 4, 2, 4, 2, 3, 4, 4};for (int x : a) {	cout << x << ' ';}cout << '\n';reverse(a.begin(), a.end());for (int x : a) {	cout << x << ' '; // 4 4 3 2 4 2 4 2 1 4 1 출력	}cout << '\n';
```

[연습문제](https://www.acmicpc.net/problem/10811)

### rotate

- rotate(begin, mid, end): [begin, end)을 mid를 기준으로 왼쪽으로 회전시킴. 
- begin에는 mid에 있던 값이, end-1에는 mid-1에 들어있던 값이 들어감.
- 시간복잡도:  `O(N)`

```c++
vector<int> a = {0, 1, 2, 3, 4, 5};rotate(a.begin(), a.begin()+2, a.end());for (int x : a) {	cout << x << ' ';}cout << '\n';// 0 1 2 3 4 5// 2 3 4 5 0 1
```

```c++
vector<int> a = {0, 1, 2, 3, 4, 5};int n = a.size();for (int i=0; i<n; i++) {	rotate(a.begin(), a.begin()+1, a.end());	print(a);}
```

오른쪽으로 뒤집고 싶을 때.

```c++
vector<int> a = {0, 1, 2, 3, 4, 5};int n = a.size();for (int i=0; i<n; i++) {	//rotate(a.begin(), a.begin()+(n-1), a.end());	rotate(a.rbegin(), a.rbegin()+1, a.rend());	print(a);}
```

[연습문제](https://www.acmicpc.net/problem/10812)

### swap

swap(a, b): a와 b에 들어있던 값을 바꿈.

```c++
int a = 10, b = 20;cout << a << ' ' << b << '\n';swap(a,b);cout << a << ' ' << b << '\n';swap(a,b);cout << a << ' ' << b << '\n';
```

```c++
vector<int> a = {1, 2};vector<int> b = {3, 4};cout << a[0] << ' ' << b[0] << '\n';swap(a,b);cout << a[0] << ' ' << b[0] << '\n';swap(a,b);cout << a[0] << ' ' << b[0] << '\n';
```

[연습문제](https://www.acmicpc.net/problem/10813)

### unique

- 좌표 압축을 할 때 많이 사용. 범위가 중구난방일 때 1부터 n까지로 줄일 수 있는 좋은 방법.
- unique(begin, end): [begin, end) 구간에서 연속되는 같은 값을 하나를 제외하고 제거.
- 실제로 컨테이너의 크기를 줄이거나 제거하지 않음.
- 중복을 덮어씌우거나 시프트 시키는 방식으로 작동함.
- 중복을 제거한 후의 end 이터레이터를 리턴.
- 정렬이 되어있지 않으면 붙어있는 중복값만 제거됨.
- 사용목적: 들어있는 값 중, 같은 값 모두 제거.

```c++
for (int x : a) {	cout << x << ' ';}cout << '\n';auto last = unique(a.begin(),a.end());for (int x : a) {	cout << x << ' ';}cout << '\n';for (auto it = a.begin(); it != last; ++it) {	cout << *it << ' ';}cout << '\n';
```

뒷 부분 제거.

```c++
vector<int> a = {1, 1, 2, 2, 2, 3, 1, 1, 1, 2, 2, 2, 2};auto last = unique(a.begin(),a.end());a.erase(last, a.end());for (int x : a) {	cout << x << ' ';}cout << '\n';
```

정렬 후 사용해야 중복값 제거가능.

```c++
vector<int> a = {1, 1, 2, 2, 2, 3, 1, 1, 1, 2, 2, 2, 2};sort(a.begin(),a.end());auto last = unique(a.begin(),a.end());a.erase(last, a.end());for (int x : a) {	cout << x << ' ';}cout << '\n';
```

### sort

- sort(begin,  end): [begin, end)를 <를 기준으로 정렬. (오름차순)
- sort(begin, end, cmp): [begin, end)를 cmp 기준으로 정렬.

```c++
vector<int> a = {5, 3, 2, 1, 4};for (int x : a) {	cout << x << ' ';}cout << '\n';sort(a.begin(), a.end());for (int x : a) {	cout << x << ' ';}cout << '\n';
```	

[연습문제](https://www.acmicpc.net/problem/2750)

비교함수.

```c++
bool cmp(const int &u, const int &v) {	return u > v; // u가 v의 앞에 있는게 올바르면 treu, 바뀌어야하면 false}vector<int> a = {5, 3, 2, 1, 4};
// 하단 모두 내림차순sort(a.begin(), a.end(), greater<int>());sort(a.begin(), a.end(), cmp);sort(a.begin(), a.end(), [](int u, int v) {	return u > v;});
```

[연습문제](https://www.acmicpc.net/problem/1181)

```c++
sort(a.begin(), a.end(), [](string u, string v) {	if (u.size() < v.size()) { 		return true;	} else if (u.size() == v.size()) {		return u < v;	} else {		return false;	}});
```	

더 간결하게.

```c++
sort(a.begin(), a.end(), [](string u, string v) {if (u.size() == v.size()) {return u < v;} else {return u.size() < v.size();}});
```

```c++
sort(a.begin(), a.end(), [](string u, string v) {	return (u.size() < v.size()) || (u.size() == v.size() && u < v);});
```

하단 방법도 좋음.

```c++
make_pair<u.size(), u) < make_pair(v_size(), v)
```

[연습문제](https://www.acmicpc.net/problem/11650)

pair 사용시 편함.

```c++
int n;scanf("%d",&n);vector<pair<int,int>> a(n);for (int i=0; i<n; i++) {	scanf("%d %d",&a[i].first,&a[i].second);}sort(a.begin(),a.end());for (int i=0; i<a.size(); i++) {	printf("%d %d\n",a[i].first, a[i].second);}
```

직접 struct를 구현하는 경우에는 비교 함수를 만들어줘야함.

```c++
struct Point {int x, y;}bool cmp(const Point &u, const Point &v) {if (u.x < v.x) {return true;} else if (u.x == v.x) {return u.y < v.y;} else {return false;}}
```	

< 연산자를 오버로딩해 구현할 수 있음.

// todo 후에 좀 더 이해하기.

```c++
#include <cstdio>
#include <vector>
#include <algorithm>
using namespace std;
struct Point {
    int x, y;
    bool operator < (const Point &v) const {
        if (x < v.x) {
            return true;
        } else if (x == v.x) {
            return y < v.y;
        } else {
            return false;
        }
    }
};
int main() {
    int n;
    scanf("%d",&n);
    vector<Point> a(n);
    for (int i=0; i<n; i++) {
        scanf("%d %d",&a[i].x,&a[i].y);
    }
    sort(a.begin(),a.end());
    for (int i=0; i<a.size(); i++) {
        printf("%d %d\n",a[i].x, a[i].y);
    }
    return 0;
}
```

[연습문제2](https://www.acmicpc.net/problem/11651)

[연습문제3](https://www.acmicpc.net/problem/10825)

### stable_sort

- stable_sort(begin, end): [begin, end)를 <를 기준으로 정렬.
- stable_sort(begin, end, cmp): [begin, end)를 cmp를 기준으로 정렬.
- sort와의 차이점은 값이 같은 경우에 입력으로 주어진 순서가 유지됨.
- 버블정렬, 머지소트가 대표적임.

[연습문제](https://www.acmicpc.net/problem/10814)

### binary_search

- binary_search(begin, end, value): [begin, end)에서 value를 찾으면  true, 못 찾으면 false.
- binary search를 사용하기 때문에 정렬되어있어야 함.
- binary_search(begin, end, value, cmp): [begin, end)에서 value를 cmp를 기준으로 찾으면  treu, 못찾으면 false.

```c++
vector<int> a = {1, 5, 6, 7, 10, 20};for (int i=1; i<=10; i++) {cout << i << ": ";cout << binary_search(a.begin(), a.end(), i) << '\n';}
```

[연습문제](https://www.acmicpc.net/problem/10815)

### lower_bound / upper_bound

- lower_bound: [begin, end)에서 value보다 작지 않은 첫 번째 이터레이터.
- upper_bound: [begin, end)에서 value보다 큰 첫 번째 이터레이터.
- binary search를 이용해서 구현되어있으므로 binary search를 사용해서 어떤 값의 위치를 찾고 싶을 때 이용하면됨.
- 정렬이 되어있으면 upper bound에서 lower bound를 빼면 그 수가 몇 개 있는지 알 수 있음.

```c++
vector<int> a = {1, 3, 4, 5, 7, 7, 8};for (int i=1; i<=10; i++) {	auto l = lower_bound(a.begin(), a.end(), i);	auto r = upper_bound(a.begin(), a.end(), i);	cout << i << ": ";	cout << "lower_bound: " << (l-a.begin()) << ' ';	cout << "upper_bound: " << (r-a.begin());	cout << '\n';}
```

[연습문제](https://www.acmicpc.net/problem/10815)

### equal_range

- equal_range(begin, end, value): lower_bount, upper_bount를 pair 형태로 리턴.

[연습문제](https://www.acmicpc.net/problem/10815)

### min / max

- min(a, b)- min(a, b, cmp)- min(initializer_list)- min(initializer_list, cmp)- max(a, b)- max(a, b, cmp)- max(initializer_list)- max(initializer_list, cmp)

```c++
cout << min(2, 3) << '\n';cout << max(2, 3) << '\n';int a = 10, b = 20, c = 30;cout << min(min(a, b), c) << '\n';cout << min({a, b, c}) << '\n';cout << max(max(a, b), c) << '\n';cout << max({a, b, c}) << '\n';
```

cmp 함수 만들기.

```c++
string u = "long string";string v = "short";cout << min(u, v) << '\n';cout << min(u, v, [](string u, string v) {return u.size() < v.size();}) << '\n';
```

[연습문제](https://www.acmicpc.net/problem/10817)

### minmax

- minmax(a, b)- minmax(a, b, cmp)- minmax(initializer_list)- minmax(initializer_list, cmp)
- min과 max를 pair로 리턴함.

[연습문제](https://www.acmicpc.net/problem/10817)

### min_element / max_element

- min_element(begin, end)- min_element(begin, end, cmp)- max_element(begin, end)- max_element(begin, end, cmp)
- [begin, end)에서 최소/최대값의 이터레이터를 구함.

```c++
vector<int> a = {4, 2, 1, 5, 7, 3};auto it = min_element(a.begin(), a.end());cout << "최소: " << *it << ", 위치: " << (it-a.begin()) << '\n';it = max_element(a.begin(), a.end());cout << "최대: " << *it << ", 위치: " << (it-a.begin()) << '\n';
```

[연습문제](https://www.acmicpc.net/problem/10818)

### minmax_element

- minmax_element(begin, end)- minmax_element(begin, end, cmp)

[연습문제](https://www.acmicpc.net/problem/10818)

### next_permutation

- next_permutation(begin, end)- next_permutation(begin, end, cmp)- prev_permutation(begin, end)- prev_permutation(begin, end, cmp)
- [begin, end)를 순열이라고 생각했을 때, 사전 순으로 다음에 오는 순열을 만듬.
- 마지막 순열이면 false 리턴. 
- do-while문을 사용하는 거의 유일한 경우.
- 모든 경우를 다해야하는데 순서가 중요할 때 많이 사용하는 알고리즘.

```c++
vector<int> a = {3, 1, 2};sort(a.begin(), a.end());do {for (int x: a) {	cout << x << ' ';}	cout << '\n';} while(next_permutation(a.begin(), a.end()));
```