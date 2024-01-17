# 옵티마이저
##### 옵티마이저의 개념
사용자가 실행한 SQL을 해석하고 데이터를 추출하기 위한 실행계획을 수립하는 프로세스

---
##### 옵티마이저의 종류
###### RBO(Rule Based Optimizer)
- 규칙기준 옵티마이저
- 인덱스 구조나 사용하는 연산자에 따라 부여되는 순위가 정해져 있음(Rank Rule)

###### CBO(Cost Based Optimizer)
- 대상 Rows를 처리하는데 필요한 자원 사용을 최소화
- 데이터를 빨리 처리하는데 목적이 있음

---
##### 옵티마이저의 레벨별 설정
###### 데이터베이스 전체에 지정
```SQL
--[initSID.ora를 이용해서 지정]
OPTIMIZER_MODE = [RULE/CHOOSE/FIRST_ROWS/ALL_ROWS]
```

###### 세션별로 지정
```SQL
ALTER SESSION SET OPTIMIZER_MODE = [RULE/CHOOSE/FIRST_ROWS/ALL_ROWS]
```

###### 각 SQL별로 지정
```SQL
SELECT /*+first_rows*/
		ename
FROM emp;
```
---
##### RBO와 CBO의 실행계획 비교
- 동일 SQL에 대해서 각 옵티마이저가 수립한 실행계획은 서로 다를 수 있음
- 이는 SQL의 퍼포먼스가 옵티마이저에 따라 다르다는 의미임
- 옵티마이저의 종류에 따라 달라지는 DB성능의 차이점을 이해하는 것이 중요함