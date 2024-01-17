---
"\blink":
  - "[[Collection]]"
---
# List Interface
![[11장_List.png]]
> List인터페이스는 **중복을 허용**하면서  **저장순서가 유지**되는 컬렉션을 구현하는데 사용된다.
1. ArrayList
2. Vector
3. LinkedList
##### 주요 Method
###### 1. 데이터 추가
|Return Type|Method(args)|explain|
|:---------:|------------|-------|
|boolean|add(Object o)|요소를 추가|
|void|add(int idx, Object o)|매개변수로 넘어온 데이터를 지정된 index위치에 추가|
|boolean|addAll(Collection c)|매개변수로 넘어온 컬렉션의 모든 요소를 추가|
|boolean|addAll(int idx, Collection c)|매개변수로 넘어온 데이터를 지정된 index위치부터 추가|
###### 2. 데이터 확인
|  Return Type  | Method(args)          | explain                                                                  |
|:-------------:| --------------------- | ------------------------------------------------------------------------ |
|Object|get(int idx)|지정된 위치(index)에 있는 객체를 반환|
|int|indexOf(Object o)|지정된 객체의 위치(index)를 반환(순방향 탐색), 없으면 -1|
|int|lastIndexOf(Object o)|지정된 객체의 위치(index)를 반환(역방향 탐색), 없으면 -1|
|ListIterator|listItorator()|List의 객체에 접근할 수 있는 ListIterator를 반환|
|ListIterator|listItorator(int idx)|List의 객체의 지정된 위치에 접근할 수 있는 <br>ListIterator를 반환|
|boolean|contains(Object o)|매개변수로 넘어온 객체가 해당 컬렉션에 있는지 확인|
|boolean|containsAll(Collection c)|매개변수로 넘어온 객체들이 해당 컬렉션에 있는지 확인|
|boolean|isEmpty()|컬렉션이 비어있는지 확인|
|boolean|equals(Object)|매개변수로 넘어온 객체와 같은 객체인지 확인|
|int|size()|요소의 개수를 리턴|
###### 3. 데이터 삭제 
| Return Type | Method(args)            | explain                                              |
|:-----------:| ----------------------- | ---------------------------------------------------- |
|    void     | clear()                 | 컬렉션에 있는 모든 요소 데이터를 지운다.             |
|   boolean   | remove(Object o)        | 매개 변수와 동일한 객체를 삭제한다                   |
|   Object    | remove(int idx)         | 지정된 위치(index)에 있는 객체를 제거한다.           |
|   boolean   | removeAll(Collection c) | 매개변수로 넘어온 객체들을 해당 컬렉션에서 삭제한다. |
###### 4. 데이터 변경
| Return Type | Method(args)                 | explain                                                                                                                                                              |
|:-----------:| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|   Object    | set(int idx, Object element) | 지정된 위치(index)에 객체(element)를 저장                                                                                                                            |
|    void    | sort(Comparator c)           | 지정된 비교자(Comparator)로 List를 정렬                                                                                                                              |
|    List     | subList(int from, int to)    | 지정된 범위(from부터 to)에 있는 객체를 반환                                                                                                                          |
|    int     | hashCode()                   | 해시 코드 값을 리턴                                                                                                                                                  |
|  Iterator   | iterator()                   | 데이터를 한 건씩 처리하기 위한 iterator 객체를 리턴                                                                                                                  |
|  Iterator   | iterator(int idx)            | 데이터를 한 건씩 처리하기 위한 iterator 객체를 리턴                                                                                                                  |
|   boolean   | retainAll(Collection c)      | 지정된 Collection에 포함된 객체만을 남기고 <br>다른 객체들은 Collection에서 삭제한다.<br> 이 작업으로 인해 Collection에 변화가 있으면 true <br>그렇지 않으면 false를 반환한다. |
| Object\[\]  | toArray()                    | 컬렉션에 있는 데이터들을 배열로 복사                                                                                                                                 |
| Object\[\]  | toArray(Object[] a)          | 컬렉션에 있는 데이터들을 지정한 타입의 배열로 복사                                                                                                                   |
##### 차이점
1. 동기화(Synchronization):
	- <span style ="color:violet">ArrayList</span>: ArrayList는 스레드 안전성을 제공하지 않습니다. 즉, 여러 스레드가 동시에 ArrayList에 접근하면 외부에서 동기화를 처리해야 합니다. 이로 인해 ArrayList는 빠른 접근 및 간단한 구현을 제공하며, 성능 측면에서 동기화 오버헤드가 적습니다.
	- <span style ="color:lime">Vector</span>: Vector는 스레드 안전(**Thread Safe**)한 클래스로, 내부적으로 동기화를 처리합니다. 따라서 여러 스레드에서 동시에 Vector에 접근하는 경우에도 안전하게 사용할 수 있지만, 이로 인해 동기화 관련 오버헤드가 있어 ArrayList보다 성능이 떨어질 수 있습니다.
	- <span style ="color:orange">LinkedList</span>: LinkedList는 동기화를 지원하지 않습니다. 따라서 여러 스레드에서 동시에 접근하는 경우에는 외부에서 동기화를 관리해야 합니다.

