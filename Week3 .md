# SQL_BASIC 3주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_3rd_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**3주차 과제는 문제 풀이를 중심으로**, 강의에서 제시된 예제 문제 중 **7 문제 이상을 선택하여 직접 풀어본 뒤**, 강의 영상의 풀이와 비교해 **틀린 부분, 맞은 부분, 새롭게 배운 개념**을 구체적으로 정리해주세요. (적어도 3문제는 정리해야 합니다.) 완성된 과제는 Gihub에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**(수행 인증샷은 필수입니다.)** 

## SQL_BASIC_3rd

### 섹션 3. 데이터 탐색 - 조건, 추출, 요약

### 2-6. 연습문제 1~3번

### 2-6. 연습문제 7~9번

### 2-6. 연습문제 10~12번

### 2-6. 연습문제 13~17번

### 2-7. 정리 

### 2-8. 새로운 집계함수



## 섹션 4. 쿼리 잘 작성하기, 쿼리 작성 템플릿 및 오류를 잘 디버깅하기

### 3-1. INTRO

### 3-2. SQL 쿼리 작성하는 흐름

### 3-3. 쿼리 작성 템플릿과 생산성 도구 



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | 🍽️         |
| 5주차 | 섹션 **4-4** ~ **4-9** | 🍽️         |
| 6주차 | 섹션 **5-1** ~ **5-7** | 🍽️         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 개념정리

## 2-6. 연습문제

~~~
✅ 학습 목표 :
* 연습문제(7문제 이상) 푼 것들 정리하기
~~~

*생각해야 할 것* <br>
- 조건이 있는가?

- 어떤 테이블을 사용해야 하는가?

- 어떤 집계가 필요한가?

- 어떤 컬럼을 사용해야 하는가?


**1: 포켓몬 중에 type2가 없는 포켓몬의 수를 작성하는 쿼리.** <br>
~ 가 없다 -> IS NULL 반대는 IS NOT NULL <br>
SELECT 
    count(id) AS cnt
FROM basic.pokemon
WHERE 
    type2 IS NULL 

**3: type2 와 상관없이 type1 의 포켓몬 수를 알 수 있는 쿼리 작성.**<br>
'~ 와 상관없이'는 조건이 아님

SELECT 
    type1,
    COUNT(id) AS cnt
FROM basic.pokemon
GROUP BY 
    type1

**5: 동명 이인이 있는 이름은 무엇일까요?**<br>
조건확인: 같은 이름이 두 개 이상(동명이인) => COUNT(name)이 두 개 이상

SELECT 
    name,
    COUNT(name) AS trainer_cnt
FROM basic.trainer
GROUP BY 
    name
HAVING
    trainer_cnt >= 2

**11: type2 가 있는 포켓몬 중에 제일 많은 type1 은 무엇인가요?**<br>
가장 많은 것 1개만 출력 -> LIMIT 1 로 설정하면됨 

SELECT
    type1, 
    COUNT(id) AS pokemon_cnt
FROM basic.pokemon
WHERE 
    type2 IS NOT NULL
GROUP BY 
    type1
ORDER BY
    pokemon_cnt DESC
LIMIT 1

**13: 포켓몬의 이름에 "파" 가 들어가는 포켓몬은 어떤 포켓몬이 있을까요?** <br>
컬럼 LIKE -% 는 -로 끝나는 단어. %-는 -로 시작하는 단어. %-%는 -가 들어간 단어.

name LIKE 는 WHERE 조건에 

SELECT 
    kor_name
FROM basic.pokemon
WHERE 
    kor_name LIKE "%파%"

**16: 포켓몬을 많이 풀어준 트레이너는 누구일까요?**<br>
status = "" 따옴표 붙이기 

SELECT 
    status,
    COUNT(id) as cnt
FROM basic.trainer_pokemon
GROUP BY 
    status 
HAVING status = "Released"
ORDER BY 
    status DESC
LIMIT 1

**17: 트레이너 별로 풀어준 포켓몬의 비율이 20%가 넘는 포켓몬 트레이너는 누구일까요?**<br>
COUNTIF 사용하기 

SELECT 
    trainer_id,
    COUNTIF(status="Released"),
    COUNT(pokemon_id) AS pokemon_cnt,
    COUNTIF(status="Released")/COUNT(pokemon_id) AS released_ratio
FROM basic.trainer_pokemon
GROUP BY 
    trainer_id
HAVING
    released_ration >= 0.2 


## 2-8. 새로운 집계함수

~~~
✅ 학습 목표 :
* SQL 쿼리 구조를 이해할 수 있다. 
* SELECT, FROM, WHERE을 활용하는 방법을 설명할 수 있다. 
~~~

GROUP BY 함수를 사용할 때 컬럼명을 명시해야 하는데, GROUP BY ALL 을 사용하면 추론하여 사용이 가능해진다. 



## 3-2. 쿼리를 작성하는 흐름

~~~
✅ 학습 목표 :
* 쿼리를 작성하는 흐름을 설명할 수 있다.
~~~

지표 고민: 어떤 문제를 해결하기 위해 데이터가 필요한가?

지표 구체화: 추상적이지 않고 구체적인 지표 명시(분자,분모 표시)

지표 탐색: 유사한 문제를 해결한 케이스가 있나 확인 -> 존재한다면 해당 쿼리 리뷰

쿼리 작성: 데이터가 있는 테이블 찾기 

데이터 정합성 확인: 예상한 결과와 동일한지 확인

쿼리 가독성: 나중을 위해 깔끔하게 쿼리 작성

쿼리 저장: 쿼리는 재사용되므로 문서로 저장 



## 3-3. 쿼리 작성 템플릿과 생산성 도구

~~~
✅ 학습 목표 :
* 생산성 도구를 만들 수 있다.
~~~

**쿼리 작성 템플릿**

- 쿼리를 작성하는 목표, 확인할 지표

- 데이터의 기간

- 사용할 테이블 

- Join KEY

- 데이터 특징

생산성 도구: Espanso

핵심 로직 -> 특정 단어가 감지되면 정의된 것으로 바꿈




<br>
<br>

---

# 2️⃣ 학습 인증란
![학습인증란](https://github.com/HEEJU21/asset/blob/main/SQL_week3.png)




<br><br>



---

# 3️⃣ 확인문제

## 문제 1

> **🧚Q. 포켓몬 게임에 재미를 느낀 동혁은 포켓몬 도감에서 강력한 포켓몬 타입을 미리 선점하기 위해, 먼저 어떤 포켓몬들이 있는지 포켓몬 수를 기준으로 내림차순 정렬하여  확인하고자 했습니다.**
>
> 그래서 다음과 같은 필요한 정보를 미리 정리해보았습니다. 

~~~
조건 : type2는 상관없이
보고 싶은 컬럼 : type1
집계 내용 : 각 type1 별 포켓몬 수
정렬 기준 : 포켓몬 수를 기준으로 내림차순 정렬
~~~

> **이 목표를 바탕으로 동혁이 아래와 같은 쿼리를 잘 작성했지만, 일부 SQL 문법 요소를 빼먹었습니다. 비어 있는 부분인 ㄱ,ㄴ,ㄷ 에 들어갈 알맞은 SQL 구문을 채워보세요:**

~~~sql
SELECT type1, (ㄱ)
FROM pokemon
(ㄴ) type1
ORDER BY (ㄱ) (ㄷ);
~~~



~~~
ㄱ : COUNT(id) AS pokemon_cnt
ㄴ : GROUP BY 
ㄷ : pokemon_cnt DESC
~~~



### 🎉 수고하셨습니다.
