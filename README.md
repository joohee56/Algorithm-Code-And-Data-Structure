# Data-Structure-And-Algorithm-Code
자료구조와 알고리즘 코드 저장소입니다.    
</br>

* [Java Collection 시간 복잡도 / 특징 정리](https://www.grepiu.com/post/9)
</br>

## 자료구조 
### 선형 자료 구조
* <a href="자료구조/Array.md">배열(Array)</a>
* <a href="자료구조/스택/Stack.md">스택(Stack)</a>
* <a href="자료구조/큐/Queue.md">큐(Queue), 덱(Deque)</a>
* <a href="자료구조/연결리스트/연결리스트.md">연결 리스트(LinkedList)</a>

### 비선형 자료구조
* 트리
  * <a href="자료구조/트리/binaryTree.md">이진트리(BinaryTree)</a>   
  * <a href="자료구조/트리/binarySearchTree.md">이진탐색트리(BinarySearchTree)</a>
  * <a href="자료구조/priorityQueue(heap).md">우선순위 큐(PriorityQueue), 힙(Heap)</a>
* <a href="자료구조/해시.md">해시(Hash)</a>
* 그래프
</br>

## 알고리즘
### 정렬
* [삽입 정렬(insertion sort)](알고리즘/Sort/insertionSort.md)
* [병합 정렬(merge sort)](알고리즘/Sort/mergeSort.md)
* 힙 정렬
* [위상 정렬(topological sort)](알고리즘/Sort/topologicalsort.md)
* 계수 정렬
</br>

| 정렬 방법 | 최악의 경우 | 최선의 경우 | 특징 |
|:-----|:----|:----|:----|
| 삽입 정렬 | O(n^2) |  O(n) | 데이터가 거의 정렬되어 있을 때 최고의 성능을 발휘한다. |
| 병합 정렬 | O(nlogn) | O(nlogn) | 안정적인 성능으로 정렬할 수 있다. 병합 과정에서 추가적인 메모리를 필요로 한다. |
| 힙 정렬 | O(nlogn) | O(nlogn) | 안정적인 성능으로 정렬할 수 있다. 데이터를 삽입과 동시에 정렬할 수 있다. |
| 계수 정렬 | O(n+k) | O(n+k) | 데이터 타입과 범위에 의존적이므로 항상 사용가능한 것은 아니다. </br> k는 값의 최솟값~최댓값 범위의 크기이다. |
| 위상 정렬 | O(v+e) | O(v+e) | 작업의 순서가 존재할 때 사용되는 알고리즘이다. </br> v는 정점의 갯수, e는 간선의 갯수이다. |
</br>

### 최소 신장 트리(MST)
* [크루스칼(Kruskal)](알고리즘/MST/kruskal.md)
* [프림(Prim)](알고리즘/MST/prim.md)
### 최단 경로
  * [다익스트라(Dijkstra)](알고리즘/dijkstra.md)
  * 플로이드(Floyd)
  * 벨만 포드(Bellman-ford)
