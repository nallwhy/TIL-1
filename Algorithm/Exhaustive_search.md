# 알고리즘 문제 해결 전략 - Exhaustive search (완전 탐색)

- Brute force (무식하게 풀기): 컴퓨터의 빠른 계산 능력을 이용해 가능한 경우의 수를 일일이 나열하면서 답을 찾는 방법
- Exhaustive search (완전 탐색): 가능한 방법을 전부 만들어 보는 알고리즘

## Recursive function, Recursion (재귀 함수, 재귀 호출)

- 범위가 작아지면 작아질수록 각 조각들의 형태가 유사해지는 작업을 구현할 때 유용하게 사용되는 개념
- 자신이 수행할 작업을 유사한 형태의 여러 조각으로 쪼갠 뒤 그 중 한 조각을 수행하고, 나머지를 자기 자신을 호출해 실행하는 함수

### 자연수 n이 주어졌을 때 1부터 n까지의 합을 반환하는 함수

```c++
int sum(int n) {
    int result = 0;
    for(int i=1; i<=n; i++) {
        result += i;
    }
    return result;
}

int recursiveSum(int n) {
    if(n == 1) return 1;	// 더이상 쪼개지지 않을 때
    return n + recursiveSum(n-1);
}
```

모든 재귀 함수는 base case('더이상 쪼개지지 않는' 최소한의 작업)에 도달했을 때 답을 곧장 반환하는 조건문을 포함해야 함

### n개의 원소 중 m개를 고르는 모든 조합을 찾는 알고리즘

```c++
// 중첩 for 문
for(int i=0; i<n; i++) 
	for(int j=i+1; j<n; j++) 
		cout << i << " " << j << endl;
	
// 재귀 함수	
// picked: 지금까지 고른 원소들의 번호
// toPick: 더 고를 원소의 수
void pick(int n, vector<int>& picked, int toPick) {
    // base case
    if(toPick == 0) {
        printPicked(picked);
        return;
    }
    // 고를 수 있는 가장 작은 번호 계산
    int smallest = picked.empty() ? 0 : picked.back() + 1;
    // 원소 하나 고름
    for(int next=smallest; next<n; next++) {
        picked.push_back(next);
        pick(n, picked, toPick-1);
        picked.pop_back();
    }
}
```

중첩 for문과 달리 우리가 n개의 원소 중에서 몇 개를 고르든지 사용할 수 있음

**간결한 코드를 작성하는 Tip**: 입력이 잘못되거나 범위에서 벗어난 경우도 base case로 택해 맨 처음에 처리 -> 함수를 호출하는 시점에서 이런 오류를 검사할 필요가 없음. 재귀 함수는 항상 한군데 이상에서 호출되기 때문에, 이 습관은 반복적인 코드를 제거하는 데 큰 도움이 됨.

### 예제 - [보글게임 BOGGLE](https://algospot.com/judge/problem/read/BOGGLE) (난이도: 하)

```c++
const int dx[8] = {-1, -1, -1, 1, 1, 1, 0, 0};
const int dy[8] = {-1, 0, 1, -1, 0, 1, -1, 1};

// 5x5 게임 판의 해당 위치에서 주어진 단어가 시작하는지를 반환
bool hasWord(int y, int x, const string& word) {
    // base case 1: 시작 위치가 범위 밖이면 무조건 실패
    if(!inRange(y, x)) return false;
    // base case 2: 첫 글자가 일치하지 않으면 실패
    if(board[y][x] != word[0]) return false;
    // base case 3: 단어 길이가 1이면 성공
    if(word.size() == 1) return true;
    // 인접한 여덟 칸을 검사
    for(int direction=0; direction<8; direction++) {
        int nextY = y + dy[direction], nextX = x + dx[direction];
        // 다음 칸이 범위 안에 있는지, 첫 글자는 일치하는지 확인할 필요가 없음
        if(hasWord(nextY, nextX, word.substr(1))) return true;
    }
}
```

#### 시간 복잡도 분석

