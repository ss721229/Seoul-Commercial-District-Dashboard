## 프로젝트 소개

### 프로젝트 주제

**서울시 상권 분석 대시보드**

### 주제 선정 목적

- 서울시 상권을 분석함으로써 가게 운영 및 창업에 도움을 줌
- 행정동, 나이, 서비스 등 다양한 주제의 정보 제공으로 인사이트를 제공

## 프로젝트 상세

### 사용 기술 및 프레임워크

- **Storage** : Amazon S3
- **Database(DW)** : Snowflake (SQL)
- **Dashboard** : Preset

### 인프라 아키텍쳐

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e937a7f9-dece-4540-8e1e-3c5966896424/585a3ea9-1182-476b-823b-ee220bd689a2/Untitled.png)

### 데이터 소스

- **서울 열린데이터 광장**
    - **길단위 인구**
        - https://data.seoul.go.kr/dataList/OA-22178/S/1/datasetView.do
        - **데이터 상세(CREATE TABLE)**
            
            ```sql
            CREATE OR REPLACE TABLE seoulmarketdata.raw_data.population (
                quarters VARCHAR(20), # 기준_년분기_코드
                region_code VARCHAR(20), # 행정동_코드
                region VARCHAR(20), # 행정동_코드_명
                pop_total INTEGER, # 총_유동인구_수
                pop_male INTEGER, # 남성_유동인구_수
                pop_female INTEGER, # 여성_유동인구_수
                pop_10s INTEGER, # 연령대_10_유동인구_수
                pop_20s INTEGER, # 연령대_20_유동인구_수
                pop_30s INTEGER, # 연령대_30_유동인구_수
                pop_40s INTEGER, # 연령대_40_유동인구_수
                pop_50s INTEGER, # 연령대_50_유동인구_수
                pop_60s INTEGER, # 연령대_60_유동인구_수
                pop_00to06 INTEGER, # 시간대_00_06_유동인구_수
                pop_06to11 INTEGER, # 시간대_06_11_유동인구_수
                pop_11to14 INTEGER, # 시간대_11_14_유동인구_수
                pop_14to17 INTEGER, # 시간대_14_17_유동인구_수
                pop_17to21 INTEGER, # 시간대_17_21_유동인구_수
                pop_21to24 INTEGER, # 시간대_21_24_유동인구_수
                pop_monday INTEGER, # 월요일_유동인구_수
                pop_tuesday INTEGER, # 화요일_유동인구_수
                pop_wednesday INTEGER, # 수요일_유동인구_수
                pop_thursday INTEGER, # 목요일_유동인구_수
                pop_friday INTEGER, # 금요일_유동인구_수
                pop_saturday INTEGER, # 토요일_유동인구_수
                pop_sunday INTEGER # 일요일_유동인구_수
            );
            ```
            
    - **소득 소비**
        - [https://data.seoul.go.kr/dataList/OA-21278/S/1/datasetView.do](https://data.seoul.go.kr/dataList/OA-22166/S/1/datasetView.do)
        - **데이터 상세(CREATE TABLE)**
            
            ```sql
            CREATE OR REPLACE TABLE seoulmarketdata.raw_data.consume (
                quarters VARCHAR(20), # 기준_년분기_코드
                region_code VARCHAR(20), # 행정동_코드
                region VARCHAR(20), # 행정동_코드_명
                monthly_income INTEGER, # 월_평균_소득_금액
                income_grade SMALLINT, # 소득_구간_코드
                con_total INTEGER, # 지출_총금액
                con_grocery INTEGER, # 식료품_지출_총금액
                con_cloth INTEGER, # 의류_신발_지출_총금액
                con_household INTEGER, # 생활용품_지출_총금액
                con_medical INTEGER, # 의료비_지출_총금액
                con_transport INTEGER, # 교통_지출_총금액
                con_education INTEGER, # 교육_지출_총금액
                con_entertaion INTEGER, # 유흥_지출_총금액
                con_culture INTEGER, # 여가_문화_지출_총금액
                con_other INTEGER,  # 기타_지출_총금액
                con_food INTEGER # 음식_지출_총금액
            );
            ```
            
    - 점포
        - https://data.seoul.go.kr/dataList/OA-22172/S/1/datasetView.do
        - **데이터 상세(CREATE TABLE)**
            
            
    - 추정 매출
        - https://data.seoul.go.kr/dataList/OA-22175/S/1/datasetView.do

## 프로젝트 결과

### **길단위 인구 대시보드**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e937a7f9-dece-4540-8e1e-3c5966896424/c5facc9f-abc9-4e0e-8a01-de3a3b11edfb/Untitled.png)

해당 분기의 행정동별 성별의 길단위 인구와 서울시 전체 길단위 인구를 파악할수 있다 이를 바탕으로 어떤 점포를 열고 어느 성별을 타겟팅 할지를 선택할수 있는 데이터를 얻을수있다

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e937a7f9-dece-4540-8e1e-3c5966896424/3d5ea284-d666-4b47-ab9d-27f531829b9c/Untitled.png)

요일별 길단위 인구를 파악하여 어느 요일에 길단위 인구가 많은지 확인할수 있다. 서울시 전체 요일별 길단위 인구를 참조하여 해당 행정동과 서울시 전체의 행정동의 길단위 인구차이를 확인할수 있다

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e937a7f9-dece-4540-8e1e-3c5966896424/362cc4f2-1aed-40d1-b98c-7b778c49d1e8/Untitled.png)

10대 부터 60대까지의 행정동별 데이터를 한눈에 파악가능하게 중첩되는 그래프로 생성해 이를 바탕으로 어느 행정동에서 어떤 세대의 길단위 인구가 큰지 파악하여 점포 창업에 도움을 받을수 있다

