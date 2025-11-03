# SQL_BASIC 5주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_5th_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**5주차 과제는 문제 풀이를 중심으로**, 강의에서 제시된 예제 문제 중 **3 문제 이상을 선택하여 직접 풀어본 뒤**, 강의 영상의 풀이와 비교해 **틀린 부분, 맞은 부분, 새롭게 배운 개념**을 구체적으로 정리해주세요. (적어도 4문제는 정리해야 합니다.) 완성된 과제는 Gihub에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**(수행 인증샷은 필수입니다.)** 



## SQL_BASIC_5th

### 섹션 5. 데이터 탐색 - 변환

### 4-4. 날짜 및 시간 데이터 이해하기(2) (EXTRACT, DATETIME_TRUNC, PARSE_DATETIME, FROMAT_DATETIME)

### 4-5. 시간 데이터 연습문제 1~2번

### 4-5. 시간 데이터 연습문제 3~5번

### 4-6. 조건문 (CASE WHEN, IF)

### 4-7. 조건문 연습 문제

### 4-8. 정리

### 4-9. BigQuery 공식 문서 확인하는 법

(강의에서 연습문제가 많아서 따로 프로그래머스 문제 과제는 없습니다.)



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | ✅         |
| 5주차 | 섹션 **4-4** ~ **4-9** | ✅         |
| 6주차 | 섹션 **5-1** ~ **5-7** | 🍽️         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<br>



<!-- 여기까진 그대로 둬 주세요-->

---

# 4-4. 날짜 및 시간 데이터 이해하기(2) (EXTRACT, DATETIME_TRUNC, PARSE_DATETIME, FROMAT_DATETIME)

~~~
✅ 학습 목표 :
* 날짜 및 시간 데이터에 대해서 더 자세히 설명할 수 있다. 
* CURRENT_TIME, EXTRACT, DATETIME_TRUNC, PARSE_DATETIME, FROMAT_DATETIME 을 설명할 수 있다. 
~~~

## DATETIME 함수

### CURRENT_DATETIME
- CURRENT_DATETIME([time_zone]): 현재 datetime 출력
```sql
SELECT
 # 연-월-일 출력
 CURRENT_DATE() AS current_date,
 # 연-월-일 출력
 CURRENT_DATE("Asia/Seoul") AS asia_date,
 # 연-월-일 시간 출력
 CURRENT_DATETIME() AS current_datetime,
 # 연-월-일 시간(time_zone 반영) 출력
 CURRENT_DATETIME("Asia/Seoul") AS current_datetime_asia;
```

### EXTRACT
- DATETIME에서 특정 부분만 추출하고 싶은 경우
- 월별 주문, 일자별 주문 등
```sql
SELECT
 # 연-월-일 출력
 EXTRACT(DATE FROM DATETIME "2024-01-02 14:00:00") AS date,
 # 년도만 출력
 EXTRACT(YEAR FROM DATETIME "2024-01-02 14:00:00") AS year,
 # 달만 출력
 EXTRACT(MONTH FROM DATETIME "2024-01-02 14:00:00" AS DATETIME) AS month,
 # 날짜만 출력
 EXTRACT(DAY FROM DATETIME "2024-01-02 14:00:00" AS DATETIME) AS day,
 # 시간만 출력
 EXTRACT(HOUR FROM DATETIME "2024-01-02 14:00:00" AS DATETIME) AS hour,
 # 분만 출력
 EXTRACT(MINUTE FROM DATETIME "2024-01-02 14:00:00" AS DATETIME) AS minute;

 # 요일만 출력: 한 주의 첫날이 일요일인 [1,7] 범위의 값을 반환
 EXTRACT(DAYOFWEEK FROM datetime_col)
```

### DATETIME_TRUNC
- DATE와 HOUR만 남기고 싶은 경우: 시간 자르기
```sql
SELECT
 # 날짜 이하는 0으로 출력
 DATETIME_TRUNC(DATETIME "2024-03-02 14:42:13", DAY) AS day_trunc,
 # 년도 이하는 0으로 출력
 DATETIME_TRUNC(DATETIME "2024-03-02 14:42:13", YEAR) AS year_trunc,
 # 달 이하는 0으로 출력
 DATETIME_TRUNC(DATETIME "2024-03-02 14:42:13", MONTH) AS month_trunc,
 # 시간 이하는 0으로 출력
 DATETIME_TRUNC(DATETIME "2024-03-02 14:42:13", HOUR) AS hour_trunc;
```

