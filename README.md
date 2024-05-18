## 프로젝트 소개

---

### 프로젝트 주제

**서울시 상권 분석 대시보드**

### 주제 선정 목적

- 사업의 시작은 **많은 조사를 필요**로 합니다. 각 지역의 상권마다 업종별 매출 차이, 주 고객층의 소비 패턴, 유동인구 수등 상당히 많고 **다양한 지표를 종합적으로 고려**해야 보다 안전하고 성공적인 사업을 운영할 수 있습니다.
- 저희 팀은 **사업을 시작하시려는 분들이나 이미 가게를 운영하고 계신 사장님들**께서 서울시 상권의 다양한 데이터를 보다 쉽게 확인할 수 있도록 데이터 전처리 및 시각화작업을 진행하고, **누구나 쉽게 사용할 수 있는 대시보드**를 제공하여 의사결정에 있어서 실질적인 도움을 드리고자 이번 주제를 기획하게 되었습니다.

### 기대효과

1. **효율적인 의사결정 지원 :** 다양한 지표를 시각화하여 제공함으로써, 자영업자들이 빠르고 정확하게 정보를 분석하고 합리적인 의사결정을 내릴 수 있습니다.
2. **사업 성공률 향상** : 업종별, 지역별 매출 차이와 주 고객층의 소비 패턴 등을 고려하여 적합한 사업 전략을 세울 수 있어, 성공적인 사업 운영 가능성이 높아집니다.
3. **시간 및 비용 절감** : 복잡한 데이터를 손쉽게 확인하고 분석할 수 있어, 자영업자들이 조사에 드는 시간과 비용을 절감할 수 있습니다.
4. **경쟁력 강화**: 상권 분석을 통해 경쟁 업체와의 차별화 전략을 수립하고, 보다 효과적인 마케팅 및 운영 전략을 실행할 수 있습니다.

### 참여자 정보 및 역할

안재영(팀장) : 서울시 길단위 인구 데이터 가공 및 시각화

서상민 : 2023년 서울시 추정 매출 데이터 가공 및 시각화

이승준 : 서울시 점포 관련 데이터 가공 및 시각화

좌상원 : 데이터 적재, 서울시 소득/소비 데이터 가공 및 시각화

## 프로젝트 상세

