# 프로그램 대회에서 사용하는 C, C++, STL

## C

### printf, scanf

포맷 문자열을 이용해서 입출력을 받는 함수.

#### 포맷 문자열

- %d: 정수
- %x: 16진수 정수
- %o: 8진수 정수
- %f: float
- %lf: double
- %Lf: long double
- %lld: long long
- %c: 문자
- %s: 문자열
- %i: 정수 (형태에 따라 다르게 입력 받음)

#### %i

입력하는 수의 형태에 따라 다르게 입력 받음.

- 10진수: 일반 정수 (10 -> 10)
- 8진수: 앞에 0이 붙어있는 정수 (010 -> 8)
- 16진수: 앞에 0x이 붙어있는 정수 (0x10 -> 16)

[연습문제](https://www.acmicpc.net/problem/11816)

```c
#include <stdio.h>

int main() {
    int x;
    scanf("%i", &x);
    printf("%d", x);
    return 0;
}
```

#### scanf

**scanf의 리턴값**

- scanf의 리턴값은 성공적으로 입력받은 인자의 개수.
- 파일의 끝까지 입력받아야 하는 경우에는 `while(scanf("%d %d", &a, &b) == 2)`처럼 사용할 수 있음. (테스트 케이스가 주어지지 않아 파일의 끝이나 EOF까지 입력을 받아야하는 경우 사용할 수 있음)

[연습문제](https://www.acmicpc.net/problem/10951)

```c
#include <stdio.h>

int main() {
    int a, b;
    while(scanf("%d %d", &a, &b) == 2) {
        printf("%d\n", a+b);
    }
    return 0;
}
```

**공백과 줄바꿈은 무시하는 scanf**

```
10 20 30
```
을 입력하는 것과

```
10
             20
     30
```
입력하는 것은 같음.

**%c**

%c는 생각처럼 동작하지 않을 때가 많은데 공백과 줄바꿈 모두 문자여서 문자로 인식하기 때문임.

![](Baekjoon-ProgrammingLanguage_1.png)

A = 65 / 공백 = 32 / 줄바꿈 = 10

위와 같은 경우 공백과 줄바꿈을 문자로 받아서 엉뚱한 출력값이 나옴.

![](Baekjoon-ProgrammingLanguage_2.png)

위와 같은 경우는 공백은 처리가 되었지만 줄바꿈이 처리되지 않음.

![](Baekjoon-ProgrammingLanguage_3.png)

테스트 케이스를 입력받을 때부터 줄바꿈을 처리해줘야함.

![](Baekjoon-ProgrammingLanguage_4.png)

가장 많이 쓰는 방법으로 위와 같이 %c 앞에 공백을 주어 줄바꿈과 공백을 무시하고 입력을 받음.

**%[]**

%[123]: 대괄호 안에 입력한 문자(1,2,3)만 입력받음.

ex) 

```c
scanf("%[123]", a);	// 123232131456을 입력하면 a에 123232131만 들어감
```

%[^123]: 대괄호 안에 입력한 문자(1,2,3)을 제외하고 입력받음.

ex) 

```c
scanf("%[^123]", a);	// 9860832을 입력하면 a에 98608만 들어감
```

**응용: 그대로 출력하기**

[연습문제](https://www.acmicpc.net/problem/11718)

`scanf("%[^\n]\n", s);`

- 줄바꿈을 입력받지 않기 때문에 편리한 방법이지만, 각 줄의 앞 뒤에 있는 공백은 무시하므로 빈 줄을 입력받을 수 없음.
- 공백으로 시작하는 경우에 공백을 무시하므로 문자부터 입력받음.

```c
#include <stdio.h>

int main() {
    char s[101];
    while(scanf("%[^\n]\n", s) == 1) {
        printf("%s\n", s);
    }
    return 0;
}
```
 
xcode에서 컴파일한 경우에 한 줄씩 밀려서 출력되는 문제가 있었으나 백준 홈페이지에서는 정답처리됨.

[연습문제2](https://www.acmicpc.net/problem/11719): 공백과 빈 줄이 있는 입력

이럴 경우 어쩔 수 없이 한 글자씩 입력받는 getchar()와 한 줄을 통째로 입력받는fgets() 함수 이용.

```c
#include <stdio.h>

int main() {
    char c;
    while((c = getchar()) && c != EOF) {
        printf("%c", c);
    }
    return 0;
}
```

**%1d**

- %와 d 사이에 넣은 수의 길이 만큼 끊어서 입력을 받음.
- 입력으로 인접행렬이 주어지는 경우에 많이 사용.

[연습문제](https://www.acmicpc.net/problem/11720)

```c
#include <stdio.h>

int main() {
    int c;
    scanf("%d", &c);
    int result = 0;
    while(c--) {
        int a;
        scanf("%1d", &a);
        result += a;
    }
    printf("%d", result);
    return 0;
}
```

**%10s**

[연습문제](https://www.acmicpc.net/problem/11721)

- %와 s 사이에 넣은 수의 길이 만큼 끊어서 입력을 받음.
- 입력받을 수 있는 것의 개수가 지정한 개수보다 적으면 그만큼만 입력받음.

```c
#include <stdio.h>

int main() {
    char s[101];
    while( scanf("%10s", s) == 1) {
        printf("%s\n", s);
    }
    return 0;
}
```

**%*d**

변수에 저장하지 않음

```c
#include <stdio.h>
int main() {
	int x,y;
	scanf("%d %*d %d", &x, &y); // 10 20 30 입력
	printf("%d %d\n", x, y); // 10 30 출력함
	return 0;
}
```