2. 내부 구조:
	- <span style ="color:violet">ArrayList</span> 및 <span style ="color:lime">Vector</span>: ArrayList와 Vector는 배열 기반의 동적 배열 구조를 사용합니다. 요소를 배열에 저장하고 필요에 따라 크기를 동적으로 조절합니다.
	- <span style ="color:orange">LinkedList</span>: LinkedList는 이중 연결 리스트 구조를 사용합니다. 각 요소는 자신의 이전 요소와 다음 요소에 대한 참조를 가지고 있으며, 요소를 추가하거나 삭제하는데 효율적입니다.

4. 크기 조절:
	 - <span style ="color:violet">ArrayList</span> 및 <span style ="color:lime">Vector</span>: ArrayList와 Vector는 요소를 추가하거나 제거할 때 자동으로 크기를 조절합니다.
	- <span style ="color:orange">LinkedList</span>: LinkedList도 크기를 동적으로 조절할 수 있지만, 크기 조절은 수동으로 처리해야 합니다.

5. 성능:
	- <span style ="color:violet">ArrayList</span> 및 <span style ="color:lime">Vector</span>: ArrayList와 Vector는 요소의 삽입 또는 삭제가 끝에서 빠르지만 중간에서는 느릴 수 있습니다. ArrayList는 50% 증가량을 사용하고, Vector는 100% 증가량을 사용합니다.
	- <span style ="color:orange">LinkedList</span>: LinkedList는 요소의 삽입 또는 삭제가 중간에서 효율적이며, 끝에서의 삽입 및 삭제도 빠릅니다. 그러나 요소를 찾는데는 ArrayList나 Vector보다 느릴 수 있습니다.

6. 사용 사례:
	- <span style ="color:violet">ArrayList</span> 및 <span style ="color:lime">Vector</span>: ArrayList와 Vector는 대부분의 상황에서 사용됩니다. 다만, Vector는 Multi-threaded 환경에서 사용하고자 할 때 또는 Java 1.0 이전 버전과의 호환성을 유지해야 할 때 사용됩니다.
	- <span style ="color:orange">LinkedList</span>: LinkedList는 요소의 삽입 또는 삭제가 빈번하게 발생하는 경우에 사용하며, 성능적인 이점을 살리고자 할 때 유용합니다.
### ArrayList
```java
/*생성시*/
ArrayList al1 = new ArrayList();
// ArrayList를 생성

ArrayList al2 = new ArrayList(Collection c);
// 주어진 Collection이 저장된 ArrayList를 생성

ArrayList al3 = new ArrayList(int intialCapacity);
// 지정된 초기용량을 갖는 ArrayList를 생성
```
##### 주요 Method
|Return Type|Method(args)|explain|
|:---------:|------------|-------|
| Object | clone() | ArrayList를 복제 |
| void | ensureCapacity(int minCapacity) | ArrayList의 용량이 최소 minCapacity가 되도록 변경 |
| void | trimToSize() | 용량을 크기에 맞게 줄임(빈 공간을 없앤다.) |

