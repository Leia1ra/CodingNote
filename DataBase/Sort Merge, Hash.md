# Sort/merge Join

##### SORT/MERGE JOIN
- Sort/Merge Join을 사용해야 하는 경우
	- 연결 고리에 인덱스가 전혀 없는 경우
	- 대용량 자료를 조인해야 함으로써 [[Index|인덱스]] 사용에 따른 랜덤 액세스의 오버헤드가 많은 경우
- 튜닝 포인트
	- 각 테이블로부터 데이터를 빨리 읽어 들이도록 함
	- 각 테이블로부터 읽어 들인 데이터를 조인하기 전에 정렬(Sort)을 하는데, 
	- 이러한 정렬 작업을 빨리 끝낼 수 있도록 함
---
##### SORT/MERGE JOIN의 수행 절차
1. 각 테이블에 대해 동시에 독립적으로 데이터를 먼저 읽어들임
2. 읽혀진 각 테이블의 데이터를 조인을 위한 연결고리에 대하여 정렬을 수행함
3. 정렬이 모두 끝난 후에 조인 작업이 수행됨
---
##### SORT/MERGE JOIN이 불리한 경우
- 각 테이블에서 읽은 데이터를 조인하기 전에 정렬(Sort)하는데, 이때 정렬할 데이터가 지나치게 큰 경우
- 각 테이블로부터 읽어 들인 데이터의 크기가 매우 큰 경우
---
##### SORT/MERGE JOIN의 장단점
- 장점
	- 연결고리에 인덱스가 생성되어 있지 않은 경우에 빠른 조회를 수행할 수 있다
- 단점
	- 각 테이블로부터 읽어 들인 데이터의 크기가 매우 큰 경우 성능상 불리하다
	- 데이터 정렬 소요시간, 데이터 스캔 소요시간
---
---
# Hash Join
##### HASH JOIN
- 각 테이블에 대한 처리를 독립적으로 하지만, Hash Join은 Driving Table이 있음
- 읽은 각 테이블의 데이터를 조인하기 위해 해싱을 이용해서 해시 값을 만들어 조인을 수행함
- 튜닝포인트
	- Driving Table의 결정
	- 각 테이블로부터 독립적으로 데이터를 읽어 들일 때 빨리 처리하도록 함
	- 해시 조인을 위한 메모리를 최적화 함
---
##### HASH JOIN의 수행 절차
1. Driving Table 결정
2. Driving Table의 연결조건 Column 해싱 및 해시 값 생성
3. 읽어 들인 데이터와 해싱에서 만들어지는 해시 값을 메모리에 저장
4. Hash Join이 적용될 테이블의 연결조건 Column 해싱 및 해시 값 생성
5. 읽어 들인 데이터와 해싱에서 만들어진 해시 값을 메모리에 저장
6. 각 테이블에 조인할 데이터가 있는지, 조인하고자 만들었던 해시 값 간에 충돌 확인
	- > 충돌이 발생할 경우, 2차 해싱 수행
7. 각 테이블의 해시값을 "="로 조인을 수행함
---
##### HASH JOIN의 장단점
- 장점
	- 하드웨어 자원이 넉넉한 상황에서는 다른 조인에 비해 보다 효율적인 수행이 가능함
- 단점
	- 하드웨어 자원이 부족한 상황에서는 다른 조인 방법보다 비효율적임
---
##### 예시
![[Pasted image 20231006044626.png]]
> 본문
```SQL
SELECT /*+USE_MERGE(A B)*/
B.COURSE_CODE, B.YEAR, B.COURSE_SQ_NO, B.MEMBER_TYPE,
B.MEMBER_ID, B.CHAP_NO, B.PARAG_NO,
A.OPEN_YN, A.END_YN
FROM EC_COURSE_SQ A, EC_PROGRESS B
WHERE
A.COURSE_CODE = B.COURSE_CODE
AND A.YEAR = B.YEAR
AND A.COURSE_SQ_NO = B.COURSE_SQ_NO;
```
Index :
EC_COURSE_SQ_PK : 
	COURSE_CODE
	+ YEAR
	+ COURSE_SQ_NO
EC_PROGRESS_SQ_PK : 
	COURSE_CODE
	+ YEAR
	+ COURSE_SQ_NO
	+ MEMBER_TYPE
	+ MEMBER_ID
	+ CHAP_NO
	+ PARAG_NO

> 수정문
```SQL
SELECT /*+ORDERED USE_HASH(A B)*/
B.COURSE_CODE, B.YEAR, B.COURSE_SQ_NO, B.MEMBER_TYPE,
B.MEMBER_ID, B.CHAP_NO, B.PARAG_NO,
A.OPEN_YN, A.END_YN
FROM EC_COURSE_SQ A, EC_PROGRESS B
WHERE
A.COURSE_CODE = B.COURSE_CODE
AND A.YEAR = B.YEAR
AND A.COURSE_SQ_NO = B.COURSE_SQ_NO;
```