서울시 나이대별 길단위 인구를 분기별 라인그래프형식으로 만들어 분기별 길단위 인구의 세대별 변화를 확인할수 있다

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e937a7f9-dece-4540-8e1e-3c5966896424/94e14f02-ca07-427f-9da1-2de402ad24e5/Untitled.png)

행정동 시간대별 길단위 인구를 파악하여 어느 시간대의 길단위 인구가 많은지 파악을 할수있다.

서울시 전체의 시간대별 길단위인구를 확인하여 서울시 길단위 인구를 파악하여 운영에 도움이 되는 정보를 얻을수 있다

### **소비 대시보드**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e937a7f9-dece-4540-8e1e-3c5966896424/a06caebe-4be2-4b39-a8d9-19dded171afd/Untitled.png)

지역별 소비 데이터를 기반으로 서울시 주민들이 어떤 지역에서 소비를 가장 많이 하는지를 알 수 있습니다.  

소비 상위 지역의 다양한 분야 별 소비 분포를 알 수 있습니다. 이는 사용자로 하여금 어떤 지역에서 어떤 업종이 유리한 지를 판단할 수 있게 도와줍니다.

분기별로 시민들의 분야별 소비 변화 추이를 나타낸 그래프 입니다. 사용자는 어떠한 분야에 사람들이 소비를 많이하고, 그 분야가 성장세인지, 하락세인지를 판단하도록 도와줍니다.

### **점포 대시보드**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e937a7f9-dece-4540-8e1e-3c5966896424/b9003aae-5314-4d9e-a857-b381c347dd03/Untitled.png)

서울시 업종별 개업,폐업수를 나타내는 그래프
하늘색은 개업수를 나타내며, 남색은 폐업수를 나타내고 있습니다. 이 그래프를 확인하여 서울시에서 어떤 업종이 개업이 많이 되고 폐업이 많이 되는지 확인할 수 있습니다

서울시 어느 지역에 많은 점포들이 분석하는 차트로 총 263000개의 매장이 있고 역삼1동, 신당동 … 이렇게 점포가 활발하게 있는 것을 확인 할 수 있습니다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e937a7f9-dece-4540-8e1e-3c5966896424/f613b6a1-e04b-4a36-9525-5081b09d3e4f/Untitled.png)

서울시 전체 점포수를 지도에 적용하여 보기 편하게 만들었습니다. 지도에는 서울시 행정구역의 위도 경도를 넣어서 가시화 했습니다

서울시 프랜차이즈 비중을 나타내는 그래프로 x좌표는 총 매장수 y좌표는 프랜차이즈 수 포인트 크기는 프랜차이즈 퍼센트로 만들어 보기 쉽게 만들었습니다.

### **추정 매출 대시보드**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e937a7f9-dece-4540-8e1e-3c5966896424/35534f81-c0ba-4210-bbed-1ed06d70509e/Untitled.png)

2023년 서울시의 서비스 업종 별 평균 매출 금액, 최대 매출 금액, 최소 매출 금액을 나타내었습니다. 최소 매출 금액의 경우 최대 매출 금액과 차이가 크기 때문에 logarithmic을 사용하였습니다. 대체로 최대 매출 금액과 평균 매출금액의 그래프 형태가 비슷하였으며, 평균 매출 금액이 가장 높은 것은 한식음식점(9.52B), 그 다음이 수산물판매(6.71B)이었습니다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e937a7f9-dece-4540-8e1e-3c5966896424/9b49c8de-f17e-47d6-9fa2-6c92433e4b42/Untitled.png)

2023년 서울시의 나이에 따라 매출 금액과 매출 건수의 변동을 확인할 수 있도록 하였습니다. 나이에 따라 매출이 높은 서비스 업종을 확인할 수 있으며, 서비스 업종에 따라 고객 타겟팅을 하는데 도움이 될 수 있을 것입니다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e937a7f9-dece-4540-8e1e-3c5966896424/529ca50b-e444-4e9a-8181-59e8c13dafeb/Untitled.png)

2023년 서울시 길단위 인구 테이블을 JOIN하여 길단위 인구에 따른 행정동 별 매출 금액을 나타내었습니다. 특정 몇 개의 행정동이 큰 매출 금액을 나타내었고, 가락1동의 경우(왼쪽 중앙의 연주황색 원) 길단위 인구가 타 행정동에 비해 매우 적지만 높은 매출 금액을 나타내었습니다.

## 참여자 정보 및 역할

안재영(팀장) : 

서상민 : 2023년 서울시 추정 매출 데이터 가공 및 시각화

이승준 : 서울시 점포 관련 데이터 가공 및 시각화

좌상원 : 데이터 적재, 서울시 소득/소비 데이터 가공 및 시각화

## 느낀점(넣어도 되고 안 넣어도 됩니다)

안재영 : 

서상민 : 

이승준 : 

좌상원 : 

## 개선점

### 데이터 적재 자동화 구현

- 현재 분기 별로 저장된 데이터를 S3에 “직접” 적재한 뒤 사용하는 방식을 사용
- 현재 상황에서 자동화 기능 구현 방법을 탐색 및 공유 (실제로 구현 X)
    - github-action
    - 로컬 혹은 EC2에서 crontab
    - AWS EventBridge + Lambda

### Preset 대시보드 구현

- Preset 사용에 미숙해서 생각했던 차트를 모두 구현하지 못함
- 그러나 데이터 부분이 더 중요하기에 데이터 적재 자동화를 우선 탐색
