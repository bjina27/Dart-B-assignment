# SQL_BASIC 6주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_6th_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**6주차 과제는 강의 내용을 정리하는 것과 함께, 프로그래머스에서 제공하는 SQL 문제를 직접 풀어보는 실습도 병행합니다.** 강의에서는 **배운 내용을 정리하고 주요 쿼리 예제를 정리**하며, 프로그래머스 문제는 **직접 풀어본 뒤 풀이 과정과 결과, 배운 점을 함께 기록**해주세요. 완성된 과제는 Github에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**(수행 인증샷은 필수입니다.)** 

## SQL_BASIC_6th

### 섹션 6. 다량의 자료를 연결 : JOIN 

### 5-1. Intro

### 5-2. JOIN 이해하기

### 5-3. 다양한 JOIN 방법

### 5-4. JOIN 쿼리 작성하기 

### 5-5. JOIN을 처음 공부할 때 헷갈렸던 부분

### 5-6. JOIN 연습문제 1~2번

### 5-6. JOIN 연습문제 3~5번

### 5-7. 정리



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | ✅         |
| 5주차 | 섹션 **4-4** ~ **4-9** | ✅         |
| 6주차 | 섹션 **5-1** ~ **5-7** | ✅         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<!-- 여기까진 그대로 둬 주세요-->

<br>

---

# 1️⃣ 개념정리

## 5-2. JOIN 이해하기

~~~
✅ 학습 목표 :
* JOIN에 대한 정의와 필요성에 대해 설명할 수 있다.
~~~

### JOIN의 개념
1. SQL JOIN: 서로 다른 데이터 테이블을 연결하는 것
2. 공통적으로 존재하는 컬럼(Key)이 있다면 JOIN 할 수 있음
  - 보통 id값을 Key로 많이 사용하고, 특정 범위로 JOIN 가능

### JOIN을 해야하는 이유
1. 관계형 데이터베이스(RDBMS) 설계시 정규화 과정을 거침
   - 정규화는 중복을 최소화하게 데이터를 구조화
   - User Table은 유저 데이터만, Order Table은 주문 데이터만
   - 따라서 데이터를 다양한 Table에 저장해서 필요할 때 JOIN해서 사용
2. 데이터 분석하는 관점에선 미리 JOIN되어 있는 것이 좋을 수도 있지만, 개발 관점에서는 분리되어 있는 것이 좋음
   - 대신 데이터 웨어하우스에서 JOIN + 필요한 연산을 해서 데이터 마트를 만들어 활

## 5-3. 다양한 JOIN 방법

~~~
✅ 학습 목표 :
* JOIN 방법들의 종류를 설명할 수 있다. 
* 각 JOIN 방법들의 차이점에 대해서 설명할 수 있다. 
~~~

### INNER JOIN

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/aa7708f8-2688-4d51-9e55-ecac24e868da" />
<img width="200" height="100" alt="image" src="https://github.com/user-attachments/assets/d245057d-83bf-4758-a094-0acd69fadbbc" />

- 두 테이블의 공통요소만 연결


### LEFT/RIGHT JOIN

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/aa7708f8-2688-4d51-9e55-ecac24e868da" />
<img width="200" height="180" alt="image" src="https://github.com/user-attachments/assets/f7f9d40b-6705-4679-9fc3-7481220e05a4" />
<img width="200" height="180" alt="image" src="https://github.com/user-attachments/assets/6f74dd41-7dc1-4765-878f-5f3f9b27d60a" />

- 왼쪽/오른쪽 테이블 기준으로 연결


### FULL (OUTER) JOIN

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/aa7708f8-2688-4d51-9e55-ecac24e868da" />
<img width="200" height="210" alt="image" src="https://github.com/user-attachments/assets/8bfc9b18-9b63-4f5c-b797-aa355d4a6baf" />

- 양쪽 기준으로 연결

### CROSS JOIN

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/aa7708f8-2688-4d51-9e55-ecac24e868da" />
<img width="200" height="400" alt="image" src="https://github.com/user-attachments/assets/1f2d8881-d9e8-4246-9d91-60dd9424cd7d" />

- 두 테이블의 각각의 요소를 곱하기

### JOIN 집합 관점으로 생각하기

<img width="500" height="200" alt="image" src="https://github.com/user-attachments/assets/fef03fdf-dfae-4832-9a67-a017d3eeb1d8" />


## 5-4. JOIN 쿼리 작성하기 

~~~
✅ 학습 목표 :
* JOIN을 사용한 문법에 대해 이해하여 적용할 수 있다.
* JOIN 을 활용한 쿼리를 작성할 수 있다. 
~~~

### JOIN 쿼리 작성 흐름

| 단계 | 단계명 | 내용 |
|------|---------|------|
| 1 | 테이블 확인 | 테이블에 저장된 데이터, 컬럼 확인 |
| 2 | 기준 테이블 정의 | 가장 많이 참고할 기준 테이블 정의 |
| 3 | JOIN Key 찾기 | 여러 Table과 연결할 Key(ON) 정리 |
| 4 | 결과 예상하기 | 결과 테이블을 예해서 손, 엑셀로 작성 |
| 5 | 쿼리 작성 / 검증 | 예상한 결과와 동일한 결과가 나오는지 확인 |

