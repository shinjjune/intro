# 꼭 알아둬야 할 자료 구조: 스택 (Stack)

* 데이터를 제한적으로 접근할 수 있는 구조
  *한쪽 끝에서만 자료를 넣거나 뺄 수 있는 구조
* 가장 나중에 쌓은 데이터를 가장 먼저 빼낼 수 있는 데이터 구조
  * 큐: FIFO 정책
  * 스택: LIFO 정책

## 1. 스택 구조
* 스택은 LIFO(Last In, Fisrt Out) 또는 FILO(First In, Last Out) 데이터 관리 방식을 따름
  * LIFO: 마지막에 넣은 데이터를 가장 먼저 추출하는 데이터 관리 정책
  * FILO: 처음에 넣은 데이터를 가장 마지막에 추출하는 데이터 관리 정책
* 대표적인 스택의 활용
  * 컴퓨터 내부의 프로세스 구조의 함수 동작 방식
* 주요 기능
  * push(): 데이터를 스택에 넣기
  * pop(): 데이터를 스택에서 꺼내기
  * Visualgo 사이트에서 시연해보며 이해하기 (push/pop 만 클릭해보며): https://visualgo.net/en/list
  
![image](https://user-images.githubusercontent.com/47058441/73116752-3fae3780-3f7f-11ea-95fb-026727a0f926.png)
