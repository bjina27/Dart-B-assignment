# SQL_BASIC 7주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_7th_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**7주차 과제는 강의 내용을 정리하는 것과 함께, 프로그래머스에서 제공하는 SQL 문제를 직접 풀어보는 실습도 병행합니다.** 강의에서는 **배운 내용을 정리하고 주요 쿼리 예제를 정리**하며, 프로그래머스 문제는 **직접 풀어본 뒤 풀이 과정과 결과, 배운 점을 함께 기록**해주세요. 완성된 과제는 Github에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**(수행 인증샷은 필수입니다.)** 

**(마지막 주차입니다. 조금만 더 힘내주세요~!!!)**

## SQL_BASIC_7th

### 섹션 7 데이터 결과 검증, 가독성 있는 쿼리 작성하기

### 6-1. Intro

### 6-2. 가독성을 챙기기 위한 SQL 스타일 가이드

### 6-3. 가독성을 챙기기 위한 WITH 문 & 파티션

### 6-4. 데이터 결과 검증 정의

### 6-5. 데이터 결과 검증 예시

### 6-6. 정리 



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | ✅         |
| 5주차 | 섹션 **4-4** ~ **4-9** | ✅         |
| 6주차 | 섹션 **5-1** ~ **5-7** | ✅         |
| 7주차 | 섹션 **6-1** ~ **6-6** | ✅         |

<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 개념정리

## 6-2. 가독성을 챙기기 위한 SQL 스타일 가이드

~~~
✅ 학습 목표 :
* 데이터 결과 검증하기 전에 실수가 발생하는 원인을 설명할 수 있다.
* SQL 쿼리를 가독성 있게 작성할 수 있다. 
~~~

### 데이터 결과 검증을 하기 전에 해야할 것들

#### 실수가 발생하는 경우
- 문법을 잘못 알고 있는 경우
  - 문법 공부
- 데이터를 파악하지 않고 쿼리를 작성하는 경우
  - 데이터의 메타 정보가 저장되면 좋음
- 쿼리가 복잡한 경우
  - 단순화

#### 타인과 쿼리를 공유하는 경우
- 쿼리를 가독성있게 작성해야 함
  - 별도의 설명이 없어도 이해할 수 있도록
- 쿼리를 변경해야하는 경우, 특정 부분만 바꾼건지 전체를 바꾼건지에 대해 파악하는 것이 좋음

### 대표적인 SQL 스타일 가이드

#### 예약어는 대문자로 작성
- sql에서 문법적인 용도로 사용하고 있는 문자들은 대문자로 작성
  - 예: SELECT, FROM, WHERE, 각종 함수

#### 컬럼 이름은 snake_case로 작성
- 컬럼 이름은 CamelCase가 아닌 snake_case로 작성
- 회사의 기준에 따른 일관성이 중요

#### 명시적 vs 암시적 이름
- Alias로 별칭을 지을 때는 명시적인 이름을 적용
- AS a, AS b과 같이 컬럼의 의미가 명확하지 않은 것이 아닌 명시적인 이름을 사용
- JOIN할 때 테이블의 이름도 명시적으로 진행하는 편이 좋음
- AS를 생략해서 별칭을 설정할 수도 있음
  - AS를 쓰는 것도 명시적 표현
 
#### 왼쪽 정렬
- 기본적으로 왼쪽 정렬을 기준으로 작성

#### 예약어나 컬럼은 한 줄에 하나씩 권장
- 컬럼은 바로 주석 처리할 수 있는 장점이 있기에, 한 줄에 하나씩 작성
- 예시
```sql
SELECT
  col1,
  col2,
  col3
FROM table
WHERE
``` 

#### 쉼표는 컬럼 바로 뒤에
- 의견이 갈리는 부분: 쉼표 앞 vs 쉼표 뒤
- BigQuery는 마지막 쉼표를 무시해서 뒤에 작성해도 무

## 6-3. 가독성을 챙기기 위한 WITH문 & 파티션

