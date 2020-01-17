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


----------------------------------------------------------------------------------------------------
### 191218 power bi 실습 정리

```
----- 랭킹값 구하는 process----
[‎2019-‎12-‎18 오후 4:47]  Harvey Jung:  
1번 판매량 년단위 누적값 뽑기
CY YTD = TOTALYTD(sum('판매데이터'[ACTUAL]),'날짜테이블'[Date]) -> 년 누적값!!!

2번 작년대비 순위 학인 함수 -> 순서로 만들어줌 
작년대비순위  = RANKX(ALL('대리점'[대리점명]), [CY YTD])

3번 -> 1순위 이름 찾아내는 쿼리 
작년대비순위1위_name = 
VAR RK = ADDCOLUMNS(SUMMARIZE('대리점','대리점'[대리점명])
, "RK"
, [작년대비순위 ]
)
RETURN
FILTER(FILTERS('대리점'[대리점명]),[작년대비순위 ]=1)


4번 랭크1번 뽑기
ACTUAL_ONE = 
VAR rank1 = [작년대비순위1위_name]
RETURN
CALCULATE([CY YTD],'판매데이터'[InvoiceAccountName]=rank1)
 
 
이 대화를 저장했습니다. 잠시 후에 비즈니스용 Skype의 [대화] 탭 및 Outlook의 [대화 내용] 폴더에서 확인할 수 있습니다. 
[‎2019-‎12-‎18 오후 4:47]  Harvey Jung:  
1번 판매량 년단위 누적값 뽑기
CY YTD = TOTALYTD(sum('판매데이터'[ACTUAL]),'날짜테이블'[Date]) -> 년 누적값!!!

2번 작년대비 순위 학인 함수 -> 순서로 만들어줌 
작년대비순위  = RANKX(ALL('대리점'[대리점명]), [CY YTD])

3번 -> 1순위 이름 찾아내는 쿼리 
작년대비순위1위_name = 
VAR RK = ADDCOLUMNS(SUMMARIZE('대리점','대리점'[대리점명])
, "RK"
, [작년대비순위 ]
)
RETURN
FILTER(FILTERS('대리점'[대리점명]),[작년대비순위 ]=1)


4번 랭크1번 뽑기
ACTUAL_ONE = 
VAR rank1 = [작년대비순위1위_name]
RETURN
CALCULATE([CY YTD],'판매데이터'[InvoiceAccountName]=rank1)

1월,2월,3월,1분기,4월... 이런식으로 셀을 몽땅 합쳐야함. 

```


## 인턴/ 개발 시각화 자료

![image](https://user-images.githubusercontent.com/47058441/72401000-3a3a3b80-378e-11ea-82f0-c68847c8415b.png)


## 개발 시각화 자료(2020-01-17)
![image](https://user-images.githubusercontent.com/47058441/72578021-26badc00-3918-11ea-9d90-a5466b51e99f.png)

![image](https://user-images.githubusercontent.com/47058441/72578050-3a664280-3918-11ea-9557-5028b1d0a4ae.png)

![image](https://user-images.githubusercontent.com/47058441/72578082-4eaa3f80-3918-11ea-92cf-ad0baf6fa9fe.png)





