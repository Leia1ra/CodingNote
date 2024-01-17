# 타임리프의 주된 기능은 th:### 형식으로 입력함  
##### Controller 예시
```Java
@GetMapping("/thymeleaf")  
public String thymeleaf(Model model){  
    model.addAttribute("msg", "thymeleaf");  
    model.addAttribute("sanitize", "<script>alert('ㅎㅇ')</script>");  
	  
    Map map = new HashMap();  
    map.put("name", "Thymeleaf");  
    map.put("type", "template");  
    HomeVO hv = new HomeVO();  
    hv.setName("lee");  
    hv.setAge(26);  
    model.addAttribute("mp", map);  
    model.addAttribute("hv", hv);  
	  
    List li = new ArrayList();  
    li.add("thy"); li.add("me"); li.add("le"); li.add("af");  
    model.addAttribute("li",li);  
	  
    List<HomeVO> hli = new ArrayList<HomeVO>();  
    hli.add(new HomeVO("lee",26));  
    hli.add(new HomeVO("hong",26));  
    hli.add(new HomeVO("kwak",27));  
    model.addAttribute("hli", hli);  
    return "example/Thymeleaf";  
}
```
---
##### 표현식
- 간단한 표현식에 대한 예시만 작성, 기능에 대한 상세는 이후 기술
###### 변수 표현식
```HTML
<div th:text="${msg}"></div> 
```
###### 선택 표현식
```HTML
<form action="#" method="post" th:object="${hv}">  
    <input type="text" th:value="*{name}">  
    <input type="text" th:value="*{['age']}">  
</form>
```
###### 메시지 표현식
> messages.properties
```properties
welcome.message=ㅎㅇ
```

```HTML
<div th:text="#{welcome.message}"></div>
```
###### URL 표현식
- Server의 Context 경로를 자동으로 붙여줌 
- 동적인 파라메터의 생성을 간편하게 지원함
```HTML
<a th:href="@{/community/write(id=${detail.id},name='leia1ra')}">글쓰기</a>
```
	 다수의 파라미터를 사용하기 위한 구분은 ,로 합니다.
###### Fragment 표현식
> 상세는 이후 단편 삽입 참고
```HTML
<div th:insert="~{example/fragment :: one}"></div>
```


---
##### 문자 삽입
###### 직접 문자 삽입
```HTML
<div th:text="${msg}">속성 값에 지정된 값을 Sanitize 하여 출력합니다.</div>  
<div th:utext="${sanitize}">속성 값에 지정된 값을 출력합니다.</div>
```
	Sanitize : 위험한 코드나 데이터를 변환 또는 제거하여 무력화 하는 것
###### 인라인 처리
``` HTML
<div>안녕하세요! [[${msg}]]입니다</div>
```
###### 값 결합
``` HTML
<div th:text="'안녕하세요! ' + ${msg} + '입니다'"></div>  
```
###### 리터럴 치환
```HTML
<div th:text="|안녕하세요! ${msg}입니다|"></div>  
```
###### Session
```HTML
<h1 th:text="${session['hello']}"></h1>  
```
###### Parameter
```HTML
<span th:text="${param.hello}"></span>
```
	 http://localhost:8080/community/?hello=gd
	 <span>gd</span>로 출력
---
##### 변수와 연산자

###### 지역변수
```HTML
<div th:with="a=1,b=2">  
	<span th:text="|${a} + ${b} = ${a+b}|"></span>  
</div>  
```
###### 비교와 등가
```HTML
<span th:text="1>=10">false</span>,
<span th:text="1<=10">true</span>,
<span th:text="1==10">false</span>,
<span th:text="1!=10">true</span>,
<span th:text="${msg}==thymeleaf">true</span>,
<span th:text="${msg}!=thymeleaf">false</span>
<!--삼항연산자-->
<div th:text="${msg}=='thymeleaf'?'thymeleaf':'JSP'"></div>
```
---
##### 조건 분기
###### if, unless 조건 분기문
```HTML
<div th:if="${msg}=='thymeleaf'">true : ㅎㅇ</div>  
<div th:unless="${msg}=='JSP'">false : ㅂㅇ</div>  
```
###### Switch 분기문
```HTML
<div th:switch="${msg}">  
	<span th:case="thymeleaf" th:text="|${msg} 입니다|"></span>  
	<span th:case="JSP" th:text="|구닥다리 ${msg} 입니다|"></span>  
	<span th:case="*">템플릿 엔진</span>  
</div> 
```
---
##### 참조
###### VO
```HTML
<form action="#">  
    vo Object 참조 ->  
    <input type="text" th:value="${hv.name}">  
    <input type="text" th:value="${hv['age']}">  
</form>
```

```HTML
<form action="#" method="post" th:object="${hv}">  
    vo th:Object 참조  
    <input type="text" th:value="*{name}">  
    <input type="text" th:value="*{['age']}">  
</form>
```
###### Map
```HTML
<div>  
	Map 데이터를 결합한 객체 참조 ->  
	<span th:text="${mp.get('name')}"></span>  
	<span th:text="${mp['type']}"></span>  
</div>  
```