---
### Vector
```java
/*생성시*/
Vector v1 = new Vector();
// Vector를 생성

Vector v2 = new Vector(Collection c)
// 주어진 Collection이 저장된 Vector를 생성

Vector v3 = new Vector(int initialCapacity);
// 지정된 초기 용량을 갖는 Vector를 생성

Vector v4 = new Vector(int initialCapacity, int capacityIncrement);
// 지정된 초기용량과 용량증가치를 갖는 Vector를 생성
```
##### 주요 Method
###### 1. 데이터 추가
|Return Type|Method(args)|explain|
|:---------:|------------|-------|
|void| addElement(E obj)| Vector의 끝에 지정된 요소(obj)를 추가|
|void| insertElementAt(E obj, int index)| Vector에 지정된 index에 지정된 객체를 삽입|
###### 2. 데이터 확인
| Return Type | Method(args) | explain                               |
|:-----------:| ------------ | ------------------------------------- |
|     int     | capacity()   | Vector의 최대 수용 가능한 크기를 반환 |
|void| copyInto(Object\[\] anArray)| 구성요소를 지정된 배열에 복사|
| Enumeration | element()    | 구성요소에 대한 Enumeration을 반환    |
|E| elementAt(int index)| 지정된 index에 구성요소를 반환|
|E| firstElement()| 첫번째 구성요소를 반환|
|E| lastElement()|Vector의 마지막 구성요소를 반환|
|int| indexOf(Object o, int index)| 지정된 요소가 처음으로 나타나는 인덱스를 반환<br> 지정된 index보다 앞에 있거나, <br>요소를 찾을 수 없으면 -1을 반환합니다. |
|int| lastIndexOf(Object o, int index) |지정된 요소가 마지막으로 나타나는 인덱스를 반환<br> 지정된 index보다 뒤에 있거나, <br>요소를 찾을 수 없으면 -1을 반환합니다. |
###### 3. 데이터 삭제 
| Return Type | Method(args)            | explain                                              |
|:-----------:| ----------------------- | ---------------------------------------------------- |
|void|removeAllElements()|Vector의 모든 구성요소를 제거하고 크기를 0으로 변경|
|boolean|removeElement(Object obj)|요소의 첫 번째(인덱스가 가장 낮은) 항목을 제거|
|void|removeElementAt(int index)|지정된 인덱스의 구성요소를 제거|
|boolean|removeIf<br>(Predicate\<? superE\> filter)|주어진 조건을 만족하는 컬렉션의 모든 요소를 제거|
|protected <br>void|removeRange<br>(int fromIndex, int toIndex)|fromIndex(포함)와 toIndex(제외) 사이에 있는 <br>모든 요소를 ​​제거합니다.|
###### 4. 데이터 변경
| Return Type | Method(args)                                | explain                                                |
|:-----------:| ------------------------------------------- | ------------------------------------------------------ |
|    void     | setElement(E obj, int index)                | 지정된 인덱스에 있는 구성요소를 지정된 객체로 설정     |
|    void     | setSize(int newSize)                        | Vector의 크기를 변경                                   |
|    void     | ensureCapacity(int minCapacity)             | Vector의 용량이 최소 minCapacity가 되도록 변경         |
|    void     | replaceAll<br>(UnaryOperator\<E\> operator) | 각 요소를 해당 요소에 연산자를 적용한 결과로 변경 |
|Spliterator\<E\>|spliterator()|Vector의 요소를 병렬 처리할 수 있는 <br>Spliterator 인터페이스를 반환 <br>Spliterator는 요소를 분할하고 <br>병렬 작업을 지원하는 반복자를 반환|
##### Stack
| Return Type | Method(args) | explain |
|:-----------:| ------------ | ------- |
|boolean|empty()|스택이 비어 있는지 확인합니다.|
|E|peek()|스택의 맨 위 요소를 확인합니다.|
|E|pop()|스택의 맨 위 요소를 제거하고 반환합니다.|
| E   | push(E item) | 요소를 스택의 맨 위에 추가합니다. |
|int|search(Object o)|지정된 요소의 스택에서의 위치를 확인합니다.|

