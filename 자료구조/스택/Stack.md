## 스택
<img src="https://github.com/joohee56/Algorithm-Code-And-Data-Structure/assets/83942393/eba7c0c7-58d5-48eb-8cc7-4909e40c5280" width="50%"/>
</br>

### 특징
* 식당 주방에 `쌓여있는 접시 더미`를 생각하면 이해하기 쉽다.
* `후입선출(LIFO: Last-In-First-Out)`
* 입출력은 `맨 위`에서만 일어나고 중간에는 데이터를 삽입하거나 삭제할 수 없다.
</br>

### 추상 자료형(ADT)
```
객체: 후입선출(LIFO)의 접근 방법을 유지하는 요소들의 모음
연산:
- push(x): 주어진 요소 x를 스택의 맨 위에 추가한다.
- pop(): 스택이 비어있지 않으면 맨 위에 있는 요소를 삭제하고 반환한다.
- isEmpty(): 스택이 비어있으면 true를 아니면 false를 반환한다.
- peek(): 스택이 비어있지 않으면 맨 위에 있는 요소를 삭제하지 않고 반환한다.
- isFull(): 스택이 가득 차 있으면 true를 아니면 false를 반환한다.
- size(): 스택 내의 모든 요소들의 개수를 반환한다.
- display(): 스택 내의 모든 요소들을 출력한다.
```
</br>

### 활용 예
- 문서나 그림, 수식 등의 편집기(Editor)에서 되돌리기(undo) 기능
- 함수 호출에서 복귀 주소를 기억하는데 스택 사용
- 소스코드나 문서에서 <a href="괄호검사와 스택.md">괄호 닫기가 정상적으로 되었는지 검사</a>
- 계산기 프로그램에서 입력된 수식을 계산
- 미로찾기에서 출구 찾기
</br>

### 스택의 구현
* `배열`
  * 간단히 구현할 수 있지만, 스택의 크기가 고정된다.
* `연결리스트`
  * 조금 복잡하지만, 크기가 제한되지 않는다. 따라서 isFull() 연산이 필요없다.
* 삽입과 삭제 연산을 위한 변수 `top`이 필요하다. top은 초기엔 -1을 가리키고, 데이터가 push되면 1 증가하여 해당 위치에 데이터를 넣는다. 즉, top은 가장 최근에 들어온 데이터를 가리킨다.
</br>

`배열로 구현한 int 스택 클래스`
```C++
#include <cstdio>  
#include <cstdlib>

// 오류 처리 함수
inline void error(char *message) {
    printf("%s\n", message);
    exit(1);
}

const int MAX_STACK_SIZE = 20;  //스택의 최대 크기 설정
class ArrayStack {
    int top;    //요소의 개수
    int data[MAX_STACK_SIZE];   //요소의 배열
public:
    ArrayStack() {  //스택 생성자 (ADT의 create() 역할)
        top = -1;       
    }
    ~ArrayStack() {}    //스택 소멸자
    bool isEmpty() {
        return top == -1;
    }
    bool isFull() {
        return top == MAX_STACK_SIZE-1;
    }
    void push(int e) {  //맨 위에 항목 삽입 
        if(isFull()) {
            error("스택 포화 에러");
        }
        data[++top] = e;
    }
    int pop() {     //맨 위의 요소를 삭제하고 반환
        if(isEmpty()) {
            error("스택 공백 에러")
        }
        return data[top--];
    }
    int peek() {    //삭제하지 않고 요소 반환
        if(isEmpty()) {
            error("스택 공백 에러");
        }
        return data[top];
    }
    void display() {    //스택 내용을 화면에 출력
        printf("[스택 항목의 수 = %2d] ==> ", top+1);
        for(int i=0; i<=top; i++) {
            printf("<%2d>", data[i]);
        }
        printf("\n");
        }
    }
}
```
