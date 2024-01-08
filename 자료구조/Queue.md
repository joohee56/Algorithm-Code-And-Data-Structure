## 큐
<img src="https://github.com/joohee56/Algorithm-Code-And-Data-Structure/assets/83942393/51b9004f-77f2-4011-b58d-83b9d21c670c" width="50%"/>   

* 큐의 대표적인 예로는 은행에서 서비스를 기다리는 고객들이 한 줄로 늘어선 열을 들 수 있다.
* `선입선출(FIFO: First-In First-Out)`
* 큐에서 `삽입`이 일어나는 곳을 `후단(rear)`라고 하고 `삭제`가 일어나는 곳을 `전단(front)`라고 한다.
</br>

### 추상자료형 (ADT)
```
객체: 선입선출(FIFO)의 접근 방법을 유지하는 요소들의 모음
연산:
- enqueue(e): 주어진 요소 e를 큐의 맨 뒤에 추가한다.
- dequeue(): 큐가 비어 있지 않으면 맨 앞 요소를 삭제하고 반환한다.
- isEmpty(): 큐가 비어 있지 않으면 true를 아니면 false를 반환한다.
- peek(): 큐가 비어 있지 않으면 맨 앞 요소를 삭제하지 않고 반환한다.
- isFull(): 큐가 가득 차 있으면 true를 아니면 false를 반환한다.
```
</br>

### 큐의 활용
- `버퍼(buffer)`: 컴퓨터 장치들 사이에서 데이터를 주고 받을 때, 각 장치들 사이에 존재하는 속도나 시간의 차이를 극복하기 위한 임시 기억 장치
  - 키보드와 컴퓨터 사이의 큐(입력 버퍼)
  - CPU와 프린터 사이의 인쇄 작업 큐
  - 실시간 비디오 스트리밍에서의 버퍼링
- 미로찾기(BFS)
</br>

### 큐의 구현
#### `선형 큐`
  * front와 rear의 초기 값은 -1이다.
  * enqueue: rear를 하나 증가시키고 그 위치에 데이터를 저장한다. `O(1)`
  * dequeue: front를 하나 증가시키고 front가 가리키는 위치에 있는 요소를 삭제한다. `O(1)`
  * 문제점
    * front와 rear가 계속 증가만 하기 때문에 언젠가는 배열의 끝에 도달하게 되고, 배열의 앞부분이 비어 있더라도 더 이상 삽입하지 못한다.
    * 따라서, 후단에 더 이상 삽입할 공간이 없으면 모든 요소들을 앞쪽으로 이동시킨다.
    * 이 방법은 시간 복잡도가 O(n)이므로 비효율적이다. 따라서 원형 큐가 사용이 된다.
      
#### `원형 큐`
<img src="https://github.com/joohee56/Algorithm-Code-And-Data-Structure/assets/83942393/def6795e-bea3-4176-a9c4-1fcfb3db26e6" width="50%"/>
</br>
</br>

* front와 rear의 값이 배열의 끝인 MAX_SIZE_QUEUE-1에 도달하면 다음에 증가되는 값은 0이 된다.
* front와 rear의 초기값은 -1이 아닌 0이다. (사실 같은 위치만 가리키기만 하면 된다.)
* enqueue: rear를 하나 증가시키고 그 위치에 데이터를 저장한다. `O(1)`
* dequeue: front를 하나 증가시키고 front가 가리키는 위치에 있는 요소를 삭제한다. `O(1)`  
* 따라서 front는 항상 큐의 첫 번째 요소 하나 앞을, rear은 마지막 요소를 가리킨다.
* front == rear은 공백 상태
* front가 rear보다 하나 앞에 있으면 포화 상태 (공백 상태와 구분하기 위해 한 자리를 비워둔다.)

```C++
#define MAX_QUEUE_SIZE 100

class CirulcarQueue {
protected:
    int front;
    int rear;
    int data[MAX_QUEUE_SIZE];
    
public:
    CirulcarQueue() {
        front = rear = 0;
    }
    bool isEmpty() {
        return front == rear;
    }
    bool isFull() {
        return (rear+1)%MAX_QUEUE_SIZE == front;
    }
    void enqueue(int val) {
        if(isFull()) {
            error(" error: 큐가 포화상태입니다.");
        } else {
            rear = (rear+1)%MAX_QUEUE_SIZE;
            data[rear] = val;
        }
    }
    int dequeue() {
        if(isEmpty()) {
            error(" error: 큐가 공백상태입니다.");
        } else {
            front = (front+1)%MAX_QUEUE_SIZE;
            return data[front];
        }
    }
}
```
</br>

## 덱(deque)
덱(deque)은 dobule-ended queue의 줄임말로서 큐의 전단(front)와 후단(rear)에서 모두 삽입 삭제가 가능한 큐를 의미한다.    
<img src="https://github.com/joohee56/Algorithm-Code-And-Data-Structure/assets/83942393/b718db2c-5fa0-4f2b-b903-03c5254d3de5" width="50%"/>
* 덱은 스택과 큐의 연산들을 모두 가지고 있다.
* `addFront` - 스택의 push
* `deleteFront` - 스택의 pop
* `addRear` - 큐의 enqueue
* `deleteFront` - 스택의 dequeue
* 추가로, `getFront`, `getRear`, `deleteRear` 연산을 제공한다. 
</br>

### 덱의 구현
덱의 동작은 원형 큐과 거의 비슷하다. 
```C++
class CircularDeque: public CircularQueue {
public:
    CircularDeque() {}
    void addRear(int val) {
        enqueue(val);       //CircularQueue의 enqueue 호출
    }
    int deleteFront() {
        return dequeue();   //CircularQueue의 dequeue 호출
    }
    void addFront(int val) {
        if(isFull()) {
            error(" error: 덱이 포화상태입니다.");
        } else {
            data[front] = val;
            front = (front-1+MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE;
        }
    }
    int deleteRear() {
        if(isEmpty()) {
            error(" error: 덱이 공백상태입니다.");
        } else {
            int ret = data[rear];
            rear = (rear-1+MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE;
        }
    }
}
```

      
