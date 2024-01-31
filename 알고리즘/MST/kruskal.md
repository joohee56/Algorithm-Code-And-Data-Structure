## 크루스칼(Kruskal) 알고리즘
* `탐욕적인 방법(greedy method)`을 사용한다.
* 각 단계에서 사이클을 이루지 않는 최소 비용 간선을 선택한다.
* 사이클이 생기는지 검사하는 방법에는 `union-find` 연산이 쓰인다.

### 시간 복잡도
* 간선들을 정렬하는 시간에 좌우된다.
* 퀵 정렬이나 최소 힙과 같은 효율적인 알고리즘을 사용한다면 `O(ElogE)`이다. 

### ADT
```
1. 그래프의 모든 간선을 가중치에 따라 오름차순으로 정렬한다.
2. 가장 가중치가 작은 간선 e를 뽑는다.
3. e를 신장 트리에 넣을 경우 사이클이 생기면 삽입하지 않고 2번으로 이동한다.
4. 사이클이 생기지 않으면 최소 신장 트리에 삽입한다.
5. n-1개의 간선이 삽입될 때 까지 2번으로 이동한다.
```

### 코드
```C++
void kruskal() {
    MinHeap heap;   //최소 힙
    //힙에 모든 간선 삽입
    for(int i=0; i<size-1; i++) {
        for(int j=i+1; j<size; j++) {
            if(hasEdge(i, j) {
               heap.insert(getEdge(i, j), i, j);  //간선의 비용, 정점 i, 정점 j
            }
        }
    }

    int edgeAccepted = 0;   //선택된 간선의 수
    while(edgeAccepted < size-1) {  //간선의 수가 size-1 이 되면 종료
        HeapNode& e= heap.remove();    //최소 힙에서 삭제
        int uset = findSet(e.getV1());  //정점 u의 집합 번호
        int vset = findSet(e.getV2());  //정점 v의 집합 번호
        if(uset != vset) {
            print("간선 추가 : %c - %c (비용: %d)", getVertex(e.getV1()), getVertex(e.getV2()), e.getKey());
            setUnion(uset, vset);
            edgeAccepted++;
        }
    }
}
```
