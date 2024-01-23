## 이진 탐색 트리(BST, Binary Search Tree)
이진트리 기반의 탐색을 위한 자료구조로, 이진 탐색 트리는 특별한 성질을 만족하는 이진트리를 말한다.
* 모든 노드는 `유일한 키`를 갖는다.
* `왼쪽` 서브트리의 키들은 루트의 키보다 `작다`.
* `오른쪽` 서브트리의 키들은 루트의 키보다 `크다`.
* 왼쪽과 오른쪽 서브트리도 이진 탐색 트리이다.
* 이진 탐색 트리를 중위 순회 방법으로 순회하면 오름차순으로 노드를 방문한다.
* 이진 탐색 트리는 정렬된 상태를 유지한다. 
</br>

**탐색 방법**
* 이진 탐색 트리에서 어떤 탐색키가 주어지면 이 값을 루트 노드의 키와 비교한다.
* 탐색키가 루트 노드보다 작으면 원하는 노드는 왼쪽 서브트리에 있고, 크면 오른쪽 서브트리에 있다.
* 이진 탐색 트리는 이를 통해 원하는 키의 값의 노드를 효율적으로 탐색할 수 있도록 한다.
</br>

### ADT
```
객체: 이진 탐색 트리(BST)의 특성을 만족하는 이진트리: 어떤 노드 x의 왼쪽 서브트리의 키들은 x의 키보다 작고, 오른쪽 서브트리의 키들은 x의 키보다 크다. 이때 왼쪽과 오른쪽 서브트리도 모두 이진 탐색 트리이다.
연산:
  - insert(n): 이진 탐색 트리의 특성을 유지하면서 새로운 노드 n을 이진 탐색 트리에 삽입한다. O(logn)
  - remove(n): 이진 탐색 트리의 특성을 유지하면서 노드 n을 트리에서 삭제한다. O(logn)
  - search(key): 키 값이 key인 노드를 찾아 반환한다. O(logn)
```
* 삽입, 삭제, 탐색 연산 모두 이진 트리가 좌우의 서브 트리의 균형을 이룰 경우 `O(logn)`이 되고, 한쪽으로 치우친 경사 이진 트리의 경우 `O(n)`이다.
* 이진 탐색 트리를 만들면서 트리의 높이를 logn으로 한정시키는 균형 기법이 있는데, AVL 트리를 비롯하여 여러 기법들이 있다. 
</br>

### 탐색 연산
#### 순환적인 탐색 함수
```C++
BinaryNode* searchRecur(BinaryNode* n, int key) {
    if(n == NULL) {
        return NULL;
    }
    if(key == n->getData()) {
        return n;
    } else if(key < n->getData()) {
        return searchRecur(n->getLeft(), key);
    } else {
        return searchRecur(n->getRight(), key);
    }
}
```

#### 반복적인 탐색 함수
```C++
BinaryNode* searchIter(BinaryNode* n, int key) {
    while(n != NULL) {
        if(key == n->getData()) {
            return n;
        } else if(key < n->getData()) {
            n = n->getLeft();
        } else {
            n = n->getRight());
        }
    }
    return NULL:
}
```
</br>

### 삽입 연산
* 먼저 해당 키 값을 갖는 노드를 탐색한다.
* 탐색을 하는 이유는 같은 키 값을 갖는 노드가 없어야 하고, 탐색에 실패한 위치가 삽입될 위치이기 때문이다.
```C++
void insertRecur(BinarNode* r, BinaryNode* n) {  //n이 새로 삽입될 노드
    if(n->getData() == r->getData()) {  //이미 있는 데이터
        return;
    } else if(n->getData() < r->getData()) {
        if(r->getLeft() == NULL) {
            r->setLeft(n);
        } else {
            insertRecur(r->getLeft(), n);
        }
    } else {
        if(r->getRight() == NULL) {
            r->setRight(n);
        } else {
            insertRecur(r->getRight(), n);
        }
    }
}
```
</br>

### 삭제 연산
* 삭제하려는 노드의 위치를 탐색한다.
* 삭제에는 다음의 3가지 경우를 고려해야 한다.
  1. 삭제하려는 노드가 단말 노드일 경우
  2. 삭제하려는 노드가 왼쪽이나 오른쪽 서브트리 중 하나만 가지고 있는 경우
  3. 삭제하려는 노드가 두 개의 서브트리 모두 가지고 있는 경우
