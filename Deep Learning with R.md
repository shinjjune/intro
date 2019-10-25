## Deep Learning with R



### Chapter 1. 딥러닝이란 무엇인가??

![image](https://user-images.githubusercontent.com/47058441/64746375-46935b00-d546-11e9-8e22-f09e896c9e95.png)

* 인공지능: 인간이 일반적으로 수행하는 지적 작업을 자동화하려는 노력
* 머신러닝: 수행 방벙을 아는 모든 일을 넘어서 지정한 작업을 수행하는 방법을 계산기가 스스로 학습할 수 있는가 => 인공지능을 구현하는 구체적 접근 방식
* 딥러닝: 머신러닝의 특정 하위 분야로, 표현들을 점점 더 의미 있게 만들어 가는 연속 계층들을 학습하게 하는데 중점을 두고 데이터 표현을 학습하는 방법 
=> 계층적 표현 학습(layered representations learning), 위계적 표현 학습(hierarchical representations learning)
=> 신경망

머신러닝의 발전 요소
* 하드웨어
* 데이터셋 및 벤치마크
* 알고리즘의 발전

* classification
* regression
* clustering




### Chapter 2. 신경망 둘러보기

keras이용해보자


python 설치 -> anaconda 설치 -> anaconda prompt 실행

* 콘다 및 파이썬 패키지 업데이트
```
> conda update -n base conda
> conda update --all
```

* 텐서플로 설치
텐서플로란?
```
>pip install tensorflow
```
만약 AVX를 지원하지 않는 CPU를 사용하고 있다면 다음과 같이 1.5 버전을 설치합니다.
```
pip install tensorflow==1.5.0
```

jupyter notebook 환경으로 실행하자
```
>jupyter notebook
```