### JOIN 문법

- FROM 하단에 JOIN할 Table을 작성하고 ON 뒤에 공통된 컬럼(Key)를 작성
- 테이블 이름이 길면 별칭(Alias)를 사용할 수 있음
  
```sql
SELECT
  A.col1,
  A.col2,
  B.col1,
  B.col2
FROM table1 AS A
LEFT JOIN table2 AS B
ON A.key # Alias를 사용할 수 있음
```

### JOIN별 쿼리 예시

| JOIN 종류 | ON 필수 여부 | 쿼리 예시 |
|------------|--------------|------------|
| **INNER JOIN** | O | 아래 예시 참고 |
| **LEFT/RIGHT JOIN** | O | 아래 예시 참고 |
| **FULL JOIN** | O | 아래 예시 참고 |
| **CROSS JOIN** | X | 아래 예시 참고 |

**INNER JOIN**
```sql
SELECT
  col
FROM table_a AS A
INNER JOIN table_b AS B
ON A.key = B.key;
```

**LEFT/RIGHT JOIN**
```sql

SELECT
  col
FROM table_a AS A
LEFT/RIGHT JOIN table_b AS B
ON A.key = B.key;
```

**FULL JOIN**
```sql
SELECT
  col
FROM table_a AS A
FULL JOIN table_b AS B
ON A.key = B.key;
```

**CROSS JOIN**
```sql
SELECT
  col
FROM table_a AS A
CROSS JOIN table_b AS B;
```


## 5-6. JOIN 연습문제 1~5번 

~~~
✅ 학습 목표 :
* 연습문제(3문제 이상) 푼 것들 정리하기
~~~

1. 트레이너가 보유한 포켓몬들은 얼마나 있는지 알 수 있는 쿼리를 작성하라.
```sql
SELECT
  tp.*,
  p.id,
  p.kor_name
  FROM (
  SELECT
    id,
    trainer_id,
    pokemon_id,
    status
FROM basic.trainer_pokemon
WHERE
  status IN ("Active", "Training")
) AS tp
LEFT JOIN basic.pokemon AS p
ON tp.pokemon_id = p.id
```
<img width="500" height="200" alt="image" src="https://github.com/user-attachments/assets/6a298fa1-70a3-426b-9e0b-3b476c2cb837" />

2. 각 트레이너가 가진 포켓몬 중에서 'Grass' 타입의 포켓몬 수를 계산하시오.
```sql
SELECT
  tp.*,
  p.type1
  FROM (
  SELECT
    id,
    trainer_id,
    pokemon_id,
    status
FROM basic.trainer_pokemon
WHERE
  status IN ("Active", "Training")
) AS tp
LEFT JOIN basic.pokemon AS p
ON tp.pokemon_id = p.id
WHERE
  type1 = "Grass"
```
<img width="500" height="200" alt="image" src="https://github.com/user-attachments/assets/f32cba5c-22a0-44f3-a898-e43881aa047b" />

3. 트레이너의 고향과 포켓몬을 포획한 위치를 비교하여, 자신의 고향에서 포켓몬을 포획한 트레이너의 수를 계산하라.
```sql
SELECT
  COUNT(DISTINCT tp.trainer_id) AS trainer_uniq,
FROM basic.trainer AS t
LEFT JOIN basic.trainer_pokemon AS tp
ON t.id = tp.trainer_id
WHERE
  tp.location IS NOT NULL
  AND t.hometown = tp.location
```
<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/fb5f19f1-6365-4283-9114-00c23a28d825" />

<br>

<br>

---

# 2️⃣ 확인문제 & 문제 인증

## 프로그래머스 문제 

https://school.programmers.co.kr/learn/courses/30/lessons/164673

> 조건에 부합하는 중고거래 댓글 조회하기 (JOIN)

https://school.programmers.co.kr/learn/courses/30/lessons/144854

> 조건에 맞는 도서와 저자 리스트 출력하기 (JOIN)

<img width="1398" height="512" alt="image" src="https://github.com/user-attachments/assets/a602b84a-494a-4d44-b8a2-8114f60dd139" />
<img width="1384" height="598" alt="image" src="https://github.com/user-attachments/assets/041680ea-4315-4358-9014-4822237f3d0e" />



---

# 3️⃣ 참고자료

JOIN 에 대해서 그림으로 쉽게 이해할 수 있는 자료들도 있어서 첨부합니다. 아래의 블로그도 학습할 때 같이 참고해주세요.

1. https://data-marketing-bk.tistory.com/entry/SQL-JOIN-%ED%95%9C-%EB%B0%A9%EC%97%90-%EC%A0%95%EB%A6%AC-%EA%B0%9C%EB%85%90%EB%B6%80%ED%84%B0-%EC%BD%94%EB%93%9C%EA%B9%8C%EC%A7%80-%EC%9D%B4%EA%B2%83%EB%A7%8C-%EB%B3%B4%EC%9E%90



2. https://velog.io/@wijoonwu/JOIN

<br>

### 🎉 수고하셨습니다.
