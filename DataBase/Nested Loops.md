# Nested Loops
---
##### Nested Loops 조인의 튜닝 포인트
- 테이블 간 조인 횟수를 최소화를 위한 조인 순서의 최적화
- Driven테이블의 경우 연결고리 인덱스가 반드시 사용되어야 함
---
##### Nested Loops 조인의 장단점
- 장점
	- 인덱스를 통한 랜덤 액세스(Random Access)기반에서 좋은 성능을 보임
- 단점
	- 인덱스가 없는 상태에선 속도가 저하됨
	- 대용량 데이터를 처리할 경우 성능이 저하됨
---
##### 조인 순서 제어 방법
- 힌트 사용
```SQL
-- 1. 조인 순서 제어
/*+ORDERED*/
-- 2. From절에 기술한 테이블 순서대로 제어
/*+LEADING(table명)*/
```
- 뷰(View) 활용
- 서프레싱 활용
- From 절 테이블 순서 변경(단, [[Optimizer|CBO]]에서는 의미 없음)
---
##### 연결고리 Column에 대한 인덱스의 중요성
- 양쪽 모두 인덱스가 있는 경우
	- 두 테이블 중 조회되는 결과가 적은 테이블을 선택하여 드라이빙 테이블로 선택함
- 한쪽만 인덱스가 있는 경우
	- 인덱스가 없는 테이블을 드라이빙 테이블로 사용함
- 양쪽 모두 인덱스가 없는 경우
	- Nested Loops 조인 방식으로 조인이 이루어지지 않음
---
##### 예시
> 본문
```SQL
SELECT 
	A.COURSE_CODE, A.COURSE_NAME,
	B.EXAM_NO, B.EXAM_KIND, B.EXAM_TITLE,
	C.YEAR, C.COURSE_SQ_NO, C.APPLY_TYPE, C.EVAL_RATE
FROM EC_COURSE A, EX_EXAM B, EC_EXAM_TERM C
WHERE
	A.COURSE_CODE = C.COURSE_CODE -- 조인의 서순이 잘못되었음(가장 적은순부터 최적화)
	AND B.COURSE_CODE = C.COURSE_CODE
	AND B.EXAM_NO = C.EXAM_NO
	AND C.COURSE_CODE = 14;
```
	Index :
		EC_COURSE_PK : COURSE_CODE
		EC_EXAM_PK : COURSE_CODE + EXAM_NO
		EC_EXAM_TERM_PK : COURSE_CODE + YEAR + COURSE_SQ_NO + EXAM_NO

> 수정문
```SQL
SELECT 
	A.COURSE_CODE, A.COURSE_NAME,
	B.EXAM_NO, B.EXAM_KIND, B.EXAM_TITLE,
	C.YEAR, C.COURSE_SQ_NO, C.APPLY_TYPE, C.EVAL_RATE
FROM EC_COURSE A, EX_EXAM B, EC_EXAM_TERM C
WHERE
	A.COURSE_CODE = B.COURSE_CODE
	AND B.COURSE_CODE = C.COURSE_CODE
	AND B.EXAM_NO = C.EXAM_NO
	AND A.COURSE_CODE = 14;
```

