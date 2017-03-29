# 프로그래밍 대회에서 사용하는 C 강의 정리

강의 사이트: [코드플러스](https://code.plus/course/2)

## 입출력

### scanf, printf

포맷 문자열을 이용해서 입출력을 받는 함수

### 포맷 문자열

- %d: int
- %x: 16진수 정수
- %o: 8진수 정소
- %f: float
- %lf: double
- %Lf: long double
- %lld: long long
- %c: 문자
- %s: 문자열
- %i: 정수의 형태에 따라 다르게 입력 받음

### %i

입력하는 수의 형태에 따라 다르게 입력 받음

- 10진수: 일반 정수 (10 입력 시 10 저장)
- 8진수: 앞에 0이 붙어있는 정수 (010 입력 시 8 저장)
- 16진수: 앞에 0x가 붙어있는 정수 (0x10 입력 시 16 저장)

연습문제: [8진수, 10진수, 16진수](https://www.acmicpc.net/problem/11816)

```c
#include <stdio.h>

int main() {
    int x;
    scanf("%i", &x);
    printf("%d", x);
    return 0;
}
```

### scanf의 리턴값

- scanf의 리턴값은 성공적으로 입력받은 인자의 개수
- 테스트 케이스가 주어지지 않아 파일의 끝이나 EOF까지 입력을 받아야하는 경우 `while( scanf("%d %d", &a, &b) == 2 )`와 같이 사용할 수 있음

연습문제: [A+B - 4](https://www.acmicpc.net/problem/10951)

```c
#include <stdio.h>

int main() {
    int a, b;
    while(scanf("%d %d", &a, &b) == 2)
        printf("%d\n", a+b);
    return 0;
}
```

### scanf는 공백과 줄바꿈을 무시함

```
// 이렇게 입력하는 것과
10 20 30
// 이렇게 입력하는 것이 같음
10
	20
  30
```

### %c

%c를 사용하면 예상대로 동작하지 않을 때가 많은데, 이는 공백과 줄바꿈이 모두 문자여서 문자로 인식하기 때문

아스키코드: A=65 / 공백=32 / 줄바꿈=10

```
입력값
3
A B C
D E F

기대하는 출력
65 66 67
68 69 70
```

```c
// 공백과 줄바꿈 모두 처리하지 않은 코드
// 출력
// 10 65 32
// 66 32 67
#include <stdio.h>

int main() {
    int n;
    scanf("%d", &n);
    while(n--) {
        char x, y, z;
        scanf("%c%c%c", &x, &y, &z);
        printf("%d %d %d\n", x, y, z);
    }
    return 0;
}
```

```c
// 공백을 처리한 코드
// 출력
// 10 65 66
// 32 67 68
#include <stdio.h>

int main() {
    int n;
    scanf("%d", &n);
    while(n--) {
        char x, y, z;
        scanf("%c %c %c", &x, &y, &z); // 공백 처리
        printf("%d %d %d\n", x, y, z);
    }
    return 0;
}
```

```c
// 공백과 줄바꿈 모두 처리한 코드
// 출력
// 65 66 67
// 68 69 70
#include <stdio.h>

int main() {
    int n;
    scanf("%d\n", &n); // 줄바꿈 처리
    while(n--) {
        char x, y, z;
        scanf("%c %c %c\n", &x, &y, &z); // 공백과 줄바꿈 처리
        printf("%d %d %d\n", x, y, z);
    }
    return 0;
}
```

```c
// 가장 많이 쓰이는 방법
// 공백과 줄바꿈 모두 처리한 코드2
// 출력
// 65 66 67
// 68 69 70
#include <stdio.h>

int main() {
    int n;
    scanf("%d", &n); 
    while(n--) {
        char x, y, z;
        // %c 앞에 공백을 주어 공백과 줄바꿈을 모두 무시하고 입력을 받음
        scanf(" %c %c %c\n", &x, &y, &z); // 공백과 줄바꿈 처리
        printf("%d %d %d\n", x, y, z);
    }
    return 0;
}
```

### %[]

`%[123]`: 대괄호 안에 입력한 문자 1, 2, 3이 아닌 문자가 나오기 전까지만 입력받음

```c
scanf("%[123]", a); // 1232156123 입력
printf("%s", a); // 12321 출력
```

`%[^123]`: 대괄호 안에 입력한 문자 1, 2, 3이 나오기 전까지만 입력받음

```c
scanf("%[^123]", a); // 5678912356 입력
printf("%s", a); // 56789 출력
```

연습문제: [그대로 출력하기](https://www.acmicpc.net/problem/11718)

주요 코드: `scanf("%[^\n]\n", a);`

- 줄바꿈을 입력받지 않기 때문에 편리한 방법
- 각 줄의 앞 뒤에 있는 공백을 무시하여 빈 줄은 입력받을 수 없음
- 공백으로 시작하는 경우 공백을 무시하므로 문자부터 입력받음

```c
#include <stdio.h>

int main() {
    char a[110];
    while(scanf("%[^\n]\n", a)==1) {
        printf("%s\n", a);
    }
    return 0;
}
```

연습문제: [그대로 출력하기2](https://www.acmicpc.net/problem/11719)

- 한 글자씩 입력받아 출력하는 방식 이용
- getchar() 혹은 fgets() 함수 이용

```c
#include <stdio.h>

int main() {
    char a;
    while((a = getchar()) && a != EOF) {
        printf("%c", a);
    }
    return 0;
}
```

### %1d

- %와 d 사이에 넣은 수의 길이 만큼 끊어서 입력받음
- 입력으로 인접행렬이 주어지는 경우에 많이 사용

연습문제: [숫자의 합](https://www.acmicpc.net/problem/11720)

```c
#include <stdio.h>

int main() {
    int n;
    int result = 0;
    scanf("%d", &n);
    while(n--) {
        int a;
        scanf("%1d", &a);
        result += a;
    }
    printf("%d", result);
    return 0;
}
```

### %10s

- %와 d 사이에 넣은 수의 길이 만큼 끊어서 입력받음
- 입력받을 수 있는 것의 개수가 지정한 개수보다 적으면 그만큼만 입력받음

연습문제: [열 개씩 끊어 출력하기](https://www.acmicpc.net/problem/11721)

```c
#include <stdio.h>

int main() {
    char n[110];
    while(scanf("%10s", n) == 1) {
        printf("%s\n", n);
    }
    return 0;
}
```

### %*d

%와 d 사이에 *이 있는 경우 변수에 저장하지 않음

```c
#include <stdio.h>
int main() {
	int x,y;
	scanf("%d %*d %d", &x, &y); // 10 20 30 입력
	printf("%d %d\n", x, y); // 10 30 출력함
	return 0;
}
```