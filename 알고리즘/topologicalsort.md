## 위상 정렬 (topological sort)
위상 정렬은 방향 그래프에 존재하는 각 정점들의 선행 순서를 위배하지 않으면서 모든 정점을 나열하는 것을 의미한다.    
예) 인공지능 과목을 수강하기 위해서는 자료구조, 알고리즘, 운영체제를 먼저 수강해야 한다. 

### 알고리즘 
1. 진입 차수가 0인 정점을 선택한다. (여러 개 존재할 경우 어느 정점을 선택해도 무방하다.)
2. 선택된 정점과 여기에 연결된 모든 간선을 삭제한다.
3. 삭제되는 간선과 연결된 남아 있는 정점들의 진입 차수를 변경한다.
4. 모든정점이 삭제되면 알고리즘이 종료된다.

### 특징
* 정점이 삭제되는 순서가 위상 순서(topological order)가 된다.
* 남아 있는 정점 중 진입 차수가 0인 정점이 없다면 위상 정렬은 불가능하다. 이것은 그래프에 사이클이 존재한다는 의미이고, 모든 과목이 선수 과목을 갖는 것을 말한다.

### 시간 복잡도
* 정점의 갯수가 V이고, 간선의 갯수가 E 라면 `O(V+E)`이다.
* 모든 노드를 확인하면서 각 노드에서 나가는 간선들을 하나씩 제거하는 과정을 거치기 떄문이다.

### ADT
```
topoSort()

for i<-0 to n-1 do
  if 모든 정점이 선행 정점을 가지면
    then 사이클이 존재하고 위상 정렬 불가;
  선행 정점을 가지지 않는 정점 v 선택;
  v를 출력;
  v와 v에서 나온 모든 간선들을 그래프에서 삭제;
```

### 코드
```C++
void TopoSort() {
  //모든 정점의 진입 차수를 계산
  for(int i=0; i<size; i++) {
      inDeg[i] = 0;  //초기화
  }
  for(int i=0; i<size; i++) {
      Node *node = adj[i];  //정점 i에서 나오는 간선들
      while(node != NULL) {
          inDeg[node->getId()]++;
          node = node->getLink();
      }
  }
  //진입 차수가 0인 정점을 스택에 삽입
  ArrayStack s;
  for(int i=0; i<size; i++) {
      if(inDeg[i] == 0) {
          s.push(i);
      }
  }
  //위상 순서를 생성
  while(s.isEmpty() == false) {
      int w = s.pop();
      printf("%c", getVertex(w));  //정점 출력
      Node *node = adj[w];        //정점에 연결된 정점들의 차수를 변경
      while(node != NULL) {
        int u = node->getId();
        inDeg[u]--;              //진입 차수를 감소
        if(inDeg[u] == 0) {
            s.push(u);
        }
        node = node->getLink();
      }
  }
}
```

