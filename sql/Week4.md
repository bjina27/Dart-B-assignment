# SQL_BASIC 4주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_4th_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**4주차 과제부터는 강의 내용을 정리하는 것과 함께, 프로그래머스에서 제공하는 SQL 문제를 직접 풀어보는 실습도 병행합니다.** 강의에서는 **배운 내용을 정리하고 주요 쿼리 예제를 정리**하며, 프로그래머스 문제는 **직접 풀어본 뒤 풀이 과정과 결과, 배운 점을 함께 기록**해주세요. 완성된 과제는 Github에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**(수행 인증샷은 필수입니다.)** 

## SQL_BASIC_4th

### 섹션 4. 쿼리 잘 작성하기, 쿼리 작성 템플릿 및 오류를 잘 디버깅하기

### 3-4. 오류를 잘 디버깅하는 방법



## 섹션 5. 데이터 탐색 - 변환

### 4-1. INTRO

### 4-2. 데이터 타입과 데이터 변환(CAST, SAFE_CAST)

### 4-3. 문자열 함수(CONCAT, SPLIT, REPLACE, TRIM, UPPER)

### 4-4. 날짜 및 시간 데이터 이해하기(1) (타임존, UTC, Millisecond, TIMESTAMP/DATETIME)



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | ✅         |
| 5주차 | 섹션 **4-4** ~ **4-9** | 🍽️         |
| 6주차 | 섹션 **5-1** ~ **5-7** | 🍽️         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 개념정리

## 3-4. 오류를 디버깅하는 방법

~~~
✅ 학습 목표 :
* 오류의 정의에 대해 설명할 수 있다. 
* 오류 메시지를 보고 디버깅이라는 과정을 수행할 수 있다. 
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->

### SQL 쿼리 작성 중 발생하는 오류

#### Error의 정의
- 부정확하거나 잘못된 행동을 의미
- 실수와 동의어인 경우도 있음
- 오류메세지가 알려주고자 하는 것
  - 길잡이 역할: 현재 작성한 방식으로는 답을 얻을 수 없음
  - 문제 진단: 이 부분이 문제가 됨


#### Syntax error: 문법 오류
<img src="image-5.png" alt="설명" width="300" height="200">

- 문법을 지키지 않아 생기는 오류
- Error Message를 해석 후, 해결 방법 탐색
  - 구글 검색
  - ChatGPT 질문
    - 데이터 예시나 쿼리를 제공 후 오류가 발생한 것을 말해보기
  - 커뮤니티 질문
~~~
0. 밑줄이 나타났다면, 밑줄 앞이나 뒤에 error가 있을 가능성이 큼

1. SELECT list must not be empty at [10:1]
  - 해석: SELECT 목록은 [10:1]에서 비어 있으면 안됨
  * [10:1]: 10번째 줄 1번째 칸
  - 원인: SELECT와 FROM 사이가 비어있기 때문에 발생
  - 해결: SELECT와 FROM 사이에 col 명을 적어주기

2. Number of arguments does not match for aggregate function COUNT
  - 해석: 집계 함수 COUNT의 인자 수가 일치하지 않음
  - 원인: COUNT(name, Kor_name)처럼 COUNT() 안에는 여러가지 인자가 들어가면 안됨
  - 해결: COUNT() 안에 하나의 인자만 넣기

3. SELECT list expression references column type1 a which is neither grouped nor aggregated
  - 해석: SELECT 절에 있는 type1 컬럼이 그룹화되지도 않고 집계 함수로 묶이지도 않음
  - 원인: GROUP BY에 적절한 컬럼을 명시하지 않음
  - 해결: 하단에 GROUP BY type1을 작성

4. Expected end of input but got keyword SELECT
  - 해석: 입력이 끝날 것으로 예상되었지만 SELECT 키워드가 입력됨
  - 원인: 여러개의 쿼리를 실행시키면서 하나의 쿼리가 끝날 때 ;을 붙이지 않음
  - 해결: 쿼리가 끝나는 부분에 ;을 붙이기

5. Expected end of input but got keyword WHERE at [5:1]
  - 해석: 입력이 끝날 것으로 예상되었지만 [5:1]에서 키워드 WHERE를 얻음
  - 원인: WHERE 바로 직전 LIMIT이 입력됨
  - 해결: LIMIT을 쿼리의 맨 아래에 위치시키기

6. Expected ")" but got end of script at [8:11]
  - 해석: ")"가 예상되지만 [8:11]에서 스크립트가 끝남
  - 원인: 닫는 괄호 ")"가 작성되지 않음
  - 해결: ")"을 작성
~~~  



## 4-2. 데이터 타입과 데이터 변환(CAST, SAFE_CAST)

~~~
✅ 학습 목표 :
* 데이터 타입의 종류를 설명할 수 있다. 
* 데이터 타입을 변환하는 방법을 설명할 수 있다. 
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->



## 4-3. 문자열 함수(CONCAT, SPLIT, REPLACE, TRIM, UPPER)

~~~
✅ 학습 목표 :
* 문자열 함수들의 종류를 이해하고 어떠한 상황에서 사용하는지 설명할 수 있다. 
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->



## 4-4. 날짜 및 시간 데이터 이해하기(1) (타임존, UTC, Millisecond, TIMESTAMP/DATETIME)

~~~
✅ 학습 목표 :
* 날짜 및 시간 데이터 타입과 UTC의 개념을 설명할 수 있다. 
* DATE, DATETIME, TIMESTAMP 에 대해서 설명할 수 있다.
* 시간함수들의 종류와 시간의 차이를 추출하는 방법을 설명할 수 있다. 
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->



<br>

<br>

---

# 2️⃣ 확인문제 & 문제 인증

## 프로그래머스 문제 

> 조건에 맞는 회원 수 구하기 (SELECT, COUNT) 
>
> **먼저 문제를 풀고 난 이후에 확인 문제를 확인해주세요**
>
> 문제 링크 
>
> :  https://school.programmers.co.kr/learn/courses/30/lessons/131535#

<!-- 문제를 풀기 위하여 로그인이  필요합니다. -->

<!-- 정답을 맞추게 되면, 정답입니다. 라는 칸이 생성되는데 이 부분을 캡처해서 이 주석을 지우시고 첨부해주시면 됩니다. --> 



## 문제 1

> **🧚Q. 프로그래머스 문제를 풀던 서현이는 여러 번의 시행착오 끝에 결국 혼자 해결하기 어려워 오류 메시지를 공유하며 도움을 요청했습니다. 여러분들이 오류 메시지를 확인하고, 해당 SQL 쿼리에서 어떤 부분이 잘못되었는지 오류 메시지를 해석하고 찾아 설명해주세요.**

~~~sql
# 조건에 맞는 회원 수 구하기 (SELECT, COUNT) 
# 서현이의 SQL 첫 번째 풀이
SELECT COUNT(AGE, JOINED)
FROM USER_INFO
WHERE AGE BETWEEN 20 AND 29
  AND JOINED BETWEEN '2021-01-01' AND '2021-12-31';
  
오류 메시지 : Error: Number of arguments does not match for aggregate function COUNT
 
# 수정하고 난 이후 두 번째 풀이
SELECT AGE, COUNT(*)
FROM USER_INFO
WHERE AGE BETWEEN 20 AND 29
  AND JOINED BETWEEN '2021-01-01' AND '2021-12-31';
  
오류 메시지 : SELECT list expression references column AGE which is neither grouped nor aggregated
~~~



~~~
여기에 답을 작성해주세요!
~~~



### 🎉 수고하셨습니다.