~~~
✅ 학습 목표 :
* SQL 쿼리를 가독성 있게 작성할 수 있다. 
* WITH문과 파티션을 활용해서도 가독성을 챙길 수 있다. 
~~~

### WITH 구문

#### SQL 쿼리를 작성하다 생기는 일
- 점점 복잡해지며 가독성 하락
  - 만약 아래 쿼리가 다른곳에서도 필요하면 복

#### WITH 구문

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/8eee4fba-b7a5-4320-82a9-e53427acd37c" />

- WITH문을 사용하여 쿼리를 정의해서 **재사용** 가능
- WITH 뒤에 별칭 정의
- CTE(Common Table Expression)이라고 표현
- SELECT 구문에 이름을 정해주는 것과 유사
- 쿼리 내에서 반복적으로 사용할 수 있음
- 사용 방법
```sql
WITH name_a AS(
  SELECT
    col
  FROM Table
), name_b AS(
  SELECT
    col2
  FROM Table2
)
SELECT
  col
FROM name_a
```

### PARTITION
- tavle에는 partition이라는 것이 존재할 수 있음

#### PARTITION의 장점
- 쿼리 성능 향상
  - 전체 데이터를 스캔하는 것보다 파티션을 설정한 곳만 스캔하는 것이 더 빠름
- 데이터 관리 용이성
  - 특정 일자의 데이터를 모두 변경하거나 삭제해야 하면 파티션을 설정해서 삭제할 수 있음
- 비용
  - 파티션에 해당되는 데이터만 스캔해서 비용 절감 가능

## 6-4. 데이터 결과 검증 정의 

~~~
✅ 학습 목표 :
* 데이터 결과 검증이 어떤 과정인지 설명할 수 있다. 
* 데이터 결과 검증에 대한 예시를 이해할 수 있다.  
~~~

### 데이터 결과 검증을 잘하기 위한 마인드셋
- 데이터 결과 검증을 하지 못해서 실수할 수 있음
- 실수가 반복되는 것이 문제
- 실수가 반복되지 않도록 항상 생각해야 함..

### 데이터 결과 검증의 정의
- SQL 쿼리 후 얻은 결과가 예상과 일치하는지 확인하는 과정
- 목적: 분석 결과의 정확성, 신뢰성 확보
- 방법
  - 기대하는 예상 결과를 정의
  - 쿼리 작성
  - 두 개가 일치하는지 비교
- 중요한 부분
  - 문제를 잘 정의하고, 미리 작성해보기
  - 도메인 특수성(규칙 등)을 잘 파악하기
  - SQL 쿼리 템플릿과 맥락이 유사
 
### 데이터 결과를 검증하는 흐름
1. 문제 정의 확인
   - 구체적인 문제 정의
   - 요청 사항도 구체적으로 확인
2. Input/Output
   - 데이터의 Input과 (원하는 형태의) Output 작성
   - Input과 Output 사이에 중간 결과 생각하기
3. 쿼리 작성
   - 가독성 챙기기
4. 결과 비교
   - 예상과 실제 쿼리 결과의 차이가 있는지 확인
   - 오류가 있다면 다시 쿼리 작성
   - 오류가 없다면 종결
  
### 데이터 결과를 검증할 때 자주 활용하는 SQL 쿼리

#### 대표적으로 활용하는 SQL 문법
1. COUNT(*)
   - 행 수를 확인, 의도한 데이터의 행 개수가 맞는가?
2. NOT NULL
   - 특정 컬럼에 NULL값이 존재하는가? 필수 필드가 비어있지는 않은가?
3. DISTINCT
   - 데이터의 고유값을 확인하여 중복 여부 확인
4. IF문, CASE WHEN
   - 의도와 같다면 TRUE, 아니면 FALSE
5. SELECT COUNT(DISTINCT col), COUNT(col) 두 컬럼을 보고 개수를 비

#### 데이터 결과 검증을 할 때 활용하는 방식(강사 사용)
1. 특정 user_id로 필터링을 걸어서 확인
2. 샘플 데이터 생성하기 


<br>

<br>

---

# 2️⃣ 확인문제 & 문제 인증

## 프로그래머스 문제 

