# Power BI

<h2>"5분만에 탄성을!"</h2>


![image](https://user-images.githubusercontent.com/47058441/70487720-a72c3700-1b39-11ea-840e-f3dd70148607.png)


* 조직 전체에 통찰력 있는 정보를 제공하는 비즈니스 분석 도구
* 다양한유형의 데이터 원본에 연결하고, 데이터를 정리하여 분석 수행
* 시각화 보고서를 만들어 조직에서 웹,앱 장치를 통해 정보 공유
* 대시보드를 손쉽게 구성하고 구성원과 공유

power platform (<======power BI+power Apps+power Flow(Automation))

#### Powre BI Service(서비스 포털)+ Power BI Desktop(모델링 및 보고서 작성)+ Power BI Mobile(데이터 뷰)

클라우드 서비스

import  ->  보고서 -> DB -> 공유


### M language


### DAX(Data Analysis eXpression)

* power Bi 의 데이터 분석식 작성언어
* SSAS Tabular, Excel PowerPivot, Power BI Desktop
* Excel 함수와 유사, 더 복잡한 분석 함수 제공

새 열, 새 측정값, 새 테이블...

### 191211

* 드릴스루: 필터값도 적용된다. 
* 도구설명: 

<도구설명 페이지>

페이지크기 설정, 페이지정보 설정 ,필드설정, 디자인

* 조건부 서식
테이블, 행력 등의 조건부 서식


* 사용자지정 시각화 도구: 
http://store.office.com/

pibviz, pbiz 샘플보고서 참조 가능...


### 데이터 전처리

* 데이터 가져오기(.csv, .web, .excel, .sql...)
* 데이터 변환

자주사용하는 기능
```
1. 행제거
2. 첫 행을 머릿글로 사용
3. [분류] 채우기(아래로)
4. 필터링
5. 피벗 적용 및 해제
6. 불필요한 행 삭제
```

1000개의 데이터 열 프로파일링, 오류 체크.

### 데이타 모델링
```
1. 보고서 뷰에서 숨기기
2. 열 이름 변경
3. 열 제거
4. 데이터 형식 지정
5. 데이터 범주 지정
6. 열 기준 정렬
7. DAX(새 열, 새 측정값)
8. 계층구조
```

* 가져오기 분류명= related('')

dax 연습 코드
```
1. 상위 10 = TOPN(10,SUMMARIZE('판매','분류'[분류명],'제품'[제품분류명],"매출액",[총매출금액]),[총매출금액],DESC)
2. 서울지역매출 = FILTER('판매',RELATED('지역'[시도])="서울특별시")
3. 서울 = CALCULATE(SUM('판매'[매출금액]), '지역'[시도] IN { "서울특별시" })
4. 기타 = 
        VAR __BASELINE_VALUE = CALCULATE(SUM('판매'[매출금액]), '지역'[시도] IN { "서울특별시" })
        VAR __MEASURE_VALUE = SUM('판매'[매출금액])
        RETURN
	        IF(NOT ISBLANK(__MEASURE_VALUE), __MEASURE_VALUE - __BASELINE_VALUE)
5. 기타지역매출 = CALCULATE(SUM('판매'[매출금액]),'지역'[시도]<>"서울특별시")
6. % 분류별 매출 = DIVIDE([총매출금액],CALCULATE([총매출금액],ALL('분류'[분류명])),0)
7. % 분류별 매출1 = DIVIDE([총매출금액],CALCULATE([총매출금액],ALLSELECTED('분류'[분류명])),0)
8. % 거래처별 매출 = DIVIDE([총매출금액],CALCULATE([총매출금액],ALL('거래처')),0)
9. 연간누적 = TOTALYTD([총매출금액],'날짜'[날짜])
10. 이익률 = SUM('판매'[매출이익])/SUM('판매'[매출금액])
11. 전년도매출 = CALCULATE([총매출금액],DATEADD('날짜'[날짜],-1,YEAR))


```




