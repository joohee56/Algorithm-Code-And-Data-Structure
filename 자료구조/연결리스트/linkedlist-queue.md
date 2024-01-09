## 연결리스트 큐(LinkedList-Queue)

### 연결리스트
먼저, 데이터를 담는 객체는 Node(노드)라는 객체이고 다음과 같은 구성을 갖고 있다.    
<img src="https://github.com/joohee56/Algorithm-Code-And-Data-Structure/assets/83942393/4b8637c9-d3a6-47a9-989e-dfbbfcad74b9" width="25%"/>    

그리고 위 노드들을 연결한 형식이 LinkedList(연결리스트)가 되고, 좀 더 세밀하게 말하자면 단일 연결 리스트(Singly Linked List)가 된다.    
<img src="https://github.com/joohee56/Algorithm-Code-And-Data-Structure/assets/83942393/1aa4e82c-39a7-4f31-befb-de0d1d379911" width="60%"/>
</br>
</br>

### Node 구현
```Java
class Node<E> {
  E data;
  Node<E> next;	//다음 노드를 가리키는 역할을 하는 변수
  
  Node(E data) {
    this.data = data;
    this.next = null;
  }
}
```
</br>

### 연결리스트 큐 구현
### offer(e)
<img src="https://github.com/joohee56/Algorithm-Code-And-Data-Structure/assets/83942393/996f7761-b596-44de-8556-a83e9a27d008" width="65%"/>

```Java
public class LinkedListQueue<E> implements Queue<E> {
  private Node<E> head;	//큐에서 가장 앞에 있는 노드 객체를 가리키는 변수
  private Node<E> tail;	//큐에서 가장 뒤에 있는 노드 객체를 가리키는 변수
  private int size;
  
  public LinkedListQueue() {
    this.head = null;
    this.tail = null;
    this.size = 0;
  }

  public boolean offer(E value) {
    Node<E> newNode = new Node<E>(value);
    
    //비어있을 경우
    if(size==0) {
      head = newNode;
    } else {	//그 외의 경우 마지막 노드(tail)의 다음 노드(next)가 새 노드를 가르키도록 한다. 
      tail.next = newNode;
    }
    
    tail = newNode;
    size++;
    
    return true;
  }
}
```

### poll()
<img src="https://github.com/joohee56/Algorithm-Code-And-Data-Structure/assets/83942393/810cb1fb-1087-496d-87ba-26a59ed60c81" width="65%"/>

```Java
public E poll() {
  //삭제할 요소가 없을 경우 null 반환
  if(size==0) {
    return null;
  }
  
  //삭제될 요소의 데이터를 반환하기 위한 임시 변수
  E element = head.data;
  
  //head 노드의 다음 노드
  Node<E> nextNode = head.next;
  
  //head의 모든 데이터들을 삭제
  head.data = null;
  head.next = null;
  
  //head가 가리키는 노드를 삭제한 head 노드의 다음 노드를 가리키도록 변경
  head = nextNode;
  size--;
  
  return element;
}
```
</br>

### 전체 코드
```Java
class Node<E> {
  E data;
  Node<E> next;	//다음 노드를 가리키는 역할을 하는 변수
  
  Node(E data) {
    this.data = data;
    this.next = null;
  }
}

public interface Queue<E> {
  /**
   * 큐의 가장 마지막에 요소를 추가
   */
  boolean offer(E e);
  
  /**
   * 큐의 첫 번째 요소를 삭제하고 삭제된 요소 반환
   */
  E poll();
  
  /**
   * 큐의 첫 번째 요소 반환
   */
  E peek();
}

public class LinkedListQueue<E> implements Queue<E> {
  private Node<E> head;	//큐에서 가장 앞에 있는 노드 객체를 가리키는 변수
  private Node<E> tail;	//큐에서 가장 뒤에 있는 노드 객체를 가리키는 변수
  private int size;
  
  public LinkedListQueue() {
    this.head = null;
    this.tail = null;
    this.size = 0;
  }
  
  @Override
  public boolean offer(E value) {
    Node<E> newNode = new Node<E>(value);
    
    //비어있을 경우
    if(size==0) {
      head = newNode;
    } else {	//그 외의 경우 마지막 노드(tail)의 다음 노드(next)가 새 노드를 가르키도록 한다. 
      tail.next = newNode;
    }
    
    tail = newNode;
    size++;
    
    return true;
  }

  @Override
  public E poll() {
    //삭제할 요소가 없을 경우 null 반환
    if(size==0) {
      return null;
    }
    
    //삭제될 요소의 데이터를 반환하기 위한 임시 변수
    E element = head.data;
    
    //head 노드의 다음 노드
    Node<E> nextNode = head.next;
    
    //head의 모든 데이터들을 삭제
    head.data = null;
    head.next = null;
    
    //head가 가리키는 노드를 삭제한 head 노드의 다음 노드를 가리키도록 변경
    head = nextNode;
    size--;
    
    return element;
  }
  
  public E remove() {
    E element = poll();
    
    if(element == null) {
      throw new NoSuchElementException();
    }
    
    return element;
  }

  @Override
  public E peek() {
    //요소가 없을 경우 null 반환
    if(size==0) {
      return null;
    }
    return head.data;
  }
  
  public boolean contains(Object value) {
    /**
     * head 데이터부터 x가 null이 될 때까지 value와 x의 데이터(x.data)가 
     * 같은지 비교하고 같을 경우 true를 반환한다.
     */
    for(Node<E> x = head; x != null; x = x.next) {
      if(value.equals(x.data)) {
        return true;
      }
    }
    return false;
  }
}
```
출처: https://st-lab.tistory.com/184