완전 탐색 알고리즘의 시간 복잡도 계산은 가능한 답 후보를 모두 만들어보기 때문에, 후보의 최대 수를 계산하면 됨

이 알고리즘의 경우에는 답을 하나라도 찾으면 바로 종료하기 때문에 분석이 까다로움. 최악의 경우는 답이 아예 존재하지 않는 경우로 시작 위치에서 시작하는 모둔 후보들을 빠짐없이 검사해야 함

각 칸에는 최대 여덟 개의 이웃이 있고, 탐색은 단어의 길이 N에 대해 N-1단계가 진행되므로 검사하는 후보의 수는 최대 `8^(N-1) = O(8^N)`임

후보의 수는 단어의 길이에 따라 지수적으로 증가하므로 단어의 길이가 짧은 경우에만 완전 탐색으로 해결할 수 있고, 단어의 길이가 조금이라도 길어질 경우 다른 설계 패러다임들을 사용해야 함

### 완전 탐색 레시피

1. 완전 탐색은 존재하는 모든 답을 하나씩 검사하므로, 걸리는 시간은 가능한  답의 수에 정확히 비례함. 최대 크기의 입력을 가정했을 때 답의 개수를 계산하고 이들을 모두 제한 시간 안에 생성할 수 있을지를 가늠함. 만약 시간 안에 계산할 수 없다면 다른 설계 패러다임을 적용해야 함
2. 가능한 모든 답의 후보를 만드는 과정을 여러 개의 선택으로 나눔. 각 선택은 답의 후보를 만드는 과정의 한 조각이 됨
3. 그중 하나의 조각을 선택해 답의 일부를 만들고, 나머지 답을 재귀 호출을 통해 완성함
4. 조각이 하나밖에 남지 않은 경우, 혹은 하나도 남지 않은 경우에는 답을 생성했으므로, 이것을 base case로 선택해 처리

## 문제: [소풍 PICNIC](https://algospot.com/judge/problem/read/PICNIC) (난이도: 하)

### 나의 답안

입력을 받는 것까지만 구현하고 알고리즘을 구현 못함

```c++
#include <iostream>

using namespace std;

int main() {
    int c;
    int n, m;
    bool areFriend[10][10];
    cin >> c;
    cin >> n >> m;
    
    while(c--) {
        while(m--) {
            int x, y;
            cin >> x >> y;
            areFriend[x][y] = true;
        }
        // 이곳에서 모든 방법 테스트하는 코드 필요
    }
    
    return 0;
}
```

### 풀이 - 중복으로 세는 문제가 있는 재귀 호출 코드

```c++
int n;
bool areFriends[10][10];
// taken[i] = i 번째 학생이 짝을 이미 찾았으면 true, 아니면 false
int countPairings(bool taken[10]) {
    // base case: 모든 학생이 짝을 찾았으면 한 가지 방법을 찾았으니 종료함
    bool finished = true;
    for(int i=0; i<n; i++) if(!taken[i]) finished = false;
    if(finished) return 1;
    int result = 0;
    // 서로 친구인 두 학생을 찾아 짝을 지어줌
    for(int i=0; i<n; i++) {
        for(int j=0; j<n; j++) {
            if(!taken[i] && !taken[j] && areFriends[i][j]) {
                taken[i] = taken[j] = true;
                result += countPairings(taken);
                taken[i] = taken[j] = false;
            }
        }
    }
    return result;
}
```

- 문제1: 같은 학생 쌍을 두 번 짝지어 줌. (0,1)과 (1,0)를 따로 셈
- 문제2: 다른 순서로 학생들을 짝지어 주는 것을 서로 다른 경우로 셈.. (0,1) 후에 (2,3)을 짝지어 주는 것과 (2,3) 후에 (0,1)를 짝지어주는 것을 다른 경우로 셈

### 풀이 - 중복 문제를 해결한 재귀 호출 코드

