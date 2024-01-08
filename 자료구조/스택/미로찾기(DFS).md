## 미로찾기 (DFS, Depth First Search : 깊이 우선 탐색)
### 유사코드
```
searchExit()

스택 s와 출구의 위치 x를 초기화;
s에 입구의 위치를 삽입;
while(s.isEmpty() = false) {
  do 현재위치 <- s.pop();
    if(현재위치가 출구 위치이면)
      then 성공;
    if(현재위치의 위, 아래, 왼쪽, 오른쪽 위치가 아직 방문되지 않았고 갈 수 있으면)
      then 그 위치들을 스택 s에 push();
실패;
```
</br>

### 미로 탐색을 위한 2차원 좌표 클래스
```C++
struck Location2D {
    int row;
    int col;
    Location2D(int r=0, int c=0) {
        row = r;
        col = c;
    }

    //위치 p가 자신의 이웃인지 검사하는 함수
    bool isNeighbor(Location2D &p) {
        return ((row==p.row && (col==p.col-1) || col==p.col+1)) || (col==p.col && (row==p.row-1 || row==p.row+1));
    }

    //위치 p가 자신과 같은 위치인지 검사(연산자 오버로딩 사용)
    bool operator==(Location2D &p) {
     return row==p.row && col==p.col;   
    }
}
```
</br>

### 구현
```C++
const int MAZE_SIZE = 6;
char map[MAZE_SIZE][MAZE_SIZE] = {  //e: 입구, x: 출구, 1: 벽, 0: 빈방
    {'1', '1', '1', '1', '1', '1'},
    {'e', '0', '1', '0', '0', '1'},
    {'1', '0', '0', '0', '1', '1'},
    {'1', '0', '1', '0', '1', '1'},
    {'1', '0', '1', '0', '0', 'x'},
    {'1', '1', '1', '1', '1', '1'}
}

bool isValidLoc(int r, int c) {
    if(r<0 || c<0 || r>=MAZE_SIZE || c>=MAZE_SIZE) return false;
    return map[r][c]=='0' || map[r][c] == 'x';
}

void main() {
    stack<Location2D> locStack;  //위치 스택 객체 생성 
    Location2D entry(1, 0);  //입구 객체
    locStack.push(entry);  //스택에 입구 위치 삽입

    while(locStack.empty() == false) {
        Location2D here = locStack.top();
        locStack.pop();

        int r = here.row;
        int c = here.col;
        if(map[r][c] == 'x') {  //탐색 성공
            printf("미로 탐색 성공");
            return;
        } else {
            map[r][c] = '.';  //방문 표시
            if(isValidLoc(r-1, c)) locStack.push(Location2D(r-1, c));
            if(isValidLoc(r+1, c)) locStack.push(Location2D(r+1, c));
            if(isValidLoc(r, c-1)) locStack.push(Location2D(r, c-1));
            if(isValidLoc(r, c+1)) locStack.push(Location2D(r, c+1));
        }
    }
    printf("미로 탐색 실패");
}
```