* 이진 탐색 트리에서의 삭제는 자신을 삭제하고 자신의 후계자를 정해야 하는 것과 비슷하다.
* 첫 번째 경우는 물려줄 사람이 없으므로 간단하다.
* 두 번째 경우는 그냥 후계자가 하나이므로 그냥 그 사람한테 물려주면 된다.
* 세 번째 경우는 양쪽 파벌 중에 가장 중도노선을 걷는 사람에게 물려준다. (가장 복잡)

#### case1: 삭제하려는 노드가 단말 노드인 경우
* 단말 노드만 지우면 된다.
* 단말 노드의 부모 노드를 찾아 부모 노드의 링크필드를 NULL로 만들어 연결을 끊는다.
* 마지막으로 단말 노드를 동적으로 해제시킨다.
* 주의할 점은 삭제할 노드와 함께 부모 노드를 알아야 한다.
```C++
void remove(BinaryNode* parent, BinaryNode* node) {
    //case1: 삭제하려는 노드가 단말 노드일 경우
    if(node->isLeaf()) {
      if(parent==NULL) {  //node==root일 경우, 공백상태가 된다. 
          root = NULL;
      } else {
          if(parent->getLeft() == node) {
              parent->setLeft(NULL);
          } else {
              parent->setRight(NULL);
          }
      }
    }
    delete node;
}
```

#### case2: 삭제하려는 노드가 하나의 서브트리만 가지고 있는 경우
* 그 노드를 삭제한다.
* 유일한 자식을 부모 노드에 붙여준다.
* 이 경우도 삭제를 위해 부모 노드와 자식을 대체할 자식 노드를 알아야 한다.
```C++
void remove(BinaryNode* parent, BinaryNode* node) {
    //case2: 삭제하려는 노드가 왼쪽이나 오른쪽 자식만 갖는 경우
    else if(node->getLeft() == NULL || node->getRight() == NULL) {
        BinaryNode* child = (node->getLeft() != NULL) ? node->getLeft() : node->getRight();
        if(node == root) {
            root = child;
        } else {
            if(parent->getLeft() == node) {
                parent->setLeft(child);
            } else {
                parent->setRight(child);
            }
        }
    }
    delete node;
}
```
#### case3: 삭제하려는 노드가 두 개의 서브트리를 가지고 있는 경우
* 노드를 삭제한다.
* 왼쪽 자식의 루트 노드나 오른쪽 자식 노드의 루트 노드를 그냥 가지고 오면 안 된다. 이진 탐색 트리의 조건이 만족하도록 유지해야 하기 때문이다.
* 이진 탐색 트리의 조건과 만족하기 위해선 삭제되는 노드와 가장 값이 비슷한 노드가 대체되어야 한다.
* 해당 값은 왼쪽 서브트리에서 가장 큰 값이나 오른쪽 서브트리에서 가장 작은 값이다. 이 둘 중 어느 값을 선택해도 상관없다.
* 오른쪽 서브트리에서 가장 작은 값을 선택한다면 해당 값을 탐색하기 위해선 오른쪽 서브트리에서 왼쪽 자식 링크를 타고 NULL을 만날 때까지 진행하면 된다.
* 이 경우 삭제할 후계 노드, 후계 노드의 부모 정보가 필요하다.
```C++
void remove(BinaryNode* parent, BinaryNode* node) {
    //case3: 삭제하려는 노드가 두개의 자식이 모두 있는 경우
    else if(node->getLeft() == NULL || node->getRight() == NULL) {
        //삭제하려는 노드의 오른쪽 서브트리에서 가장 작은 노드를 탐색
        //succ => 후계 노드
        //succp => 후계 노드의 부모 노드
        BinaryNode* succp = node;
        BinaryNode* succ = node->getRight();
        while(succ->getLeft() != NULL) {
            succp = succ;
            succ = succ->getLeft();
        }
        //후계 노드의 부모와 후계 노드의 오른쪽 자식을 직접 연결
        if(succp->getLeft() == succ) {
            succp->setLeft(succ->getRight());
        } else {  //후계 노드가 삭제할 노드의 바로 오른쪽 자식일 경우
            succp->setRight(succ->getRight());
        }
        node->setData(succ->getData());
        node=succ;
    }
    delete node;
}
```


  
