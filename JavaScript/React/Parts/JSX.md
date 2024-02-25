> [[JSX|JSX]] : **J**avascript **S**yntax e**X**tension의 약자로서 ==JavaScript를 확장한 문법==
- React 프로젝트를 개발할 때 사용되므로 공식적인 자바스크립트 문법은 아니다.
- 브라우저에서 실행하기 전에 바벨을 사용하여 일반 자바스크립트 형태의 코드로 변환된다.
```JSX
// 실제 작성할 JSX 예시
function App() {
	return (
      <h1>Hello, JSX</h1>
    );
}

// 위와 같이 작성하면, 바벨이 다음과 같이 자바스크립트로 해석하여 준다.
function App() {
	return React.createElement("h1", null, "Hello, GodDaeHee!");
}
```

#### JSX 문법
##### 1. 반환(return)식
> 반드시 부모 요소 하나가 감싸는 형태여야 한다.
###### Error Case
```JSX
return (
	<h1>Hello</h1> <div>JSX</div>
);
```
	하나의 태그에 감싸지지 않은 다수의 부모요소를 Component에 기술할 경우 React는 오류가 발생한다.
###### Default Case
1. 임의의 태그 활용
```JSX
return (
	<div>
		<h1>Hello</h1> <div>JSX</div>
	</div>
);
```
2. Fragment 태그 문법 활용
```JSX
return (
	<Fragment>
		<h1>Hello</h1> <div>JSX</div>
	</Fragment>
);
```
	<Fragment>를 사용가능하긴 하지만 <div>태그보다 무거운 편이다.
3. Fragement 문법 활용
```JSX
return (
	<>
		<h1>Hello</h1> <div>JSX</div>
	</>
);
```
	'<> </>' 와 같이 빈 태그를 이용하여 하나의 태그로 반환할 수 있다. 

##### 2. Data Binding
> html에 변수를 넣을때는 `{ 중괄호 }`를 사용한다.
- Data Binding 시에는 [[표현식과 문장|표현식(Expression)]]만을 삽입할 수 있다.
- 주석 또한 `{ 중괄호 }`를 사용한다
###### 예제
```Tsx
const name = 'React!!';
return (
	<div>
		<div>Hello {name}!</div>
		 {/*데이터 바인딩이라고 함*/}
	</div>
);
```

##### 3. Attribute
1. Element에 class 넣을땐 className으로 대신한다
2. Element에 style 속성을 넣고 싶다면 `{Data Binding}`을 통한 `{Object Type}`을 사용한다
	- css의 속성은 카멜 표기법(`Camel Case`)에 준한다
	- ex) `font-size` -> `fontSize`
1. Element에 이벤트 속성 명은 카멜 표기법(`Camel Case`)에 준한다
	- `{Data Binding}`을 통하여 페이지 동작을 위한 [[표현식과 문장|표현식(Expression)]]을 기술한다.
###### 예제
```Tsx
function App():JSX.Element {
	function heartFn(){ console.log(1) }
	const post:string = '강남 우동 맛집';
	return (
	   <div className="App">
		  <div className="black-nav">
			 <h4 id={post}>블로그임</h4>
		  </div>
		  <h4 style={{color: 'red', fontSize:'16px'}}>{post}</h4>
		  <h4>{data[0]} <span onClick={heartFn}>👍</span></h4>
		 
	   </div>
	);
}
```