# Index
![][https://www.youtube.com/watch?v=iNvYsGKelYs]

---
##### 인덱스의 필요성
 - 데이터베이스에 저장된 자료를 더욱 빠르게 조회하기 위해 인덱스를 생성하여 사용함
 - 일반적으로 테이블이 갖고 있는 데이터 중에서 10~15%이하를 처리할 경우 사용
 - 테이블 마다 저장해 둔 데이터 수가 다르기 때문에 그 기준은 절대적이지 않음
---
##### B\*Tree 구조
- B\*Tree 인덱스의 데이터 저장방식의 핵심은 정렬(SORT)임
- ORDER BY에 의한 정렬 작업을 대체할 수 있는 수단이 될 수 있음
- MIN / MAX 함수의 최적화가 가능함
![[Pasted image 20231005010517.png]]

---
##### 인덱스 선정 절차
1. 프로그램 개발에 이용된 모든 테이블에 대하여 Access Path조사
2. 인덱스 칼럼(Column) 선정 및 분포도 조사
3. Critical Access Path 결정 및 우선 순위 선정
4. 인덱스 칼럼의 조합 및 순서 결정
5. 시험 생성 및 테스트
6. 결정된 인덱스를 기준으로 프로그램 반영
7. 실제 적용
---
##### 인덱스 생성 및 변경 시 고려 사항
1. 인덱스가 추가될 경우 기존 프로그램 동작의 영향성 검토
2. 필요할 때마다 인덱스를 생성할 경우, DML 작업속도 저하
3. 개별 Column의 분포도가 좋지 않더라도 다른 Column과 결합하여 자주 사용된다면 결합 인덱스 생성을 검토함
---
##### 인덱스 스캔의 원리
- 인덱스 스캔 시에는 한 번의 I/O가 발생할 때마다 한 개의 Block씩을 처리함
---
##### 인덱스의 필요성
- 고유(Unique) 인덱스의 Equal(=)과 범위(Range) 검색
- 중복(Non-Unique) 인덱스의 범위(Range) 검색
- OR & IN 조건 - 결과의 결합
- NOT BETWEEN 검색
---
##### 예시
> 본문
```SQL
SELECT MIN(COURSE_SQ_NO) AS MIN_SQ, MAX(COURSE_SQ_NO) AS MAX_SQ
FROM EC_COURSE_SQ
WHERE COURSE_CODE = 1960
AND YEAR = '2002';
```
	Index :
		EC_COURSE_SQ_PK : COURSE_CODE + YEAR + COURSE_SQ_NO
		EC_COURSE_SQ_IDX_01 : YEAR(Non Unique)

> 수정문
```SQL
SELECT SUM(MIN_SQ) AS MIN_SQ, SUM(MAX_SQ) AS MAX_SQ
FROM (
	SELECT /*+INDEX_ASC(A EC_COURSE_SQ_PK)*/ 
		A.COURSE_SQ_NO AS MIN_SQ, 
		0 AS MAX_SQ
	FROM EC_COURSE_SQ A
	WHERE 
		A.COURSE_CODE = 1960
		AND A.YEAR = '2002'
		AND ROWNUM = 1 /*한건만 찾게 함*/
	UNION ALL
	SELECT /*+INDEX_DESC(A EC_COURSE_SQ_PK)*/
		0 AS MIN_SQ,
		A.COURSE_SQ_NO AS MAX_SQ
	FROM EC_COURSE_SQ A
	WHERE
		A.COURSE_CODE = 1960
		AND A.YEAR = '2002'
		AND ROWNUM = 1 /*한건만 찾게 함*/
);
```

---
---
# 결합인덱스
##### 인덱스 머지(Index Merge) VS 결합인덱스
- 인덱스 머지 
	- 개별 Column에 Index가 생성
	- 모두 where절에 Equal(=) 조건으로 사용
	- 되어 각각의 인덱스 조합으로 자료에 접근하는 것을 의미
- 일반적으로 결합인덱스를 사용할 경우, 좋은 Access 경로를 제공함
---
##### 결합 인덱스의 구성
- 결합 인덱스 Column 순서 결정
	- where절 조건에 많이 사용되는 칼럼
	- Equal(=)로 사용되는 Column
	- 분포도가 좋은 Column
	- 자주 이용되는 Sort의 순서로 결정
---
##### 결합 인덱스 사용 방법
- 결합인덱스의 첫 번째 Column이 where절에서 제외되어 있는 경우
	- 결합인덱스를 사용할 수 없음
- 인덱스 스킵 스캐닝(index skip scanning) 
	- 결합인덱스의 첫 번째 Column이 where절에서 제외되고
	- 두 번째 Column부터 where절에 조건으로 기술되어 있는 경우에도
	- 그 인덱스가 사용되는 경우임
---
##### 결합 인덱스 Column에 대한 '='의 의미
- 범위 제한 조건
	- 결합 인덱스의 선행하는 Column(첫 번째 Column 포함) 순서
	- WHERE절에 '='으로 연속된 경우, 해당하는 조건을 범위제한 조건이라고 함
- 체크 조건
	- WHERE 절 조건에서 선행 Column이 '=' 조건에 없다면 후행 조건은 체크조건이 됨
- 인덱스 매칭률![[Pasted image 20231005042349.png]]
---
##### 예시
>제시문
```SQL
SELECT /*+RULE*/
	COURSE_CODE, COUNT(COURSE_DATE) AS CNT
FROM EC_PROGRESS
WHERE
	COURSE_CODE < 1000
	AND YEAR = '2000'
GROUP BY COURSE_CODE;
```
	index :
		EC_PROGRESS_PK : 
			COURSE_CODE 
			+ YEAR 
			+ COURSE_SQ_NO 
			+ MEMBER_TYPE 
			+ MEMBER_ID 
			+ CHAR_NO 
			+ PARAG_NO

>수정문
```SQL
SELECT /*+RULE ORDERED USE_NL(B A)*/
	A.COURSE_CODE, COUNT(A.COURSE_DATE) AS CNT
FROM EC_COURSE B, EC_PROGRESS A 
WHERE
	A.COURSE_CODE = B.COURSE_CODE
	AND B.COURSE_CODE < 1000
	AND A.YEAR = '2000'
GROUP BY COURSE_CODE;
```
---
---
# 인덱스 활용이 불가능한 경우
##### 인덱스를 사용하지 말아야 하는 경우
- 6블록 이상의 데이터를 가진 테이블에서, 쿼리로 처리할 데이터가 전체 데이터 중 15% 이상을 초과할 경우엔 인덱스를 사용하지 않는 것이 매우 좋은 성능을 냄
---
##### 인덱스를 사용이 불가능한 경우
- NOT 연산자와 비교
- IS NULL, IS NOT NULL
- 옵티마이저에 의한 취사선택
- 인덱스 컬럼의 변형(External / Internal Suppressing)
---
##### 옵티마이저에 의한 선택 절자
> 1단계 : 주어진 조건에 대한 각 인덱스 별로 매칭률을 계산해서 매칭률이 높은 것을 우선 선택
![[Pasted image 20231005042349.png]]

> 2단계 : 인덱스별 매칭률이 같을 경우에는 인덱스를 구성하는 컬럼의 개수가 많은 것 우선 선택

> 3단계 : (인덱스별 매칭률 = 인덱스 구성 컬럼의 개수)인 경우에는 최근 생성 우선 선택


##### 예시
>제시문
```SQL
SELECT /*+INDEX(A EC_APPLY_PK) NO-USE_HASH_AGGREGATION*/
	COURSE_CODE,
	NVL(SUM(DECODE(YEAR,'1999',DEPOSIT_AMOUNT)),0) Y1999,
	NVL(SUM(DECODE(YEAR,'2000',DEPOSIT_AMOUNT)),0) Y2000,
	NVL(SUM(DECODE(YEAR,'2001',DEPOSIT_AMOUNT)),0) Y2001,
	NVL(SUM(DECODE(YEAR,'2002',DEPOSIT_AMOUNT)),0) Y2002,
FROM EC_APPLY A
WHERE
	COURSE_CODE < 1000
	AND YEAR BETWEEN '1999' AND '2002'
GROUP BY COURSE_CODE;
```
	index :
		EC_PROGRESS_PK : 
			COURSE_CODE 
			+ YEAR 
			+ COURSE_SQ_NO 
			+ MEMBER_TYPE 
			+ MEMBER_ID 

>수정문
```SQL
SELECT /*+INDEX(A EC_APPLY_PK) NO-USE_HASH_AGGREGATION*/
	COURSE_CODE,
	NVL(SUM(DECODE(YEAR,'1999',DEPOSIT_AMOUNT)),0) Y1999,
	NVL(SUM(DECODE(YEAR,'2000',DEPOSIT_AMOUNT)),0) Y2000,
	NVL(SUM(DECODE(YEAR,'2001',DEPOSIT_AMOUNT)),0) Y2001,
	NVL(SUM(DECODE(YEAR,'2002',DEPOSIT_AMOUNT)),0) Y2002,
FROM (
	SELECT /*FULL(EC_APPLY)*/
		COURSE_CODE,
		YEAR,
		SUM(DEPOSIT_AMOUNT) DEPOSIT_AMOUNT
	FROM EC_APPLY
	WHERE
		COURSE_CODE<1000
		AND YEAR BETWEEN '1999' AND '2002'
	GROUP BY COURSE_CODE, YEAR
)
GROUP BY COURSE_CODE;
```