---
### LinkedList
```java
/*생성시*/
LinkedList() ll1 = new LinkedList();
// LinkedList를 생성

LinkedList() ll2 = new LinkedList(Collection c);
// 주어진 Collection이 저장된 LinkedList를 생성
```
##### 주요 Method
###### 1. 데이터 추가
| ReturnType | Method(args) | Explain |
|:----------:| ------------ | ------- |
|void| push(Object o)| 리스트의 처음에 요소 추가|
|void| addFirst(E element)| 리스트의 처음에 요소 추가|
|void| addLast(E element)| 리스트의 끝에 요소 추가|
|boolean| offer(E e)| 요소를 리스트 끝에 추가하고 성공 여부 반환   |
|boolean| offerFirst(E e)| 리스트의 처음에 요소 추가하고 성공 여부 반환 |
|boolean| offerLast(E e)| 리스트의 끝에 요소 추가하고 성공 여부 반환   |
###### 2. 데이터 확인
| ReturnType | Method(args) | Explain |
|:----------:| ------------ | ------- |
|E| getFirst()| 리스트의 처음 요소 반환|
|E| getLast()| 리스트의 마지막 요소 반환|
|E| pop()|첫 번째 요소를 제거하고 반환|
|E| element()|첫 번째 요소를 제거하지 않고 반환|
|E| poll()| LinkedList의 첫 번째 요소를 제거하고 반환|
|E| pollFirst()| LinkedList의 첫 번째 요소를 제거하고 반환, 비어 있으면 null을 반환|
|E| pollLast()| LinkedList의 마지막 요소를 제거하고 반환, 비어 있으면 null을 반환|
|E| peek()| LinkedList의 첫 번째 요소를 제거하지 않고 반환|
|E| peekFirst()| LinkedList의 첫 번째 요소를 제거하지 않고 반환, <br>비어 있으면 null을 반환|
|E| peekLast()| LinkedList의 마지막 요소를 제거하지 않고 반환, <br>비어 있으면 null을 반환|
|Iterator<br>\<E\>|descendingIterator()|deque의 요소에 대한 반복자를 역순으로 반환|
|Spliterator<br>\<E\>|spliterator()|Vector의 요소를 병렬 처리할 수 있는 <br>Spliterator 인터페이스를 반환 <br>Spliterator는 요소를 분할하고 <br>병렬 작업을 지원하는 반복자를 반환|
###### 3. 데이터 삭제 
|ReturnType|Method(args)|Explain|
|:---:|---|---|
| E | remove() | 첫 번째 요소 제거 |
| E | removeFirst() | 첫 번째 요소 제거 |
| E | removeLast() | 마지막 요소 제거 |
|boolean|removeFirstOccurence(Object o)|LinkedList에서 첫번째로 일치하는 요소를 제거|
|boolean|removeLastOccurence(Object o)|LinkedList에서 첫번째로 일치하는 요소를 제거|
###### 4. 데이터 변경
| ReturnType | Method(args)              | Explain                    |
|:----------:| ------------------------- | -------------------------- |
| E | set(int index, E element) | 지정한 위치의 요소 변경 |

##### Queue
| ReturnType | Method(args) | Explain                                       |
| ---------- | ------------ | --------------------------------------------- |
| boolean    | add(E e)     | 요소를 큐에 추가합니다.                       |
| E          | remove()     | 큐의 맨 앞에 있는 요소를 제거하고 반환합니다. |
| E          | element()    | 큐의 맨 앞에 있는 요소를 확인합니다.          |
| boolean    | offer(E e)   | 요소를 큐에 추가합니다.                       |
| E          | poll()       | 큐의 맨 앞에 있는 요소를 제거하고 반환합니다. |
| E          | peek()       | 큐의 맨 앞에 있는 요소를 확인합니다.          |

---













