## 미로찾기 (BFS, Breadth First Search : 너비 우선 탐색)
* `enqueue`는 `push()`함수로 제공
* `dequeue`는 `front()` + `pop()`함수로 제공
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
#inline "Location2D.h"

void main() {
    const int MAZE_SIZE = 6;
    char map[MAZE_SIZE][MAZE_SIZE] = {  //e: 입구, x: 출구, 1: 벽, 0: 빈방
        {'1', '1', '1', '1', '1', '1'},
        {'e', '0', '1', '0', '0', '1'},
        {'1', '0', '0', '0', '1', '1'},
        {'1', '0', '1', '0', '1', '1'},
        {'1', '0', '1', '0', '0', 'x'},
        {'1', '1', '1', '1', '1', '1'}
    }
    
    queue<Location2D> locQueue;
    Location2D entry(1, 0);
    locQueue.push(entry);
    
    bool isValidLoc(int r, int c) {
        if(r<0 || c<0 || r>=MAZE_SIZE || c>=MAZE_SIZE) return false;
        return map[r][c]=='0' || map[r][c] == 'x';
    }
    
    while(locQueue.empty() == false) {
        Location2D here = locQueue.front();
        locQueue.pop();
        
        int r = here.row;
        int c = here.col;
        if(map[r][c] == 'x') {  //탐색 성공
            printf("미로 탐색 성공");
            return;
        } else {
            map[r][c] = '.';  //방문 표시
            if(isValidLoc(r-1, c)) locQueue.push(Location2D(r-1, c));
            if(isValidLoc(r+1, c)) locQueue.push(Location2D(r+1, c));
            if(isValidLoc(r, c-1)) locQueue.push(Location2D(r, c-1));
            if(isValidLoc(r, c+1)) locQueue.push(Location2D(r, c+1));
        }
    }
    printf("미로 탐색 실패");
    }
}
```
