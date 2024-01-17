##### Collections Framework
- 데이터 군(群)을 저장하는 클래스들을 표준화한 설계

###### 1. 데이터 추가
| Return Type | Method(args)| explain|
| :---------: | ----------- | ----- |
|boolean|add(E e)|요소를 추가한다.|
|boolean|addAll(Collection)|매개변수로 넘어온 컬렉션의 모든 요소를 추가한다.|

###### 2. 데이터 확인
| Return Type | Method(args)| explain|
| :---------: | ----------- | ----- |
|boolean|contains(Object o)|매개변수로 넘어온 객체가 해당 컬렉션에 있는지 확인.|
|boolean|containsAll(Collection c)|매개변수로 넘어온 객체들이 해당 컬렉션에 있는지 확인.|
|boolean|isEmpty()|컬렉션이 비어있는지 확인한다.|
|boolean|equals(Object o)|매개변수로 넘어온 객체와 같은 객체인지 확인한다.|


###### 3. 데이터 삭제
| Return Type | Method(args)| explain|
| :---------: | ----------- | ----- |
|void|clear()|컬렉션에 있는 모든 요소 데이터를 지운다.|
|boolean|remove(Object o)|매개 변수와 동일한 객체를 삭제한다|
|boolean|removeAll(Collection c)|매개변수로 넘어온 객체들을 해당 컬렉션에서 삭제한다.|

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
##### List
> [[List]] : 순서가 있는 데이터의 집합, 데이터의 중복을 허용한다.
- 예) 대기자 명단
1. ArrayList
2. Vector
3. LinkedList

---
##### Set
>[[Set]] : 순서를 유지하지 않는 데이터의 집합, 데이터의 중복을 허용하지 않는다. 
- 예) 양의 정수 집합, 소수의 집합
1. HashSet
2. TreeSet
---
##### Map
>[[Map]] : 키(Key)와 값(Value)의 쌍(pair)으로 이루어진 데이터의 집합 
>순서는 유지되지 않으며, 키는 중복을 허용하지 않고, 값은 중복을 허용한다.
- 우편번호, 지역번호
1. HashMap
2. TreeMap
3. HashTable
4. Properties