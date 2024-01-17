---
"\blink":
  - "[[Collection]]"
---
![[11장_Set.png]]
>[[Set]] : 순서를 유지하지 않는 데이터의 집합, 데이터의 중복을 허용하지 않는다. 
- 예) 양의 정수 집합, 소수의 집합
1. HashSet
2. TreeSet
##### 주요 Method
###### 1. 데이터 추가
| Return Type | Method(args)| explain|
| :---------: | ----------- | ----- |
|boolean|add(E e)|요소를 집합에 추가|
|boolean|addAll(Collection\<? extends E\> c)|다른 컬렉션의 모든 요소를 집합에 추가|
###### 2. 데이터 확인
| Return Type | Method(args)| explain|
| :---------: | ----------- | ----- |
|boolean|contains(Object o)|지정된 요소가 집합에 포함되어 있는지 확인|
|boolean|containsAll(Collection\<?\> c)|다른 컬렉션의 모든 요소가 집합에 포함되어 있는지 확인|
|boolean|isEmpty()|집합이 비어 있는지 확인|
|int|size()|집합의 크기를 확인|
|Object[]|toArray()|집합의 요소를 배열로 반환|
|\<T\> T[]|toArray(T[] a)|집합의 요소를 지정된 배열로 반환|
|boolean|equals(Object o)|매개변수로 넘어온 객체와 같은 객체인지 확인한다.|
###### 3. 데이터 삭제
| Return Type | Method(args)| explain|
| :---------: | ----------- | ----- |
|void|clear()|집합의 모든 요소를 제거|
|boolean|remove(Object o)|지정된 요소를 집합에서 제거|
|boolean|removeAll(Collection\<?> c)|다른 컬렉션의 모든 요소를 집합에서 제거|
|boolean|retainAll(Collection\<?> c)|다른 컬렉션의 요소만을 집합에 유지하고 나머지 요소를 제거|
###### 4. 데이터 변경
| Return Type | Method(args)| explain|
| :---------: | ----------- | ----- |
|int|hashCode()|해시 코드 값을 리턴한다|
|Iterator|iterator()|데이터를 한 건씩 처리하기 위한 iterator 객체를 리턴한다.|
|boolean|retainAll(Collection c)|지정된 Collection에 포함된 객체만을 남기고 <br>다른 객체들은 Collection에서 삭제한다.<br> 이 작업으로 인해 Collection에 변화가 있으면 true <br>그렇지 않으면 false를 반환한다.|
|int|size()|요소의 개수를 리턴한다.|
|Object\[\]|toArray()|컬렉션에 있는 데이터들을 배열로 복사한다.|
|\<T> T\[\]|toArray(T\[\])|컬렉션에 있는 데이터들을 지정한 타입의 배열로 복사한다.|

---
### HashSet
```java
/*생성시*/
HashSet hs1 = new HashSet();
// HashSet을 생성

HashSet hs1 = new HashSet(Collection c);
// 주어진 Collection이 저장된 HashSet을 생성

HashSet hs1 = new HashSet(int intialCapacity);
// 지정된 초기용량을 갖는 HashSet을 생성

HashSet hs1 = new HashSet(int intialCapacity, float loadFactor);
// 초기용량과 loadFactor를 지정하는 생성자
```
---












##### 주요 Method
###### 1. 데이터 추가
| Return Type | Method(args)| explain|
| :---------: | ----------- | ----- |


###### 2. 데이터 확인
| Return Type | Method(args)| explain|
| :---------: | ----------- | ----- |


###### 3. 데이터 삭제
| Return Type | Method(args)| explain|
| :---------: | ----------- | ----- |


###### 4. 데이터 변경
| Return Type | Method(args)| explain|
| :---------: | ----------- | ----- |

















