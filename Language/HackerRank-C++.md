# HackerRank - C++

## 1. [Say "Hello, World!" With C++](https://www.hackerrank.com/challenges/cpp-hello-world)

```c++
#include <iostream>
using namespace std;

int main() {
    printf("Hello, World!");
    return 0;
}
```

## 2. [Input and Output](https://www.hackerrank.com/challenges/cpp-input-and-output?h_r=next-challenge&h_v=zen)

```c++
#include <iostream>
using namespace std;

int main() {
    int a, b, c;
    cin >> a >> b >> c;
    cout << a+b+c;
    return 0;
}
```

## 3. [Basic Data Types](https://www.hackerrank.com/challenges/c-tutorial-basic-data-types?h_r=next-challenge&h_v=zen)

- Int ("%d") - 32 bit integer
- Long ("%ld") - 32 bit integer (현대 시스템에서는 Int와 같다)
- Long Long ("%lld") - 64 bit integer
- Char ("%c") - Character type
- Float ("%f") - 32 bit real value
- Double("%lf") - 64 bit real value

```c++
#include <iostream>
#include <cstdio>
using namespace std;

int main() {
    int a;
    long b;
    long long c;
    char d;
    float e;
    double f;
    scanf("%d %ld %lld %c %f %lf", &a, &b, &c, &d, &e, &f);
    printf("%d\n%ld\n%lld\n%c\n%f\n%lf", a, b, c, d, e, f);
    return 0;
}
```