https://school.programmers.co.kr/learn/courses/30/lessons/157343

> 특정 옵션이 포함된 자동차 리스트 구하기

https://school.programmers.co.kr/learn/courses/30/lessons/59044

> 오랜 기간 보호한 동물(1) 

https://school.programmers.co.kr/learn/courses/30/lessons/59043

> 있었는데요 없었습니다.



## LeetCode 문제

https://leetcode.com/problems/combine-two-tables/description/

> 175. Combine Two Tables

https://leetcode.com/problems/list-the-products-ordered-in-a-period/

> 1327. List the Products Ordered in a Period



<img width="1242" height="438" alt="image" src="https://github.com/user-attachments/assets/ad0d1d62-81dc-403d-8e66-3205754c3b0c" />

<img width="1226" height="428" alt="image" src="https://github.com/user-attachments/assets/2349ca3d-93ab-491e-bc9f-c40b111548be" />

<img width="1190" height="470" alt="image" src="https://github.com/user-attachments/assets/a4506d14-a216-4968-9874-d9363da17e94" />

<img width="1336" height="424" alt="image" src="https://github.com/user-attachments/assets/081f4add-a4e9-4d0f-9392-96de969f8ecf" />

<img width="1318" height="360" alt="image" src="https://github.com/user-attachments/assets/a26e8fa4-2377-48d2-887b-73cef170ec10" />


## 문제 1

> **🧚예운이는 다음 SQL 쿼리를 다트비 정규과제에 제출했다. 제출한 쿼리는 다음과 같고, 이 쿼리는 에러 메시지 없이 잘 수행하는 쿼리이다.**

~~~sql
# 주영이가 작성한 가독성 나쁜 SQL 

select u.name , o.OrderID
, p.ProductName ,od.Quantity ,od.UnitPrice 	from Users u	join Orders o on u.id = o.userId
join OrderDetails od on o.OrderID = od.orderID	join Products p on od.ProductID = p.ProductID
where u.region= 'Busan'			order by o.OrderID
~~~

> **이에 과제를 검사하던 정우는 작성한 SQL을 보고 코드 리뷰를 진행하려고 했지만, 다음 쿼리를 보고 예운이에게 질문을 하였다. "예운아, 이 쿼리 가독성이 좀 안 좋은데 내가 고쳐도 괜찮을까? 가독성 좋게 SQL 가이드에 따라 정리해보려고 해"**
>
> 다음 SQL 쿼리를 **가독성 좋은 스타일로 다시 작성해보세요.** 



~~~
SELECT 
    u.name,
    o.OrderID,
    p.ProductName,
    od.Quantity,
    od.UnitPrice
FROM 
    Users AS u
    JOIN Orders AS o 
        ON u.id = o.userId
    JOIN OrderDetails AS od 
        ON o.OrderID = od.orderID
    JOIN Products AS p 
        ON od.ProductID = p.ProductID
WHERE 
    u.region = 'Busan'
ORDER BY 
    o.OrderID;
~~~



<br>

<br>

<!-- 이렇게 SQL BASIC 과제가 마무리되었습니다. SQL은 범위가 넓고 처음 접할 때 어렵게 느껴지는 학문이지만, 이번 기수에서는 지난 기수에도 활용했던 인프런 무료 강의를 통해 기초를 탄탄히 다질 수 있도록 구성했습니다. 환경 세팅 과정이 다소 복잡했을 수도 있지만, 이번 과제를 통해 기본적인 쿼리 작성과 SELECT 명령어의 개념을 충분히 익혔을 거라 생각합니다. BASIC을 통해 데이터를 추출하는 기초를 다졌으면, 이제 ADVANCED 트랙에서 실무에 맞게 더 복잡한 문접과 분석 쿼리, 그리고 MASTER 트랙에서 실제 데이터베이스를 다루는 실무 명령어까지 배워갈 수 있습니다. 앞으로의 SQL 학습에도 화이팅이고, 부족한 템플릿이었지만 끝까지 함께해줘서 진심으로 감사드립니다.  -->

### 🎉 수고하셨습니다.
