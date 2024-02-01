## 다익스트라(Dijkstra)

```Java
void Dijkstra() {
    int[][] adjMatrix = new int[v][v];
    int start = 0;

    int[] dist = new int[v];    //출발지에서 각 정점까지 최단 경로
    boolean[] selected = new boolean[v]; ///최단경로 확정 여부
    PriorityQUeue<Vertex> pq = new PriorityQueue<>();

    Arrays.fill(dist, INF);
    dist[start] = 0;
    pq.offer(new Vertex(start, 0);   //no, dist
    int cnt = 0;
    while(!pq.isEmpty()) {
        //최단경로가 확정되지 않은 정점 중 최소 비용의 정점 선택
        Vertex now = pq.poll();
        if(selected[now.no]) continue;

        selected[now.no] = true;
        if(++cnt == v) break;    //모든 정점 최단 경로 확정

        //이웃한 정점들 중 확정된 정점을 경유하는 것이 더 적은 비용이라면 갱신
        for(int i=0; i<v; i++) {
            if(!selected[i] && adjMatrix[now.no][i] != 0 && dist[now.no] + adjMatrix[now.no][i] < dist[i]) {
                dist[i] = dist[now.no][i] + adjMatrix[now.no][i];
                pq.offer(new Vertex(i, dist[i]));
            }
        }
    }
}

class Vertex implements Comparable<Vertex> {
      int no;
      int minDist;
    
      @Override
      pubilc int compareTo(Vertex v) {
        return Integer.compare(this.minDist, v.minDist);
      }
} 
```
</br>

### 음의 가중치에서의 다익스트라
* 음의 가중치가 있는 그래프에서는 다익스트라가 제대로 동작하지 않는다.

<img src="https://github.com/joohee56/Data-Structure-And-Algorithm-Code/assets/83942393/1f26792d-6505-4a2d-a587-4dda43e33262" width="50%">

* 음의 가중치가 있는 그래프에서는 벨만 포드 알고리즘을 사용하여 해결할 수 있다. 
