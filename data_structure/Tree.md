# 대표적인 데이터 구조7: 트리
## 1. 트리 (Tree) 구조

* 트리: Node와 Branch를 이용해서, 사이클을 이루지 않도록 구성한 데이터 구조
* 실제로 어디에 많이 사용되나?
    * 트리 중 이진 트리 (Binary Tree) 형태의 구조로, 탐색(검색) 알고리즘 구현을 위해 많이 사용됨
## 2. 알아둘 용어

* Node: 트리에서 데이터를 저장하는 기본 요소 (데이터와 다른 연결된 노드에 대한 Branch 정보 포함)
* Root Node: 트리 맨 위에 있는 노드
* Level: 최상위 노드를 Level 0으로 하였을 때, 하위 Branch로 연결된 노드의 깊이를 나타냄
* Parent Node: 어떤 노드의 다음 레벨에 연결된 노드
* Child Node: 어떤 노드의 상위 레벨에 연결된 노드
* Leaf Node (Terminal Node): Child Node가 하나도 없는 노드
* Sibling (Brother Node): 동일한 Parent Node를 가진 노드
* Depth: 트리에서 Node가 가질 수 있는 최대 Level

![image](https://user-images.githubusercontent.com/47058441/73604198-f6d62e80-45cf-11ea-87a5-888443609512.png)

## 3. 이진 트리와 이진 탐색 트리 (Binary Search Tree)
* 이진 트리: 노드의 최대 Branch가 2인 트리
* 이진 탐색 트리 (Binary Search Tree, BST): 이진 트리에 다음과 같은 추가적인 조건이 있는 트리
    * 왼쪽 노드는 해당 노드보다 작은 값, 오른쪽 노드는 해당 노드보다 큰 값을 가지고 있음!

## 4. 자료 구조 이진 탐색 트리의 장점과 주요 용도
* 주요 용도: 데이터 검색(탐색)
* 장점: 탐색 속도를 개선할 수 있음
    
    단점은 이진 탐색 트리 알고리즘 이해 후에 살펴보기로 함

## 5. 파이썬 객체지향 프로그래밍으로 링크드 리스트 구현하기
### 5.1. 노드 클래스 만들기
```
class Node:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None
```
### 5.2. 이진 탐색 트리에 데이터 넣기
* 이진 탐색 트리 조건에 부합하게 데이터를 넣어야 함
```
class NodeMgmt:
    def __init__(self, head):
        self.head = head
    
    def insert(self, value):
        self.current_node = self.head
        while True:
            if value < self.current_node.value:
                if self.current_node.left != None:
                    self.current_node = self.current_node.left
                else:
                    self.current_node.left = Node(value)
                    break
            else:
                if self.current_node.right != None:
                    self.current_node = self.current_node.right
                else:
                    self.current_node.right = Node(value)
                    break
```
```
head = Node(1)
BST = NodeMgmt(head)
BST.insert(2)
```
### 5.3. 이진 탐색 트리 탐색
```
class NodeMgmt:
    def __init__(self, head):
        self.head = head
    
    def insert(self, value):
        self.current_node = self.head
        while True:
            if value < self.current_node.value:
                if self.current_node.left != None:
                    self.current_node = self.current_node.left
                else:
                    self.current_node.left = Node(value)
                    break
            else:
                if self.current_node.right != None:
                    self.current_node = self.current_node.right
                else:
                    self.current_node.right = Node(value)
                    break
    
    def search(self, value):
        self.current_node = self.head
        while self.current_node:
            if self.current_node.value == value:
                return True
            elif value < self.current_node.value:
                self.current_node = self.current_node.left
            else:
                self.current_node = self.current_node.right
        return False        
 ```
 ```
 head = Node(1)
BST = NodeMgmt(head)
BST.insert(2)
BST.insert(3)
BST.insert(0)
BST.insert(4)
BST.insert(8)
```
```
BST.search(-1)
```
### 5.4. 이진 탐색 트리 삭제
* 매우 복잡함. 경우를 나누어서 이해하는 것이 좋음
### 5.4.1. Leaf Node 삭제
* Leaf Node: Child Node 가 없는 Node
* 삭제할 Node의 Parent Node가 삭제할 Node를 가리키지 않도록 한다

![image](https://user-images.githubusercontent.com/47058441/73604232-68ae7800-45d0-11ea-99f6-10609f18c9cf.png)

### 5.4.2. Child Node 가 하나인 Node 삭제
* 삭제할 Node의 Parent Node가 삭제할 Node의 Child Node를 가리키도록 한다

![image](https://user-images.githubusercontent.com/47058441/73604236-7b28b180-45d0-11ea-816a-1cf50d549fea.png)

### 5.4.3. Child Node 가 두 개인 Node 삭제
1. 삭제할 Node의 오른쪽 자식 중, 가장 작은 값을 삭제할 Node의 Parent Node가 가리키도록 한다.
2. 삭제할 Node의 왼쪽 자식 중, 가장 큰 값을 삭제할 Node의 Parent Node가 가리키도록 한다

![image](https://user-images.githubusercontent.com/47058441/73604242-8ed41800-45d0-11ea-8568-54d85c1109bf.png)

### 5.4.3.1. 삭제할 Node의 오른쪽 자식중, 가장 작은 값을 삭제할 Node의 Parent Node가 가리키게 할 경우
* 삭제할 Node의 오른쪽 자식 선택
* 오른쪽 자식의 가장 왼쪽에 있는 Node를 선택
* 해당 Node를 삭제할 Node의 Parent Node의 왼쪽 Branch가 가리키게 함
* 해당 Node의 왼쪽 Branch가 삭제할 Node의 왼쪽 Child Node를 가리키게 함
* 해당 Node의 오른쪽 Branch가 삭제할 Node의 오른쪽 Child Node를 가리키게 함
* 만약 해당 Node가 오른쪽 Child Node를 가지고 있었을 경우에는, 해당 Node의 본래 Parent Node의 왼쪽 Branch가 해당 오른쪽 Child Node를 가리키게 함

## 5.5. 이진 탐색 트리 삭제 코드 구현과 분석
### 5.5.1 삭제할 Node 탐색
* 삭제할 Node가 없는 경우도 처리해야 함
    * 이를 위해 삭제할 Node가 없는 경우는 False를 리턴하고, 함수를 종료 시킴
 
```
# def delete(self, value):
    searched = False
    self.current_node = self.head
    self.parent = self.head
    while self.current_node:
        if self.current_node.value == value:
            searched = True
            break
        elif value < self.current_node.value:
            self.parent = self.current_node
            self.current_node = self.current_node.left
        else:
            self.parent = self.current_node
            self.current_node = self.current_node.right
    
    if searched == False:
        return False
    
    ### 이후부터 Case들을 분리해서, 코드 작성
```
### 5.5.2. Case1: 삭제할 Node가 Leaf Node인 경우

![image](https://user-images.githubusercontent.com/47058441/73604251-c0e57a00-45d0-11ea-9e18-1a5d7d496b30.png)

```
# self.current_node 가 삭제할 Node, self.parent는 삭제할 Node의 Parent Node인 상태
    if  self.current_node.left == None and self.current_node.right == None:
        if value < self.parent.value:
            self.parent.left = None
        else:
            self.parent.right = None
        del self.current_node
```
### 5.5.2. Case2: 삭제할 Node가 Child Node를 한 개 가지고 있을 경우

![image](https://user-images.githubusercontent.com/47058441/73604255-dce91b80-45d0-11ea-9edf-71bb0d653a7c.png)

```
if self.current_node.left != None and self.current_node.right == None:
        if value < self.parent.value:
            self.parent.left = self.current_node.left
        else:
            self.parent.right = self.current_node.left
    elif self.current_node.left == None and self.current_node.right != None:
        if value < self.parent.value:
            self.parent.left = self.current_node.right
        else:
            self.parent.right = self.current_node.right
```
### 5.5.3. Case3-1: 삭제할 Node가 Child Node를 두 개 가지고 있을 경우 (삭제할 Node가 Parent Node 왼쪽에 있을 때)
* 기본 사용 가능 전략
  1. 삭제할 Node의 오른쪽 자식 중, 가장 작은 값을 삭제할 Node의 Parent Node가 가리키도록 한다.
  2. 삭제할 Node의 왼쪽 자식 중, 가장 큰 값을 삭제할 Node의 Parent Node가 가리키도록 한다.
* 기본 사용 가능 전략 중, 1번 전략을 사용하여 코드를 구현하기로 함
    * 경우의 수가 또다시 두가지가 있음
        * Case3-1-1: 삭제할 Node가 Parent Node의 왼쪽에 있고, 삭제할 Node의 오른쪽 자식 중, 가장 작은 값을 가진 Node의 Child Node가 없을 때
        * Case3-1-2: 삭제할 Node가 Parent Node의 왼쪽에 있고, 삭제할 Node의 오른쪽 자식 중, 가장 작은 값을 가진 Node의 오른쪽에 Child Node가 있을 때
            * 가장 작은 값을 가진 Node의 Child Node가 왼쪽에 있을 경우는 없음, 왜냐하면 왼쪽 Node가 있다는 것은 해당 Node보다 더 작은 값을 가진 Node가 있다는 뜻이기 때문임

![image](https://user-images.githubusercontent.com/47058441/73604262-0144f800-45d1-11ea-9bf6-465f41d94e3c.png)
```
  if self.current_node.left != None and self.current_node.right != None: # case3
        if value < self.parent.value: # case3-1
            self.change_node = self.current_node.right
            self.change_node_parent = self.current_node.right
            while self.change_node.left != None:
                self.change_node_parent = self.change_node
                self.change_node = self.change_node.left
            if self.change_node.right != None:
                self.change_node_parent.left = self.change_node.right
            else:
                self.change_node_parent.left = None
            self.parent.left = self.change_node
            self.change_node.right = self.current_node.right
            self.change_node.left = self.change_node.left
```




