---
"\blink":
  - "[[List]]"
  - "[[Map]]"
  - "[[Set]]"
---
### Iterator
> 컬렉션에 저장된 요소들을 읽어오는 방법을 표준화 한 인터페이스
###### 데이터의 확인
|ReturnType|Method(args)|Explain|
|:---:|---|---|
|boolean|hasNext()|다음 요소가 있는지 확인합니다.|
|E|next()|다음 요소를 반환하고 커서를 다음 위치로 이동합니다.|
###### 데이터의 삭제
|ReturnType|Method(args)|Explain|
|:---:|---|---|
|void|remove()|현재 가리키는 요소를 삭제합니다.|

---
### ListIterator
> Iterator에 양방향 조회기능 추가(List를 구현한 경우만 사용 가능)
###### 데이터 추가
|ReturnType|Method(args)|Explain|
|:---:|---|---|
|void|add(E e)|현재 위치에 요소를 추가합니다.|
###### 데이터 확인
|ReturnType|Method(args)|Explain|
|:---:|---|---|
|boolean|hasNext()|다음 요소가 있는지 확인합니다.|
|boolean|hasPrevious()|이전 요소가 있는지 확인합니다.|
|E|next()|다음 요소를 반환하고 커서를 다음 위치로 이동합니다.|
|E|previous()|이전 요소를 반환하고 커서를 이전 위치로 이동합니다.|
|int|nextIndex()|다음 요소의 인덱스를 확인합니다.|
|int|previousIndex()|이전 요소의 인덱스를 확인합니다.|
###### 데이터 삭제
|ReturnType|Method(args)|Explain|
|:---:|---|---|
|void|remove()|현재 가리키는 요소를 삭제합니다.|
###### 데이터의 변경
|ReturnType|Method(args)|Explain|
|:---:|---|---|
|void|set(E e)|현재 가리키는 요소를 새로운 요소로 변경합니다.|

---
### Enumeration
> Iterator의 구버전, 이전 버전으로 작성된 소스와 호환을 위해 남겨두고 있을 뿐이므로 가능하면 위의 인터페이스를 사용할 것
###### 데이터 확인
|ReturnType|Method(args)|Explain|
|:---:|---|---|
|boolean|hasMoreElements()|다음 요소가 있는지 확인합니다.|
|E|nextElement()|다음 요소를 반환하고 커서를 다음 위치로 이동합니다.|