```c++
int n;
bool areFriends[10][10];
// taken[i] = i 번째 학생이 짝을 이미 찾았으면 true, 아니면 false
int countPairings(bool taken[10]) {
    // 남은 학생들 중 가장 번호가 빠른 학생을 찾음
    int firstFree = -1;
    for(int i=0; i<n; i++) {
        if(!taken[i]) {
            firstFree = i;
            break;
        }
    }
    // base case: 모든 학생이 짝을 찾았으면 한 가지 방법을 찾았으니 종료함
    if(firstFree == -1) return 1;
    int result = 0;
    // 서로 친구인 두 학생을 찾아 짝을 지어줌
    for(int pairWith=firstFree+1; pairWith<n; pairWith++) {
        if(!taken[pairWith] && areFriends[firstFree][pairWith]) {
            taken[firstFree] = taken[pairWith] = true;
            result += countPairings(taken);
            taken[firstFree] = taken[pairWith] = false;
        }
    }
    return result;
}
```

### 답의 수의 상한

가장 많은 답을 가질 수 있는 입력은 열 명의 학생이 모두 서로 친구인 경우. 이때 최종 답의 개수는 `9*7*5*3*1=945`

### 답안

```c++
#include <iostream>
#include <string.h>

using namespace std;

int c, n, m;
bool areFriends[10][10];
bool taken[10];
// taken[i] = i 번째 학생이 짝을 이미 찾았으면 true, 아니면 false
int countPairings(bool taken[10]) {
    // 남은 학생들 중 가장 번호가 빠른 학생을 찾음
    int firstFree = -1;
    for(int i=0; i<n; i++) {
        if(!taken[i]) {
            firstFree = i;
            break;
        }
    }
    // base case: 모든 학생이 짝을 찾았으면 한 가지 방법을 찾았으니 종료함
    if(firstFree == -1) return 1;
    int result = 0;
    // 서로 친구인 두 학생을 찾아 짝을 지어줌
    for(int pairWith=firstFree+1; pairWith<n; pairWith++) {
        if(!taken[pairWith] && areFriends[firstFree][pairWith]) {
            taken[firstFree] = taken[pairWith] = true;
            result += countPairings(taken);
            taken[firstFree] = taken[pairWith] = false;
        }
    }
    return result;
}

int main() {
    int c, m;
    cin >> c;
    
    while(c--) {
        cin >> n >> m;
        memset(areFriends, false, sizeof(areFriends));
        memset(taken, false, sizeof(taken));
        for(int i=0; i<m; i++) {
            int x, y;
            cin >> x >> y;
            areFriends[x][y] = areFriends[y][x] = true;
        }
        cout << countPairings(taken) << '\n';
    }
    
    return 0;
}
```

## 문제: [게임판 덮기](https://algospot.com/judge/problem/read/BOARDCOVER) (난이도: 하)

### 나의 답안 

입력조차 못받음.. 이럴려고 알고리즘 공부하나 자괴감들고 괴로워..

### 답의 수의 상한

한 블록을 4가지의 방법으로 놓을 수 있고, 최대 [50/3]=16개의 블록을 놓기 때문에 가능한 답의 상한은 `4^16=2^32`개가 됨

그냥 보면 시간 내에 해결할 수 없어 보이지만, 실제 입력을 손으로 풀어 보면 각 단계에서 우리가 선택할 수 있는 블록 배치가 크게 제한됨. 예를들어 흰 칸이 6칸 있는 입력이 주어지면 이론상 4^2=16가지 방법이 있어야 하는데, 실제로는 잘 자해봐야 두 가지 방법밖에 없음

### 답안

