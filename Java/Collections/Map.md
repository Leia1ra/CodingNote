---
"\blink":
  - "[[Collection]]"
---
![[11장_Map.png]]
>[Map] : 키(Key)와 값(Value)의 쌍(pair)으로 이루어진 데이터의 집합 
>순서는 유지되지 않으며, 키는 중복을 허용하지 않고, 값은 중복을 허용한다.
- 우편번호, 지역번호
1. HashMap
2. TreeMap
3. HashTable
4. Properties

###### 1. 데이터 추가
| Return Type | Method(args)| explain|
|:-----------:| --- | --- |
|v|put(K key, V value)|Map에 value객체를 key객체에 연결(mapping)하여 저장한다.|
|void|putAll(Map\<K,V> m)|지정된 Map의 Key-Value쌍을 추가한다.|
###### 2. 데이터 확인
|Return Type|Method(args)|explain|
|:---:| --- | --- |
|boolean|containsKey(Object k)|지정된 K객체와 일치하는 Map의 Key객체가 있는지 확인|
|boolean|containsValue(Object v)|지정된 V객체와 일치하는 Map의 value객체가 있는지 확인|
|boolean|equals(Object o)|동일한 Map인지 비교한다.|
|boolean|isEmpty()|Map이 비어있는지 확인한다.|
###### 3. 데이터 삭제
| Return Type | Method(args) | explain                     |
|:-----------:| ------------ | --------------------------- |
|    void     | clear        | Map의 모든 객체를 삭제한다. |
|V|remove(Object k)|지정한 key객체와 일치하는 key-value객체를 삭제한다.|
###### 4. 데이터 변경
|      Return Type      | Method(args)  | explain|
|:---:| --- | ---- |
|Set\<Map.Entry\<K,V>>|entrySet()|Map에 저장된 K-V쌍을 Map.Entry타입으로 저장한 Set으로 반환|
|int|hashCode()|해시코드를 반환한다.|
|V| et(Object k)|지정한 Key객체에 대응하는 value객체를 찾아서 반환한다.|
|Set\<K>|keySet()|Map에 저장된 모든 key객체를 반환한다.|
|int|size()|Map에 저장된 Key-Value쌍의 개수를 반환한다.|
|Collection\<V>|values|Map에 저장된 모든 value 객체를 반환한다.|
### Map.Entry
> Map.Entry Interface는 Map 내부 인터페이스로서, Map에 저장되는 key-value쌍을 다루기 위해 내부적으로 Entry Interface를 정의했다.

| Return Type | Method(args)     | explain                       |
|:-----------:| ---------------- | ----------------------------- |
|  boolean   | equals(Object o) | 동일한 Entry인지 비교한다.    |
|   Object   | getKey()         | Entry의 Key객체를 반환한다.   |
|   Object    | getValue()       | Entry의 Value객체를 반환한다. |
|     int     | hashCode()       | Entry의 해시코드를 반환한다.  |
|Object|setValue(Object v)|Entry의 value객체를 지정된 객체로 바꾼다.|
