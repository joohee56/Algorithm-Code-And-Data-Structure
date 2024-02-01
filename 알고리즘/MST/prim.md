## 프림(Prim)
<img src="https://github.com/joohee56/Data-Structure-And-Algorithm-Code/assets/83942393/a866a85f-abbc-4acb-8751-ae27aacc117f" width="70%">

### 알고리즘
1. 시작 정점을 선택하여 초기 트리를 만든다.
2. 현재 트리의 정점들과 인접한 정점들 중에서 간선의 가중치가 가장 작은 정점 v를 선택한다.
3. 이 정점 v를 트리에 추가한다.
4. 모든 정점이 삽입될 때까지 2번을 반복한다.
</br>

### 시간 복잡도
* 프림은 주 반복문이 정점의 수 n만큼 반복하고, 내부 반복문이 n번 반복하므로 `O(n^2)`의 시간복잡도를 가진다.
* 크루스칼은 `O(eloge)`이므로 `간선의 갯수가 매우 적은` 희박한 그래프(sparse graph)를 대상으로 적합하다.
* 반대로 `간선이 매우 많은 그래프`인 경우 프림 알고리즘이 유리하다. 
</br>

### 코드
```Java
public int Prim(int s) {
    int[][] adjMatrix = new int[n][n];
    int[] dist = new int[n];               //타 정점에서 현재 신장트리로의 간선비용 중 최소비용
    boolean[] selected = new boolean[n];  //신장트리 포함 유무
  
    //배열 초기화
    for(int i=0; i<n; i++) {
        dist[i] = INF;
        selected[i] = false;
    }
    dist[s] = 0;   //시작 정점
    PriorityQueue<Vertex> pq = new PriorityQueue<>();
    pq.offer(new Vertex(s, 0));  //no, edge
  
    int result = 0;  //MST 비용
    int cnt = 0; 
    while(!pq.isEmpty()) {  
        Vertex minVertex  = pq.poll();
        if(selected[minVertex.no]) continue;
  
        //선택한 정점 신장트리에 포함
        selected[minVertex.no] = true;
        result += minVertex.edge;
        if(++cnt == n) break;  //n개의 정점을 모두 연결할 때 까지
  
        //현재 정점과 이웃한 정점들, 신장 트리에 포함되지 않은 정점들의 간선비용 갱신
        for(int i=0; i<n; i++) {
            if(adjMatrix[minVertex.no][i] != 0 && !selected[i] && adjMatrix[minVertex.no][i] < dist[i]) {
                dist[i] = adjMatrix[minVertex.no][i];
                pq.offer(new Vertex(i, dist[i]));
            }
        }
    }
    return result;
}

class Vertex implements Comparable<Vertex> {
    int no;
    int edge;

    @Override
    public int compareTo(Vertex o) {
      return Integer.compare(this.edge, o.edge);
    }
}
```
