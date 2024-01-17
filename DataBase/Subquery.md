# SubQuery
---
##### SUBQUERY
- 하나의 질의문 내부에 하나 이상의 다른 질의문이 포함되어 그 결과를 이용할 때 내부에 포함된 쿼리문을 말함
- GROUB BY 절을 제외하고는 어디든 위치할 수 있다.
---
##### NESTED SUBQUERY
- 서브쿼리가 WHERE절에서 사용된 경우 Nested SubQuery라고 함
- Nested SubQuery가 Main Query보다 먼저 실행 될 때 속도를 낼 수 있는 유형임
- 단, 서브쿼리에서 조회하는 Main Query값에 인덱스가 없으면 서브쿼리는 먼저 실행되지 않음

---
##### CORRELATED SUBQUERY
- 서브쿼리가 WHERE절에서 사용되고 메인 쿼리에서 데이터를 하나씩 읽을 때마다 서브쿼리가 실행되어 데이터를 리턴하는 서브쿼리를 Correlated SubQuery라고 함
- 메인쿼리에서 데이터를 읽고 있는 Row수 만큼 서브쿼리가 실행됨
---
##### SCALAR SUBQUERY
- 단 하나의 데이터와 단 하나의 Column에 대한 정보를 리턴함
- Scalar SubQuery 사용 위치
	- SELECT List 항목, 
	- 함수의 인자, 
	- WHERE절의 조건, 
	- Order by 절, 
	- Case 조건절 + 결과절
---
##### ROLLUP() & CUBE()
- RollUp()
	- 데이터의 총계를 나타낼 때 사용하는 함수
- Cube()
	- 데이터의 소계를 나타낼 때 사용하는 함수
---
##### GROUPING SETS()
- 여러 개의 GROUB BY 쿼리를 통해 실행한 것과 같은 결과를 나타냄
- Grouping Sets()로 Rollup(), Cube(), Rollup() & Cube() 구현이 가능함
---
##### ANALYTIC FUNCTIONS
- 행과 행 간의 관계를 정의하거나 비교, 연산하기 위해 사용함
- 문법
```SQL
SELECT Analytic_Function(arguments) OVER
	([Partition By 칼럼][Order by 절][Windowing 절])
FROM 테이블 명...
WHERE ...
```

|                |                                                    |
| -------------- | -------------------------------------------------- |
| Arguments      | 함수에 따라 0~3개의 인자가 지정됨                  |
| Parition By 절 | 전체 집합을 기준에 의해 소그룹으로 나눔            |
| Order By 절    | 어떤 항목에 대한 정렬 기준을 기술함                |
| Windowing 절   | 함수에 의해서 제어하고자 하는 데이터 범위를 정의함 |

- Analytic Functions 종류
	- 그룹 내 데이터 순위 : ROW_NUMBER, RANK, DENSE_RANK
	- 그룹 내 비울 : RATIO_TO_REPORT
---
##### 예시
> 본문
```SQL
SELECT A.COURSE_CODE, A.COURSE_NAME
FROM EC_COURSE A
WHERE 
	A.COURSE_CODE IN(
		SELECT COURSE_CODE
		FROM EC_APPLY
		WHERE YEAR = '2000'
		GROUP BY COURSE_CODE
		HAVING COUNT(COURSE_CODE) < 500
	) OR NOT EXISTS (
		SELECT 'X'
		FROM EC_APPLY C
		WHERE 
			C.COURSE_CODE = A.COURSE_CODE
			AND C.YEAR = '2000'
	);
```
	Index :
		EC_COURSE_PK : 
			COURSE_CODE
		EC_APPLY_PK : 
			COURSE_CODE
			+ YEAR
			+ COURSE_SQ_NO
			+ MEMBER_TYPE
			+ MEMBER_ID

> 수정문
```SQL
SELECT A.COURSE_CODE, A.COURSE_NAME
FROM EC_COURSE A
WHERE 
	A.COURSE_CODE IN(
		SELECT COURSE_CODE
		FROM EC_APPLY
		WHERE YEAR = '2000'
		GROUP BY COURSE_CODE
		HAVING COUNT(COURSE_CODE) < 500
	) 
UNION ALL	
SELECT A.COURSE_CODE, A.COURSE_NAME
FROM EC_APPLY A
WHERE 
	NOT EXISTS(
		SELECT 'X'
		FROM EC_APPLY C
		WHERE 
			C.COURSE_CODE = A.COURSE_CODE
			AND C.YEAR = '2000'
	);
```
