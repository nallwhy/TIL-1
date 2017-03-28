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

## 4. [Conditional Statements](https://www.hackerrank.com/challenges/c-tutorial-conditional-if-else)

```c++
#include <cstdio>
#include <vector>
#include <string>
using namespace std;

int main() {
    vector<string> NUMBER = {"one", "two", "three", "four", "five", "six", "seven", "eight", "nine"};
    
    long num;
    scanf("%ld", &num);   
   
    if(num <= 9) {
        printf("%s", NUMBER[num-1].c_str());
    } else {
        printf("Greater than 9");
    }
    return 0;
}
```

## 5. [For Loop](https://www.hackerrank.com/challenges/c-tutorial-for-loop)

```c++
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<string> NUMBER = {"one", "two", "three", "four", "five", "six", "seven", "eight", "nine"};
    int a, b;
    cin >> a >> b;
    for(int i=a; i<=b; i++) {
        if(i <= 9) {
            cout << NUMBER[i-1];
        } else if(i % 2 == 0) {
            cout << "even";
        } else /*if(i % 2 == 1)*/ {
            cout << "odd";            
        }
        cout << "\n";
    }
    return 0;
}
```

## 6. [Functions](https://www.hackerrank.com/challenges/c-tutorial-functions)

```c++
#include <cstdio>
using namespace std;
int max_of_four(int a, int b, int c, int d) {
    int max = a;
    if(max < b) {
        max = b;
    }
    if(max < c) {
        max = c;
    }
    if(max < d) {
        max = d;
    }
    return max;    
}
int main() {
    int a, b, c, d;
    scanf("%d %d %d %d", &a, &b, &c, &d);
    int ans = max_of_four(a, b, c, d);
    printf("%d", ans);
    
    return 0;
}
```

## 7. [Pointer](https://www.hackerrank.com/challenges/c-tutorial-pointer)

```c++
#include <stdio.h>

void update(int *a,int *b) {
    int temp = *a - *b;
    *a = *a + *b;
    if(temp < 0) temp *= -1;
    *b = temp;
}

int main() {
    int a, b;
    int *pa = &a, *pb = &b;
    
    scanf("%d %d", &a, &b);
    update(pa, pb);
    printf("%d\n%d", a, b);

    return 0;
}
```

## 8. [Arrays Intreduction](https://www.hackerrank.com/challenges/arrays-introduction)

```c++
#include <cstdio>
#include <algorithm>
#include <vector>
using namespace std;

int main() {
    /* Enter your code here. Read input from STDIN. Print output to STDOUT */
    int n;
    scanf("%d", &n);
    vector<int> arr(n);
    for(int i=0; i<n; i++) {
        int a;
        scanf("%d", &a);
        arr[i] = a;
    }
    reverse(arr.begin(), arr.end());
    for(int i=0; i<n; i++) {
        printf("%d ", arr[i]);
    }
    return 0;
}
```