---

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
            -- raw_data
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
            
            -- Analysis 스키마에 대시보드를 만들기위해 제작된 테이블
            -- 행정동 별 성별 이동인구
            CREATE TABLE seoulmarketdata.ANALYSIS.POP_GENDER AS 
            SELECT 
                QUARTERS, 
                REGION, 
                POP_MALE, 
                POP_FEMALE 
            FROM seoulmarketdata.raw_data.population;
            
            -- 행정동 별 나이별 이동인구
            CREATE TABLE seoulmarketdata.ANALYSIS.POP_AGE AS 
            SELECT 
                QUARTERS, 
                REGION, 
                POP_10S,
            	POP_20S,
            	POP_30S,
            	POP_40S,
            	POP_50S,
            	POP_60S
            FROM seoulmarketdata.raw_data.population;
            
            -- 행정동 별 시간별 이동인구
            CREATE TABLE seoulmarketdata.ANALYSIS.POP_TIME AS 
            SELECT 
                QUARTERS, 
                REGION, 
                pop_00to06,
                pop_06to11,
                pop_11to14,
                pop_14to17,
                pop_17to21,
                pop_21to24 
            FROM seoulmarketdata.raw_data.population;
            
            -- 행정동 별 요일별 이동인구
            CREATE TABLE seoulmarketdata.ANALYSIS.POP_WEEK AS 
            SELECT 
                QUARTERS, 
                REGION, 
                pop_monday,
                pop_tuesday,
                pop_wednesday,
                pop_thursday,
                pop_friday,
                pop_saturday,
                pop_sunday
            FROM seoulmarketdata.raw_data.population;
            
            -- 서울시 총 성별 이동인구
            SELECT 
                concat(substr(QUARTERS,1,4),'년',substr(QUARTERS,5,6),'분기') AS "년/분기", 
                SUM(POP_MALE) AS "남성_유동인구", 
                SUM(POP_FEMALE) AS "여성_유동인구" 
            FROM seoulmarketdata.analysis.pop_gender GROUP BY "년/분기" limit 5;
            
            -- 서울시 나이별 이동인구
            SELECT 
                concat(substr(QUARTERS,1,4),'년',substr(QUARTERS,5,6),'분기') AS "년/분기", 
                SUM(POP_10S) AS "10대",
            	SUM(POP_20S) AS "20대",
            	SUM(POP_30S) AS "30대",
            	SUM(POP_40S) AS "40대",
            	SUM(POP_50S) AS "50대",
            	SUM(POP_60S) AS "60대 이상"
            FROM seoulmarketdata.analysis.pop_age GROUP BY QUARTERS limit 5;
            
            -- 서울시 시간별 이동인구
            SELECT 
                concat(substr(QUARTERS,1,4),'년',substr(QUARTERS,5,6),'분기') AS "년/분기", 
                SUM(pop_00to06) AS "00 ~ 06", 
                SUM(pop_06to11) AS "06 ~ 11",
                SUM(pop_11to14) AS "11 ~ 14",
                SUM(pop_14to17) AS "14 ~ 17",
                SUM(pop_17to21) AS "17 ~ 21",
                SUM(pop_21to24) AS "21 ~ 24"
            FROM seoulmarketdata.analysis.pop_time GROUP BY QUARTERS limit 5;
            
            -- 서울시 요일별 이동인구
            SELECT 
                concat(substr(QUARTERS,1,4),'년',substr(QUARTERS,5,6),'분기') AS "년/분기", 
                SUM(pop_monday) AS "월요일 유동인구",
                SUM(pop_tuesday) AS "화요일 유동인구",
                SUM(pop_wednesday) AS "수요일 유동인구",
                SUM(pop_thursday) AS "목요일 유동인구",
                SUM(pop_friday) AS "금요일 유동인구",
                SUM(pop_saturday) AS "토요일 유동인구",
                SUM(pop_sunday) AS "일요일 유동인구"
            FROM seoulmarketdata.analysis.pop_week GROUP BY QUARTERS limit 5;
            
            -- 대시보드 생성과정에서 row단위로 그래프를 생성할수 없어 
            -- data | 분기 | colname | 지역 | 형태로 select한 결과를 
            -- union시켜 새로운 테이블을 생성
            -- 행정동별 라인 차트 생성 테이블
            CREATE TABLE seoulmarketdata.ANALYSIS.SEOUL_POP_TIME AS
            SELECT pop_00to06 AS pop_value, QUARTERS, '00 ~ 06'  AS col, REGION
            FROM pop_time 
            UNION ALL
            SELECT pop_06to11 AS pop_value, QUARTERS, '06 ~ 11' AS col, REGION 
            FROM pop_time 
            UNION ALL
            SELECT pop_11to14 AS pop_value, QUARTERS, '11 ~ 14' AS col, REGION 
            FROM pop_time 
            UNION ALL
            SELECT pop_14to17 AS pop_value, QUARTERS, '14 ~ 17' AS col, REGION 
            FROM pop_time 
            UNION ALL
            SELECT pop_17to21 AS pop_value, QUARTERS, '17 ~ 21' AS col, REGION
            FROM pop_time 
            UNION ALL
            SELECT pop_21to24 AS pop_value, QUARTERS, '21 ~ 24' AS col, REGION
            FROM pop_time ;
            
            -- 서울시 전체 성별 길단위 인구
            CREATE TABLE seoulmarketdata.ANALYSIS.SEOUL_TOT_POP_GENDER AS
            SELECT sum(pop_male) AS pop_value, QUARTERS, 'MALE' AS col
            FROM pop_gender GROUP BY QUARTERS
            UNION ALL
            SELECT sum(pop_female) AS pop_value, QUARTERS, 'FEMALE' AS col
            FROM pop_gender GROUP BY QUARTERS; 
            
            -- 서울시 전체 요일별 길단위 인구
            CREATE TABLE seoulmarketdata.ANALYSIS.SEOUL_TOT_POP_WEEK AS
            SELECT sum(pop_monday) AS pop_value, QUARTERS, '월요일' AS col
            FROM pop_week GROUP BY QUARTERS
            UNION ALL
            SELECT sum(pop_tuesday) AS pop_value, QUARTERS, '화요일' AS col
            FROM pop_week GROUP BY QUARTERS
            UNION ALL
            SELECT sum(pop_wednesday) AS pop_value, QUARTERS, '수요일' AS col
            FROM pop_week GROUP BY QUARTERS
            UNION ALL
            SELECT sum(pop_thursday) AS pop_value, QUARTERS, '목요일' AS col
            FROM pop_week GROUP BY QUARTERS
            UNION ALL
            SELECT sum(pop_friday) AS pop_value, QUARTERS, '금요일' AS col
            FROM pop_week GROUP BY QUARTERS
            UNION ALL
            SELECT sum(pop_saturday) AS pop_value, QUARTERS, '토요일' AS col
            FROM pop_week GROUP BY QUARTERS
            UNION ALL
            SELECT sum(pop_sunday) AS pop_value, QUARTERS, '일요일' AS col
            FROM pop_week GROUP BY QUARTERS;
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
            
            CREATE OR REPLACE TABLE seoulmarketdata.analysis.consume_average AS
            SELECT 
                region,
                ROUND(AVG(con_grocery),0) AS "식료품 평균",
                ROUND(AVG(con_cloth),0) AS "의류비 평균",
                ROUND(AVG(con_household),0) AS "생활용품 평균",
                ROUND(AVG(con_medical),0) AS "의료비 평균",
                ROUND(AVG(con_education),0) AS "교육비 평균",
                ROUND(AVG(con_entertain),0) AS "유흥비 평균",
                ROUND(AVG(con_food),0) AS "음식 소비 평균",
                ROUND(AVG(con_total),0) AS "총 매출 평균"
            FROM seoulmarketdata.analysis.consume
            GROUP BY region;
            
            CREATE OR REPLACE TABLE seoulmarketdata.analysis.consume_by_quarters AS
            SELECT 
                quarters,
                ROUND(AVG(con_grocery),0) AS "식료품 평균",
                ROUND(AVG(con_cloth),0) AS "의류비 평균",
                ROUND(AVG(con_household),0) AS "생활용품 평균",
                ROUND(AVG(con_medical),0) AS "의료비 평균",
                ROUND(AVG(con_education),0) AS "교육비 평균",
                ROUND(AVG(con_entertain),0) AS "유흥비 평균",
                ROUND(AVG(con_food),0) AS "음식 소비 평균",
                ROUND(AVG(con_total),0) AS "총 매출 평균"
            FROM seoulmarketdata.analysis.consume
            GROUP BY quarters;
            ```
            
    - **점포**
        - https://data.seoul.go.kr/dataList/OA-22172/S/1/datasetView.do
        - **데이터 상세(CREATE TABLE)**
            
            ```sql
            CREATE OR REPLACE TABLE seoulmarketdata.raw_data.store (
                quarters VARCHAR(20), # 기준_년분기_코드
                region_code VARCHAR(20), # 행정동_코드
                region VARCHAR(20), # 행정동_코드_명
                service_code VARCHAR(20), # 서비스_업종_코드
                service VARCHAR(20), # 서비스_업종_코드_명
                store_count INTEGER, # 점포_수
                similar_store_count INTEGER, # 유사_업종_점포_수
                opening_rate INTEGER, # 개업_율
                opening_store_count INTEGER, # 개업_점포_수
                closure_rate INTEGER, # 폐업_률
                closure_store_count INTEGER, # 폐업_점포_수
                franchise_store_count INTEGER # 프랜차이즈_점포_수
            );
            ```
            
    - **추정 매출**
        - https://data.seoul.go.kr/dataList/OA-22175/S/1/datasetView.do
        - **데이터 상세(CREATE TABLE)**
            
            ```sql
            CREATE OR REPLACE TABLE seoulmarketdata.raw_data.sales (
                quarters VARCHAR(20), # 기준_년분기_코드
                region_code VARCHAR(20), # 행정동_코드
                region VARCHAR(20), # 행정동_코드_명
                service_code VARCHAR(20), # 서비스_업종_코드
                service VARCHAR(20), # 서비스_업종_코드_명
                sales_monthly INTEGER, # 당월_매출_금액
                sales_monthly_count INTEGER, # 당월_매출_건수
                sales_weekday INTEGER, # 주중_매출_금액
                sales_weekend INTEGER, # 주말_매출_금액
                sales_monday INTEGER, # 월요일_매출_금액
                sales_tuesday INTEGER, # 화요일_매출_금액
                sales_wednesday INTEGER, # 수요일_매출_금액
                sales_thursday INTEGER, # 목요일_매출_금액
                sales_friday INTEGER, # 금요일_매출_금액
                sales_saturday INTEGER, # 토요일_매출_금액
                sales_sunday INTEGER, # 일요일_매출_금액
                sales_00to06 INTEGER, # 시간대_00~06_매출_금액
                sales_06to11 INTEGER, # 시간대_06~11_매출_금액
                sales_11to14 INTEGER, # 시간대_11~14_매출_금액
                sales_14to17 INTEGER, # 시간대_14~17_매출_금액
                sales_17to21 INTEGER, # 시간대_17~21_매출_금액
                sales_21to24 INTEGER, # 시간대_21~24_매출_금액
                sales_male INTEGER, # 남성_매출_금액
                sales_female INTEGER, # 여성_매출_금액
                sales_10s INTEGER, # 연령대_10_매출_금액
                sales_20s INTEGER, # 연령대_20_매출_금액
                sales_30s INTEGER, # 연령대_30_매출_금액
                sales_40s INTEGER, # 연령대_40_매출_금액
                sales_50s INTEGER, # 연령대_50_매출_금액
                sales_60s INTEGER, # 연령대_60_이상_매출_금액
                sales_weekday_count INTEGER, # 주중_매출_건수
                sales_weekend_count INTEGER, # 주말_매출_건수
                sales_monday_count INTEGER, # 월요일_매출_건수
                sales_tuesday_count INTEGER, # 화요일_매출_건수
                sales_wednesday_count INTEGER, # 수요일_매출_건수
                sales_thursday_count INTEGER, # 목요일_매출_건수
                sales_firday_count INTEGER, # 금요일_매출_건수
                sales_saturday_count INTEGER, # 토요일_매출_건수
                sales_sunday_count INTEGER, # 일요일_매출_건수
                sales_00to06_count INTEGER, # 시간대_건수~06_매출_건수
                sales_06to11_count INTEGER, # 시간대_건수~11_매출_건수
                sales_11to14_count INTEGER, # 시간대_건수~14_매출_건수
                sales_14to17_count INTEGER, # 시간대_건수~17_매출_건수
                sales_17to21_count INTEGER, # 시간대_건수~21_매출_건수
                sales_21to24_count INTEGER, # 시간대_건수~24_매출_건수
                sales_male_count INTEGER, # 남성_매출_건수
                sales_female_count INTEGER, # 여성_매출_건수
                sales_10s_count INTEGER, # 연령대_10_매출_건수
                sales_20s_count INTEGER, # 연령대_20_매출_건수
                sales_30s_count INTEGER, # 연령대_30_매출_건수
                sales_40s_count INTEGER, # 연령대_40_매출_건수
                sales_50s_count INTEGER, # 연령대_50_매출_건수
                sales_60s_count INTEGER # 연령대_60_이상_매출_건수
            );
            ```
            
    - **서울시 행정동 중심점**
        - **데이터 상세(CREATE TABLE)**
            
            ```sql
            CREATE OR REPLACE TABLE seoulmarketdata.raw_data.coordinates (
                 region_code INTEGER,
                 city VARCHAR(20),
                 gu VARCHAR(20),
                 dong VARCHAR(20),
                 X FLOAT,
                 Y FLOAT
            );
            ```
            
        

### 데이터 웨어하우스 구조

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e937a7f9-dece-4540-8e1e-3c5966896424/4829fb9e-9182-4b69-a4c8-ef2a2b10b975/Untitled.png)

---

## 프로젝트 결과

---

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

## 기술적 개선점

---

### 데이터 적재 자동화 구현

- 현재 분기 별로 저장된 데이터를 S3에 “직접” 적재한 뒤 사용하는 방식을 사용함
    - 해당 방법은 데이터를 하나하나 일정 기간마다 직접 최신화시켜야된다는 문제점이 있음
    - 이를 해결하기위해 데이터 파이프라인을 구축해 자동화를 기획
- 현재 상황에서 자동화 기능 구현 방법을 탐색 및 공유 (실제로 구현 X)
    - github-action
    - 로컬 혹은 EC2에서 crontab
        - crontab : 리눅스용 작업 스케줄러로 특정 시각에 명령어를 반복 수행 가능
        - EC2 : AWS에서 제공하는 클라우드 컴퓨팅 서비스
        - crontab을 활용하면, s3에 적재하는 코드를 일정 시간마다 실행시켜 자동화할 수 있다. 이에 더해 EC2 서버 위에서 crontab을 작성한다면, 로컬이 아닌 클라우드 환경에서 본인의 컴퓨터 동작과 관계없이 crontab 코드를 지속적으로 실행할 수 있다.
    - AWS EventBridge + Lambda
        - Lambda를 이용해 파이썬 코드를 작성해 데이터를 원하는 데이터를 받아 csv파일로 처리 후 s3와 연결해 해당 데이터를 업데이트하는 기능을 구현한뒤 AWS 에서 지원하는 EventBridge를 이용해 해당 기능을 원하는 간격으로 수행하도록 설정하면 원하는 간격으로 데이터를 자동으로 가져와 처리하는 파이프라인이 구축됨
        

### Preset 대시보드 구현

- Preset 사용에 미숙해서 생각했던 차트를 모두 구현하지 못함
- 그러나 데이터 부분이 더 중요하기에 데이터 적재 자동화를 우선 탐색

## 느낀점

- 안재영 : 2차 프로젝트 준비하면서 어렵다는 소리를 많이들어서 걱정했는데 팀원들이 알아서 착착 잘 진행해줘서 편하게 작업했습니다
- 서상민 : 1차 프로젝트는 웹을 제작하느라 시간이 오래 걸렸는데, 2차 프로젝트는 웹 제작 없이 대시보드만 제작이 끝이라 편한 마음으로 임할 수 있었다. 오히려 프로젝트와 관련하여 추가할 것을 생각해보는 시간을 가질 수 있어서 좋았다.
- 이승준 : 좋은 팀원분들을 만나 빨리 작업할 수 있었으며 그만큼 좋은 결과물을 만들 수 있었다고 생각합니다. 자동화 부분을 못한것이 아쉽지만 다음 프로젝트는 자동화를 제대로 적용시켜 더욱 완벽한 프로젝트를 만들고 싶습니다
- 좌상원 : 팀원분들 다 열정적으로 맡은 바 잘 처리해 주셔서 너무 편하고 수월한 프로젝트였습니다. 데일리 스크럼을 통해 의견 교류하고 완료한 일, 해야할 일 정리하면서 프로젝트를 진행하니 진행 방향도 명확하고, 제가 해결하지 못한 부분도 빠르게 해결되어 다시한번 정기적인 회의가 중요하다고 생각했던 프로젝트였습니다. 데이터 자동화 부분을 깔끔하게 구현하지 못한 것이 좀 아쉽긴 하네요… Airflow를 다음주에 배우는데 그걸 미리 공부해서 적용해보았다면 어땠을까 하는 아쉬움이 있습니다.
