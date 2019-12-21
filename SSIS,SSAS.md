# 20191221

## SSIS, SSAS - 권태돈 강사

* Business Intelligenece
* Data Mart(데이타 통합) - 차원 모델링
* SQL Server Integration Services(SSIS, 데이터 통합 도구) - 제어흐름

* SQL Server Analysis Services(SSAS, 대량의 데이터 신속하게...) - 데이터 형식 모델, 유지관리

 

OLTP(On-Line **Transaction** Processing) : 기간계 시스템

->쪼개놓는다--> DB정규화!



Data Warehouse

Data Mart(쉬움)



처음할일: 기간계, data 원본 파악





## SSMS

* sa 의 password 찾기

1. windows 인증(관리자)
2. 보안탭
3. 로그인탭(비밀번호 변경가능)
4. 데이터베이스->복원->AdventureWorks2016, AdventureWorksDW2016 복원





#### DW란??(대기업, 공공기관)

=> 규모가 방대함...한 회사의 모든 데이터를 통합

##### 정의

* 의사 결정 지원에 사용될 수 있는 응용 프로그램들이  활용하는 정보 기반을 제공하는 하나의 **통합**된 데이터 저장공간
* 다양한 운영 환경의 시스템들로부터 데이터들을 추출(E)/변환(T)/적재(L) 해서 요약한 읽기 전용 데이터베이스

##### 특징

* 주제 지향적
* 통합성
* 비휘발성
* 시계열성

#### Data Mart(중소기업)

=>  리포트를 만들기 위한 데이터 마트, 리포트에 필요한 요소만 추출하면 됨