### PARSR_DATETIME
- **문자열**로 저장된 DATETIME을 **DATETOME 타입**으로 바꾸고 싶은 경우
- %A, %a등의 의미는 문서 확
```sql
PARSE_DATETIME('문자열의 형태', 'DATETIME 문자열') AS datetime
```

### FORMAT_DATETIME
- **DATETIME 타입** 데이터를 특정 형태의 **문자열** 데이터로 변환하고 싶은 경우
```sql
SELECT
 FORMAT_DATETIME("%c", DATETIME "2024-01-11 12:35:35") AS formatted;
```

### LAST_DAY
- 마지막 날을 알고 싶은 경우: 자동으로 월의 마지막 값을 계산해서 특정 연산을 할 경우
- LAST_DAY(DATETIME): 월의 마지막 값을 반환
```sql
SELECT
 # 1월의 마지막 날짜 출력
 LAST_DAY(DATETIME '2024-01-03 15:30:00') AS last_day,
 # 1월의 마지막 날짜 출력
 LAST_DAY(DATETIME '2024-01-03 15:30:00', MONTH) AS last_month,
 # 해당 주의 토요 날짜 출력
 LAST_DAY(DATETIME '2024-01-03 15:30:00', WEEK) AS last_day_week,
 # 해당 주의 토요일 날짜 출력
 LAST_DAY(DATETIME '2024-01-03 15:30:00', WEEK(SUNDAY)) AS last_day_week_sun,
 # 해당 주의 일요일 날짜 출력
 LAST_DAY(DATETIME '2024-01-03 15:30:00', WEEK(MONDAY)) AS last_day_week_mon;
```

### DATETIME_DIFF
- 두 DATETIME의 차이를 알고싶은 경우
- DATETIME_DIFF(첫 DATETIME, 두번째 DATETIME, 궁금한 차이)
```sql
SELECT
 # 두 datetime의 차
 DATETIME_DIFF(first_datetime, second_datetime, DAY) AS day_diff1,
 # 두 datetime의 차
 DATETIME_DIFF(second_datetime, first_datetime, DAY) AS day_diff2,
 # 두 datetime의 달 수의 차
 DATETIME_DIFF(first_datetime, second_datetime, MONTH) AS month_diff,
 # 두 datetime의 주 수 차
 DATETIME_DIFF(first_datetime, second_datetime, WEEK) AS week_diff,
FROM(
 SELECT
  DATETIME "2024-04-02 10:20:00" AS first_datetime,
  DATETIME "2024-01-01 15:30:00" AS second_datetime,
)
```

# 4-6. 조건문(CASE WHEN, IF)

~~~
✅ 학습 목표 :
* 조건문 함수의 기능을 이해하고, 설명할 수 있다. 
~~~

## 조건문 함수
- 특정 조건이 충족되면, 어떤 행동을 취함
- 특정 조건이 참일 때 A, 거짓일 때 B
  - 조건에 따른 분기 처리가 필요
- 조건에 따라 다른 값을 표시하고 싶을 때 사용
  - 컬럼을 변환하고 싶을 때 사용
  - 예를 들면, height >= 180이면 '정말 크다'라고 출력

### 조건문을 사용하는 방법
- CASE_WHEN
- IF

### 조건문 함수가 사용되는 이유
- 특정 카테고리를 하나로 합치는 전처리가 필요할 수 있음
  - 예를 들면, 1/2/3은 저학년으로, 4/5/6을 고학년으로 구분
- 데이터를 저장하는 쪽과 데이터를 분석하는 쪽이 나뉘는 경우가 있음
  - 이런 경우, 분석할 때 필요한 부분에서 조건을 설정해서 변경하는 것이 더 유용
  - 저장할 때부터 특정 카테고리를 합쳐서 저장하면, 쪼개서 보고 싶을 때 볼 수 없음
 
### 조건문: CASE_WHEN
- **여러 조건**이 있을 경우 유용
- 조건1, 조건2 둘 다에 해당하면 앞선 순서를 따름
- 문자열 함수(예: 특정 단어 추출)에서 이슈가 자주 발생
  - 데이터가 순서에 따라 결과가 달라질 수 있음음
- **조건의 순서**에 주의해야 함

```sql
# 문법

SELECT
 CASE
  WHEN 조건1 THEN 조건1이 참일 경우 결과
  WHEN 조건2 THEN 조건2가 참일 경우 결과
  ELSE 그 외 조건일 경우 결과
END AS 새로운_컬럼_이름
```

