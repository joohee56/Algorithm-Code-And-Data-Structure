## 우선순위 큐
* 우선순위가 높은 데이터가 먼저 출력되는 구조이다.
* 우선순위 큐는 배열, 연결 리스트 등의 여러 가지 방법으로 구현이 가능하지만, 가장 효율적인 구조는 `힙(heap)`이다.
</br>

### ADT
```
객체: 우선순위를 가진 요소들의 모음
연산:
  - insert(item): 우선순위 큐에 항목 item을 추가한다.
  - remove(): 우선순위 큐로부터 가장 우선순위가 높은 요소를 삭제하고 이 요소를 반환한다.
  - find(): 우선순위가 가장 높은 요소를 삭제하지 않고 반환한다.
```
</br>

### 우선순위 큐 구현 방법
#### 배열을 사용
* 정렬되지 않은 배열을 이용
  * 삽입: `O(1)`
  * 삭제: `O(n)` - 처음부터 끝까지 모든 요소들을 스캔하면서 가장 우선순위가 높은 요소를 찾아야 한다. 또한, 요소가 삭제된 다음, 뒤에 있는 요소들을 앞으로 이동시켜야 하는 부담도 있다.
* 정렬된 배열
  * 삽입: `O(n)` - 우선, 순차 탐색이나 이진 탐색을 이용해 우선순위를 비교하여 삽입 위치를 찾아야 한다. 그리고 삽입 위치를 찾은 후 삽입 위치 다음에 있는 모든 요소들을 한 칸씩 뒤로 이동시켜야 하므로 O(n)이다.
  * 삭제: `O(1)` - 정렬되어 있으므로 맨 뒤에 있는 요소를 삭제하면 된다.
#### 연결리스트 사용
* 정렬되지 않은 연결리스트 이용
  * 삽입: `O(1)` - 첫 번째 노드로 삽입한다. 배열과의 차이점은 다른 노드들의 이동은 필요없다.
  * 삭제: `O(n)` - 노드들을 방문하여 가장 우선순위가 높은 노드를 찾아야 한다.
* 정렬된 연결리스트 이용
  * 삽입: `O(n)` - 삽입 위치를 찾아야 하므로 O(n)이다.
  * 삭제: `O(1)` - 첫 번째 노드를 삭제하면 되므로 O(1)이다.
* 결과적으로 배열과 연결리스트는 시간 복잡도 차이는 없다.
</br>

## 힙(heap)
* 완전 이진트리의 일종이다.
* 우선순위 큐를 위해 만들어진 자료구조이다.
* 반 정렬 상태를 유지한다.
* 즉, 요소들이 완전히 정렬된 것은 아니지만 전혀 정렬되지 않은 것도 아닌 상태를 이용해 우선순위 큐를 구현한다.
* 삽입 - `O(logn)`
* 삭제 - `O(logn)`
* 힙은 여러 개의 값들 중에서 가장 큰 값이나 가장 작은 값들을 빠르게 찾아내도록 만들어진 자료 구조이다.
* 간단히 말하면 `부모 노드의 키 값이 자식 노드의 키 값보다 항상 큰 이진트리`를 말한다.
* 힙 트리에서는 중복된 값을 허용한다. (이진 탐색 트리는 중복된 값을 허용하지 않는다.)
</br>

### 힙의 구현
* 힙은 완전 이진트리이기 때문에 중간에 비어있는 요소가 없으므로, 힙을 저장하는 효과적인 자료구로 배열이 적합하다.
* 0번 인덱스는 사용하지 않고, 왼쪽 자식의 인덱스는 부모의 인덱스x2, 오른쪽 자식의 인덱스는 (부모의 인덱스x2)+1 로 표현할 수 있다.
```C++
class HeapNode {
    int key;
public:
    HeapNode(int k=0) : key(k) {}
```
```C++
#define MAX_ELEMENT 200
class MaxHeap {
    HeapNode node[MAX_ELEMENT];
    int size;
public:
    MaxHeap(): size(0) {}
    bool isEmpty() {
        return size==0;
    }
    bool isFull() {
        return size==MAX_ELEMENT-1;
    }

    HeapNode& getParent(int i) {
        return node[i/2];
    }
    HeapNode& getLeft(i) {
        return node[i*2];
    }
    HeapNode& getRight(i) {
        return node[i*2+1];
    }

    void insert(int key) {...}
    HeapNode remove() {...}
    HeapNode find() {
        return node[1];
    }
}
```

#### 삽입 연산
1. 힙에 새로운 요소가 들어오면, 일단 새로운 노드를 힙의 마지막 노드에 이어서 삽입한다.
2. 삽입 후에 새로운 노드를 부모 노드들과 교환하여 적절한 위치로 이동시킨다.
```C++
void insert(int key) {
    if(isFull()) {
        return;
    }

    int i = ++size;  //가장 마지막 위치에서 시작
    while(i != 1 && key > getParent(i).getKey()) {  //루트가 아니고, 부모 노드보다 키 값이 크면
        node[i] = getParent(i);   // 부모를 자신노드로 끌어내림
        i/=2;    // 한 레벨 위로 상승
    }
    node[i].setKey(key);    //최종 위치에 데이터 복사
}
```

#### 삭제 연산
1. 루트 노드를 삭제한다.
2. 가장 마지막 노드를 루트 노드로 가져온다.
3. 왼쪽 자식노드와 오른쪽 자식 노드 중 더 큰 값과 비교하여 내려간다.
```C++
HeapNode remove() {
    if(isEmpty()) {
        return NULL;
    }
    HeapNode item = node[1];  //루트노드(꺼낼 요소)
    HeapNode last = node[size--];  //힙의 마지막 노드
    int parent = 1;    //시작 위치
    int child = 2;    //루트의 왼쪽 자식
    while(child <= size) {    //힙 트리 내에서
      if(child < size && getLeft(parent).getKey() < getRight(parent).getKey()) {  //오른쪽 자식의 값이 더 크다면 오른쪽 자식의 인덱스 저장
          child++;
      }
      if(last.getKey() >= node[child].getKey()) {  //적절한 위치
          break;
      }
      node[parent] = node[child];  //교환
      parent = child;
      child*=2;
    }
    node[parent] = last;
    return item;
}
```
</br>

### 힙의 응용: 힙 정렬
힙을 이용하면 데이터를 정렬할 수 있다.
1. n개의 요소를 하나씩 힙에 삽입한다. `n x log(n)`
2. 힙에서 n번에 걸쳐 하나씩 요소들을 삭제하고 출력한다. `n x log(n)`
 
전체 시간은 nlogn + nlogn 이므로 시간 복잡도는 `O(nlogn)`이다. 
   
