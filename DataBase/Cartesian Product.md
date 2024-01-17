# 조인 조건이 없는 조인
---
##### CARTESIAN PRODUCT
- 곱집합
- 특정 테이블의 데이터를 필요한 만큼 복사(copy)하기 위한 방법
- Cartesian Product가 발생되는 경우
	- WHERE절이 없는 조인을 수행할 경우
	- 조인을 위한 조건 없이 조인을 수행할 경우
---
##### CARTESIAN PRODUCT시 자주 사용하는 테이블
- Cartesian Product만을 위한 전용 테이블을 생성해서 사용
- 이미 생성되어 있는 정규 테이블을 사용
- DUAL과 같은 dummy 테이블 사용
---
##### CARTESIAN PRODUCT 적용 예제
- UNION으로 연결된 각각의 SQL이 읽고 있는 데이터가 전부 같을 경우, 데이터 복제와 같은 개념 활용을 위해 사용
- 데이터 구조 변환을 통해 사용자가 요청한 구조대로 데이터를 조회할 때 사용
- SQL문으로 일년 치 날짜를 만들 때 사용
---
##### 예시
> 본문 : 문제 : 같은 데이터를 3번이나 읽고 있다는 것이 문제
```SQL
SELECT /*+NO_USE_HASH_AGGREGATION*/ 
	TO_CHAR(COURSE_CODE) AS "과정/연도",
	NVL(SUM(DEPOSIT_AMOUNT),0) AS DEPOSIT_AMOUNT
FROM EC_APPLY
WHERE COURSE_CODE < 100
GROUP BY COURSE_CODE

UNION ALL
SELECT /*+NO_USE_HASH_AGGREGATION*/
	YEAR AS "과정/연도",
	NVL(SUM(DEPOSIT_AMMOUNT),0) AS DEPOSIT_AMOUNT
FROM EC_APPLY
WHERE COURSE_CODE < 100
GROUP BY YEAR

UNION ALL
SELECT /*+NO_USE_HASH_AGGREGATION*/
	NULL AS "과정/연도",
	NVL(SUM(DEPOSIT_AMMOUNT),0) AS DEPOSIT_AMOUNT
FROM EC_APPLY
WHERE COURSE_CODE < 100;
```
	Index :
		EC_APPLY_PK : 
			COURSE_CODE 
			+ YEAR 
			+ COURSE_SQ_NO 
			+ MEMBER_TYPE 
			+ MEMBER_ID

> 수정문
```SQL
SELECT /*+NO_USE_HASH_AGGREGATION*/ 
	CASE 
		WHEN B.SW = 1 THEN A.CODE
		WHEN B.SW = 2 THEN A.YEAR END AS "과졍/연도",
	SUM(A.T_AMT) AS DEPOSIT_AMOUNT
FROM (
	SELECT
		TO_CHAR(COURSE_CODE) AS CODE,
		YEAR,
		NVL(SUM(DEPOSIT_AMOUNT),0) AS T_AMT
	FROM EC_APPLY
	WHERE COURSE_CODE < 100
	GROUP BY COURSE_CODE, YEAR
) AS A, (
	SELECT ROWNUM AS SW
	FROM COPY_T
	WHERE ROWNUM <= 3
) 
GROUP BY 
	B.SW,
	CASE 
		WHEN B.SW = 1 THEN A.CODE 
		WHEN B.SW = 2 THEN A.YEAR END
ORDER BY B.SW;
```