```HTML
<div th:object="${mp}">  
	th:object 참조 ->  
	<span th:text="*{get('name')}"></span>  
	<span th:text="*{get('type')}"></span>  
</div>  
```
###### List
```HTML
<div>  
	List Object 참조  
	<span th:text="${li[0]}"></span>  
	<span th:text="${li[1]}"></span>  
	<span th:text="${li[2]}"></span>  
	<span th:text="${li[3]}"></span>  
</div>
```
---
##### 반복
```HTML
<div th:each="member : ${hli}">  
	<div th:text="|${member.name} : ${member.age}|"></div>  
</div>  
```
###### 상태변수
> \[요소 저장용 변수\], \[상태변수\] : \[ 반복처리할 객체\]
``` HTML
<div th:each="member, s : ${hli}" th:object="${member}" class="container">  
	<div th:text="|${s.index} : *{name} : *{age} |"></div>  
	count :   <span th:text="${s.count}">1부터 시작하는 인덱스</span>,  
	size :    <span th:text="${s.size}">반복처리하는 객체의 사이즈</span>,  
	current : <span th:text="${s.current}">현재 반복 요소의 객체를 표시</span>,  
	even :    <span th:text="${s.even}">현재 요소의 인덱스가 짝수(true)인지</span>,  
	odd :     <span th:text="${s.odd}">현재 요소의 인덱스가 홀수(true)인지</span>,  
	first :   <span th:text="${s.first}">첫번째 요소인지</span>,  
	last :    <span th:text="${s.last}">마지막 요소인지</span>  
</div>  
```

| 유틸리티 객체 | 기능 개요 |
| :---: | ---- |
| index | 0부터 시작하는 인덱스, 현재 인덱스를 표시합니다. |
| count | 1부터 시작하는 인덱스, 현재 인덱스를 표시합니다. |
| size | 반복 처리하는 객체와 사이즈를 표시합니다. |
| current | 현재 반복 요소의 객체를 표시합니다. |
| even | 현재 요소가 짝수 번째인지 여부를 결정합니다.<br>짝수이면 true, 홀수이면 false를 표시합니다. |
| odd | 현재 요소가 짝수 번째인지 여부를 결정합니다.<br>홀수이면 true, 짝수이면 false를 표시합니다. |
| first | 현재 요소가 첫 번째 요소인지 여부를 결정합니다. |
| last | 현재 요소가 마지막인지 여부를 결정합니다. |

---
##### 유틸리티 객체  
```HTML
<article class="container">  
    <h3>유틸리티 객체 : ${#~~~}을 통하여 다양한 기능 제공</h3>  
</article>
```

| 유틸리티 객체 | 기능 개요 |
| :--- | ---- |
| \#strings | 문자 관련 편의 기능 |
| \#numbers | 숫자 서식 지원 |
| \#bools | boolean관련 기능 |
| \#dates | java.util.Date 서식 지원 |
| \#objects | 객체 관련 기능 |
| \#arrays | 배열 관련 기능 |
| \#lists | List 관련 기능 |
| \#sets | Set 관련 기능 |
| \#maps | Map 관련 기능 |

---
##### 단편(fragment) 삽입
> `example/fragement.html`
```HTML
<span th:fragment="one">하나</span>  
<span th:fragment="two">둘</span>  
<span th:fragment="three">셋</span>
```

```HTML
<h3>fragment : 다른 Template 삽입하기</h3>  
<div id="one" th:insert="example/fragment :: one">
	fragment 태그 자체를 가져옴
</div>  
<div id="two" th:include="example/fragment :: two">
	fragment 태그 안의 값만 가져옴
</div>  
<div id="three" th:replace="/example/fragment :: three">
	th:replace가 선언된 태그는 fragment태그로 대체된다. 
</div>  
```
---
##### 빈블럭
> 타임리프 표현을 어느 곳에서든 사용할 수 있도록 하는 구문
```HTML
<th:block th:if="1=1">
	<div>ㅎㅇ</div>
</th:block>
```

---
# Layout
- 규칙
	1. `implementation('nz.net.ultraq.thymeleaf:thymeleaf-layout-dialect')`를 Build.gradle에 추가 후 로드한다.
	2. templates 폴더 안에 이후 코드와 같이  ==Layout== 페이지와 ==Content==페이지를 만든다
		1. ==Layout== 페이지와 ==Content== 상단 html태그 안에`xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"`를 추가한다.
		2. ==content==파일에는 `layout:decorate="~{fragments/layout}"`와 같이 경로를 추가한다.
		3. ==Layout== 과 ==Content==의 하나의 태그 안에 `layout:fragment="content"` 코드를 통해 삽입될 ==Content==의 위치를 지정한다.
			- tag의 종류는 ==Content==쪽을 따른다.
			- 태그의 속성은 ==Layout==의 속성도 추가된다.
> Layout : `templates/fragments/layout.html`
```HTML
<html xmlns:th="http://www.thymeleaf.org"  
      xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout">  
	<head>  
	    <meta charset="UTF-8">  
	    <meta name="viewport" content="width=device-width">  
	    <title>title</title>
	    <!--공통으로 필요한 정보를 기술-->
	</head>  
	<body>  
	    <header>  
			<h1>Header 입니다.</h1>
	    </header>  
	    <th:block th:id="main" layout:fragment="content"></th:block> 
	    <footer id="mainFooter">  
			<h3>Footer 입니다.</h3>  
	    </footer>  
	</body>  
</html>
```

> Content : `templates/home.html`
```HTML
<!DOCTYPE html>  
<html xmlns:th="http://www.thymeleaf.org"   
      xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"  
      layout:decorate="~{fragments/layout}">  
<head>
	<!--각 페이지별 추가로 필요한 정보를 기술-->
</head>  
    <main layout:fragment="content">  
       <section>메인 페이지입니다.</section>
    </main>  
</html>
```