```sql
# 예시1: Rock 타입과 Ground 타입이 비슷하다 -> Rock&Ground 타입 새로 만들기

SELECT
  *,
  CASE
    WHEN type1 IN ("Rock", "Ground") OR type2 IN ("Rock", "Ground") THEN "Rock&Ground"
  ELSE type1
  END AS new_type1
FROM basic.pokemon
```

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/75b561ab-6bbf-414c-b4d3-2a93f034b4fd" />

```sql
# 예시2: 각 포켓몬의 공격력을 기준으로, 50 이상이면 'Strong', 100 이상이면 'Very Strong', 그 이하면 'Weak'으로 분류

SELECT
 eng_name,
 attack,
 CASE
  WHEN attack >= 100 THEN 'Very Strong'
  WHEN attack >= 50 THEN 'Strong'
  ELSE 'Weak'
 END AS attack_level
FROM basic.pokemon
```

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/eaf16eb3-3310-4c34-aaaf-c1ed88c88431" />

### 조건문: IF
- **단일 조건**일 경우 유용

```sql
# 문법

IF(조건문, True일 때의 값, False일 때의 값) AS 새로운_컬럼_이름
```



 # 4-5. 시간 데이터 연습문제 & 4-7. 조건문 연습 문제

~~~
✅ 학습 목표 :
* 4-5, 4-7 각각에서 두 문제 이상 (최소 4문제) 푼 내용 정리하기
~~~

## 4-5 1번 문제
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/e9f2f227-c5b0-49e7-8f8a-09e3377d7fbd" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/43e5f30a-5019-4fd8-9c1e-078d8476f3d1" />

- 데이터 타입이 컬럼명을 따르지 않을 수 있으므로 데이터의 특징은 직접 보고 판단해야 함

## 4-5 2번 문제

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/0ebf11a3-8503-406b-885e-09a702f50512" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/f56a7ef3-6482-497b-9009-da8ff61c14c6" />

- BETWEEN a and b: a와 b 사이에 있는 것을 반환

## 4-7 1번 문제

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/5f524f1c-adea-445e-b0ae-e48693953ecd" />

- 단일 조건일 때는 IF 사용

## 4-7 2번 문제

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/356da982-1a75-4aea-96ba-d5637f4ec006" />

- 여러 조건일 때는 CASE_WHEN 사용

<br>

<br>

---

# 확인문제

## 문제 1

> **🧚Q. 광윤이는 사용자 로그 데이터에서, 2021년에 접속한 사용자 수를  집계하려고 했습니다. 그는 여러 SQL 쿼리들을 실행해봤지만, 그 중 일부는 문법적으로 잘못되어 실행되지 않았습니다. 다음 보기 중 틀린 쿼리를 모두 골라보세요 (복수 선택 가능)**

~~~sql
1. SELECT COUNT(*)  
   FROM user_log  
   WHERE EXTRACT(YEAR FROM login_date) = 2021;

2. SELECT EXTRACT(YEAR FROM login_date), COUNT(*)  
   FROM user_log  
   GROUP BY EXTRACT(YEAR FROM login_date);

3. SELECT COUNT(*)  
   FROM user_log  
   WHERE login_date = '2021';

4. SELECT COUNT(*)  
   FROM user_log  
   WHERE login_date BETWEEN '2021-01-01' AND '2021-12-31';
~~~

<!-- 틀린쿼리에 대한 오류의 원인도 같이 작성해주세요. 문제에서 제공된 login_data 컬럼은 DATE type의 데이터를 가지고 있다고 가정하시면 됩니다. -->

~~~
여기에 답을 작성해주세요!
~~~



## 문제 2

> **🧚Q. 혜성이는 포켓몬 타입에 따라 설명을 부여하는 쿼리를 작성했습니다. type 1 컬럼의 값에 따라 조건을 분기했으며, 다음 SQL 쿼리를 실행했습니다.**

~~~sql
SELECT name,
       CASE 
         WHEN type1 = 'Fire' THEN 'Hot'
         WHEN type1 = 'Water' THEN 'Cool'
         ELSE 'Normal'
       END AS type_description
FROM pokemon;
~~~

> **다음 중 type_description의 결과가 'Normal'로 출력될 포켓몬은?**

| **name**   | **type1** |
| ---------- | --------- |
| Pikachu    | Electric  |
| Charmander | Fire      |
| Squirtle   | Water     |
| Bulbasaur  | Grass     |

<!-- 근거와 함께 답을 작성해주세요 -->

~~~
여기에 답을 작성해주세요!
~~~



<br>

### 🎉 수고하셨습니다.