```c++
#include <iostream>
#include <vector>
#include <string.h>

using namespace std;

// 주어진 칸을 덮을 수 있는 네 가지 방법
// 블록을 구성하는 세 칸의 상대적 위치 (dy, dx)의 목록
const int coverType[4][3][2] = {
    { {0,0}, {1,0}, {0,1} },
    { {0,0}, {0,1}, {1,1} },
    { {0,0}, {1,0}, {1,1} },
    { {0,0}, {1,0}, {1,-1} }
};

// board의 (y,x)를 type번 방법으로 덮거나, 덮었던 블록을 없앰
// delta = 1이면 덮고, -1이면 덮었던 블록을 없앰
// 만약 블록이 제대로 덮이지 않은 경우(게임판 밖으로 나가거나, 겹치거나, 검은 칸을 덮을 때) false을 반환
bool set(vector<vector<int> >& board, int y, int x, int type, int delta) {
    bool ok = true;
    for(int i=0; i<3;  i++) {
        const int ny = y + coverType[type][i][0];
        const int nx = x + coverType[type][i][1];
        if(ny < 0 || ny >= board.size() || nx < 0 || nx >= board[0].size()) ok = false;
        else if((board[ny][nx] +=  delta) > 1) ok = false;
    }
    return ok;
}

// board의 모든 빈 칸을 덮을 수 있는 방법의 수를 반환
// board[i][j] = 1 이미 덮인 칸 혹은 검은 칸
// board[i][j] = 0 아직 덮이지 않은 칸
int cover(vector<vector<int> > board) {
    int y = -1, x = -1;
    for(int i=0; i<board.size(); i++) {
        for(int j=0; j<board[0].size(); j++) {
            if(board[i][j] == 0) {
                y = i;
                x = j;
                break;
            }
        }
        if(y != -1) break;
    }
    
    // base case: 모든 칸을 채웠으면 1 반환
    if(y == -1) return 1;
    int result = 0;
    for(int type=0; type<4; type++) {
        // 만약 board[y][x]를 type 형태로 덮을 수 있으면 재귀 호출
        if(set(board, y, x, type, 1)) result += cover(board);
        // 덮었던 블록을 치움
        set(board, y, x, type, -1);
    }
    return result;
}

int main() {
    int c;
    cin >> c;
    
    while(c--) {
        int h, w;
        cin >> h >> w;
        
        vector<vector<int> > board(h, vector<int>(w, 0));
        
        for(int row=0; row<h; row++) {
            string input;
            cin >> input;
            for(int i=0; i<w; i++) {
                if(input[i] == '#') board[row][i] = 1;
            }
        }
        
        cout << cover(board) << endl;
    }
    
    return 0;
}
```

## Optimization problem (최적화 문제

문제의 답이 하나가 아니라 여러 개이고, 그 중에서 어떤 기준에 따라 가장 좋은 답을 찾아 내는 문제

### 예제: 여행하는 외판원 문제

무식하게 풀 수 있을까? 무조건 0번 도시에서 출발한다고 가정하여 n-1개의 도시를 나열하는 방법은 모두 (n-1)!가지이고, 최대 10개의 도시가 있으므로 10!=362,880이므로 안전하게 완전 탐색을 사용해 문제를 해결할 수 있음

재귀 호출을 통한 답안 생성

```c++
int n; //도시의 수
double dist[MAX][MAX] // 두 도시 간의 거리를 저장하는 배열
// 나머지 도시들을 모두 방문하는 경로들 중 가장 짧은 것의 길이를 반환
double shortestPath(vector<int>& path, vector<bool>& visited, double currentLength) {
    //base case: 모든 도시를 다 방문했을 때는 시작 도시로 돌아가고 종료
    if(path.size() == n) return currentLength + dist[path[0][path.back()]];
    double result = INF; // 매우 큰 값으로 초기화
    // 다음 방문할 도시를 전부 시도
    for(int next=0; next<n; next++) {
        if(visited[next]) continue;
        int here = path.back();
        path.push_back(next);
        visited[next] = treu;
        // 나머지 경로를 재귀 호출을 통해 완성하고 가장 짧은 경로의 길이를 얻음
        double cand = shortestPath(path, visited, currentLength + dist[here][next]);
        result = min(result, cand);
        visited[next] = false;
        path.pop_back();
    }
    return result;
}
```

## 문제: [시계 맞추기](https://algospot.com/judge/problem/read/CLOCKSYNC) (난이도: 중)

