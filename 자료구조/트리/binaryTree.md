# 이진트리
![image](https://github.com/joohee56/Algorithm-Code-And-Data-Structure/assets/83942393/d0d1acd9-584f-4688-8c30-a058c80c9ac6)
* 이진트리란 `자식 노드의 개수가 항상 2개 이하`인 트리를 말한다.
</br>

## 이진트리의 성질
* `n개의 노드를 가진 트리는 n-1개의 간선을 가진다.` : 루트를 제외한 모든 노드는 하나의 부모 노드를 갖는다.
* `높이가 h인 이진트리는 h개 이상, 2^h-1개 이하의 노드를 가진다.` : 한 레벨에는 적어도 하나의 노드가 존재해야 하므로 높이가 h인 이진트리는 최소한 h개의 노드를 가진다. 또한, 하나의 노드는 최대 2개의 자식을 가질 수 있으므로 레벨 i에서의 노드의 최대 개수는 2^(i-1)이다. 따라서 높이가 h인 이진트리의 최대 노드 개수는
* ![image](https://github.com/joohee56/Algorithm-Code-And-Data-Structure/assets/83942393/9cb86ee5-7ac0-42eb-af14-14a4bf80e155) 이므로 2^h-1이 된다.
</br>

## 이진트리 분류
### 1) 포화 이진트리(full binary tree)
<img src="https://github.com/joohee56/Algorithm-Code-And-Data-Structure/assets/83942393/6a9f0ded-2a67-4b7d-8bc0-cc26bf13f3b4" width="50%"/>    

* 각 레벨에 노드가 꽉 차있는 이진트리를 말한다. 즉, 높이 k인 포화 이진트리는 정확하게 2^k-1개의 노드를 가진다.
* 포화 이진트리에서는 각 노드에 번호를 붙일 수 있는데, 왼쪽에서 오른쪽으로 번호를 붙인다. 그리고 이 번호는 항상 일정하다. 즉, 루트 노드의 오른쪽 자식 노드의 번호는 항상 3이다.
</br>

### 2) 완전 이진트리(complete binary tree)
<img src="https://github.com/joohee56/Algorithm-Code-And-Data-Structure/assets/83942393/75c502cf-00ef-4056-a2ac-49f33798c6ac" width="80%"/>    

* 높이가 k인 트리에서 레벨 1부터 k-1까지는 노드가 모두 채워져 있고, 마지막 레벨 k에서는 왼쪽부터 오른쪽으로 노드가 순서대로 채워져 있는 이진트리를 말한다.
* 마지막 레벨에서는 노드가 꽉차있지 않아도 되지만 중간에 빈곳이 있으면 안 된다.
* `힙(heap)`이 완전 이진트리의 대표적인 예이다. 
</br>

## 이진트리 순회
* `전위 순회(preorder traversal)` : `VLR`
* `중위 순휘(inorder traversal)` : `LVR`
* `후위 순회(postorder traversal)` : `LRV`
</br>

## 레벨 순회(level order)
<img src="https://github.com/joohee56/Algorithm-Code-And-Data-Structure/assets/83942393/dbfadb27-929d-41f6-9a11-462bf22ddf86" width="30%"/>   

* 레벨 순으로 검사하는 방법이다.
* 루트 노드의 레벨이 1이고 아래로 내려갈수록 레벨은 증가한다. 동일한 레벨의 경우에는 좌에서 우로 방문한다.
* `큐`를 사용한다.
* 처음에는 큐에 `루트 노드`만 들어 있다. 
* 큐에서 노드를 하나 꺼내 방문하고 그 자식들을 큐에 삽입한다. 이때, 자식이 없으면 삽입하지 않고, 왼쪽 자식을 먼저 오른쪽 자식을 다음에 처리한다.
* 이 과정은 큐가 `공백 상태`가 될 때까지 반복한다.

```
Level_order()

initialize queue;
queue.enqueue(root);
while que.isEmpty() != TRUE do
  x <- queue.dequeue();
  if (x!=null) then
    print DATA(x);
    queue.enqueue(LEFT(x));
    queue.enqueue(RIGHT(x));
```
</br>

## 이진트리 연산
* `트리의 노드 개수 구하기`
```C++
int getCont() {
    return isEmpty() ? 0 : getCount(root);
}

int getCount(BinaryNode* node) {
    if(node == NULL) {
      return 0;
    }
    return 1 + getCount(node -> getLeft()) + getCount(node -> getRight());
}
```

* `단말 노드 개수 구하기`
```C++
int getLeafCount() {
  return isEmpty() ? 0 : getLeafCount(root);
}

int getLeafCount(BinaryNode* node) {
    if(node == NULL) {
        return 0;
    }
    if(node -> isLeaf()) {
        return 1;
    } else {
        return getLeafCount(node->getLeft()) + getLeafCount(node->getRight());
    }
}
```
* `높이 구하기` : 마찬가지로 순환을 통해 왼쪽 서브트리와 오른쪽 서브트리의 높이를 구한 후 둘 중 높은 값을 반환한다.
```C++
int getHeight() {
      return isEmpty()? 0 : getHeight(root);
}

int getHeight(BinaryNode* node) {
    if(node == NULL) {
        return 0;
    |
    int hLeft = getHeight(node->getLeft());
    int hRight = getHeight(node->getRight());
    return (hLeft > hRight) ? hLeft+1 : hRight+1;
}
```
