### JSX
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
###### - Error Case
```JSX
return (
	<h1>Hello</h1> <div>JSX</div>
);
```
	하나의 태그에 감싸지지 않은 다수의 부모요소를 Component에 기술할 경우 React는 오류가 발생한다.
###### - Resolved Case
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
---
### Hooks
```etc
	리액트 16.8 이전 버전에서는 함수형 컴포넌트에서는 상태를 관리할 수 없었다. 
	즉 State, Life Cycle 등등 이 없기 떄문에 클래스형 컴포넌트를 사용 했었다.
	리액트 16.8부터 Hooks 라는 기능이 도입되면서 함수형 컴포넌트에서도 상태를 관리할 수 있게 되었다.
	useState Hook을 사용하여 State 사용이 가능하다.
```
> 리액트의 Hook 은 **함수형** 컴포넌트에서 state와 생명주기 기능을 “연동(hook into)“할 수 있게 해주는 함수
> Hook은 class 안에서는 동작하지 않고, class 없이 React를 사용할 수 있게 해준다.
- 리액트 훅을 도입한 목적
	1. 컴포넌트에서 상태관련 로직을 사용할 때 Hook 이전에 재사용 가능한 로직을 사용하기 위해서는, render props나 고차 컴포넌트와 같은 패턴을 사용했는데, 이런 패턴은 코드의 추적을 어렵게 만들었다.  Hook을 활용하면 ==상태 관련 로직을 추상화해 독립적인 테스트와 재사용이 가능해 레이어 변화 없이 재사용==할 수 있다.
	2. 기존 `LifeCycle` 메서드 기반이 아닌 로직 기반으로 ==컴포넌트를 함수 단위로 잘게 쪼갤 수 있다는 이점== 
		`(LifeCycle 메서드에는 관련 없는 로직이 자주 섞여, 이로인해 버그가 쉽게 발생하고, 무결성을 쉽게 해친다.)`
	3. 그 외에도 클래스 기반 컴포넌트를 지양하고자 하는 목적 등도 있다
- 규칙
	1. 최상위에서만 `Hook`의 호출을 권장
		- 반복문, 조건문, 중첩된 함수 내에서 `Hook`을 실행하면 안된다.
		- 이 규칙을 따르면 `Component`가 렌더링 될 때마다 항상 동일한 순서로 `Hook`을 호출되는 것이 보장된다.
	2. 리액트 함수 컴포넌트에서만 `Hook`을 호출해야하고, 일반 `JavaScript`함수에서는 `Hook`을 호출 불가능
#### Life Cycle
- 모든 리엑트 `Component` 들에는 생명주기(`LifeCycle`)가 존재한다.
	 - `class Component`는 생명주기(`LifeCycle`) 메서드를 활용
	 - `functional Component`는 훅`Hooks`메서드를 활용
 - `생성(Mounting) -> 갱신(Updata) -> 제거(Unmounting)`의 생명주기를 갖는다.
##### 생명 주기 메소드
![Life Cycle](./Datas/React_LifeCycle.png)
###### 생성(Mount)
> `component`의 인스턴스가 생성되어 `DOM`에 삽입될 때 순서대로 호출된다.
1. constructor()
	- 컴포넌트를 새로 만들 때마다 호출되는 클래스 생성자 메서드
	- `this.props`, `this.state`에 접근할 수 있으며 `React`요소를 반환한다.
	- `setState()`를 사용할 수 없으며 `Dom`에 접근해선 안된다.
2. getDerivedStateFromProps()
	- 마운트 과정에서 호출되며, 업데이트가 시작하기 전에도 호출된다.
	- `props`에 있는 값을 `state`에 동기화 시킬 때 사용하는 메서드
3. render()
	- UI를 렌더링하는 메서드
4. componentDidMount()
	- 컴포넌트가 웹 브라우저 상에 나타난 후 즉 첫 렌더링을 마친 후에 호출하는 메서드
	- `setTimeout`, `setInterval`과 같은 비동기 작업의 처리와 `setState` 호출을 하는 경우가 많다
###### 갱신(Update)
> `props`나 `state`가 변경되면 렌더가 진행되며 순서대로 호출된다.
1. getDerivedStateFromProps()
	- 마운트 과정에서 호출되며, 업데이트가 시작하기 전에도 호출된다.
	- `props`에 있는 값을 `state`에 동기화 시킬 때 사용하는 메서드
2. shouldComponentUpdate()
	- `props`또는 `state`를 변경했을 때, 재렌더링을 시작할지 여부를 지정하는 메서드
	- `true`를 반환하면 다음 `LifeCycle` 메서드를 계속 실행하고, `false`를 반환하면 작업을 중지
3. render()
	- UI를 렌더링하는 메서드
4. getSnapshotBeforeUpdate()
	- `Component` 변화를 `DOM`에 반영하기 바로 직전에 호출하는 메서드
5. componentDidUpdate()
	- `Component` 업데이트 작업이 끝난 후 호출하는 메서드
###### 제거(Unmount)
> `Component`를 `DOM`에서 제거하는 과정
1. componentWillUnmount()
	- `Component`를 `DOM`에서 제거할 때 실행한다.
	- 이후에 `Component`는 다시 렌더링 되지 않으므로, 여기에서 setState()를 호출해선 안된다.


#### useState
> [[JavaScript/React/Parts/State|State]] : React [[JavaScript/React/Parts/Component|Component]]의 동적인 값을 상태(`State`)라고 부른다.
- 특징
	- `state`는 변동사항이 생기면 해당 `state`를 사용하는 `HTML`도 ==자동으로 재렌더링== 한다.
		따라서 자주 변경하는 `HTML`부분은 `state`로 만들어놓는게 좋다,
		`setState를 할땐 HTML 이 재렌더링 되므로 콜백함수, 혹은 특정한 이벤트로 호출이 되지 않고서는 무한 loop에 걸릴 가능성이 있다.`
	- `대입 연산자(=)`를 통한 `state`의 직접적인 변경은 불허한다.
```TypeScript
/*Destructuring 문법*/
const [data, setData] = useState("남자 코트 추천");
const [data, setData]:[string,React.Dispatch<React.SetStateAction<string>>] 
	= useState("남자 코트 추천"); // 정확한 타입명 기재한 방법
```

##### SetState함수
###### Primitive Data Type
1. setState 내에 변경할 값을 넣기
```TypeScript
const [heart, setHeart] = useState(0);  
setHeart(heart+1);
```
2. CallBack 함수를 통한 값 리턴
```TypeScript
const [heart, setHeart] = useState(0);  
setHeart((current)=>{return current+1});
```
###### Reference Data Type
+ 참조 타입(Reference Type)의 문제
	`참조형 변수는 선언시 데이터를 저장하는 주소를 가리킨다. 내부적으로 특정 값이 변경되어도, 가리키는 객체의 주소는 변하지 않기에 useState를 통한 View(HTML)의 재렌더링을 기대하긴 어렵다.`
 - Error Case
```Tsx
const [data, setData] = useState(["남자 코트 추천", "강남 우동 맛집", "리엑트 독학"]);
return (
  <div className="list">  
	<button onClick={()=>{data[0] ='여자 코트 추천'; setData(data);}}>변경</button>
	<h4>{data[0]}</h4>
  </div>
)
```
- Resolved Case
```Tsx
const [data, setData] = useState(["남자 코트 추천", "강남 우동 맛집", "리엑트 독학"]);
return (
  <div className="list">                       /*Spread Operator*/
	<button onClick={()=>{data[0] ='여자 코트 추천'; setData([...data]);}}>변경</button>
	<h4>{data[0]}</h4>
  </div>
)
```
	 ...(Spread 연산자) : 배열이나 객체의 전체 또는 일부등의 원소를 나열(spread out)할 수 기능

###### 4. 이벤트 처리
- setState함수는 늦게 처리된다.
```TSX
<input type="text" onChange={
	(event)=>{ setInputData(event.target.value); console.log(inputData); }}/>
```
#### useEffect
> `Component`가 렌더링 될때마다 특정 작업(`effect`)을 실행할 수 있도록 하는 `Hook`
> `side effect`: 코드가 의도한 주된 효과 외에 **추가적으로 발생하는 부수 효과**

[참고 링크](https://velog.io/@sukong/REACT-%EB%A6%AC%EC%95%A1%ED%8A%B8%EC%9D%98-%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0%EC%99%80-useEffect-Hook)
> React의 class 생명주기 메서드에 친숙하다면, `useEffect` Hook을 `componentDidMount`와 `componentDidUpdate`, `componentWillUnmount`가 합쳐진 것으로 생각해도 좋습니다.


---
### Component
> [[JavaScript/Next/Parts/Component|Component]] : 복잡한 HTML을 한 단어로 치환할 수 있는 Component문법
- 장점
	1. 반복적인 HTML 축약할 때
	2. 큰 페이지 혹은 자주 변경되는 UI의 유지 보수가 용이하다.
> Component
```Tsx
function Modal() {  
	return (
		<div className="modal">  
			<h4>제목</h4>  
			<p>날짜</p>  
			<p>상세 내용</p>  
		</div>  
	)  
}
```
##### 요소 반복
> javaScript의 내장함수인 map()함수를 통해 요소를 반복 출력 하는 방식
- JSX의 특성상 반복되는 Element는 `key`값을 넣어주어야 한다.
	- 단순 `index`를 `key`값으로 사용한다면, `state`에 관련하여 의도하지 않은 문제를 야기할 수 있음.
```TSX
function NumberList() {
	const numbers = ["Data1", "Data2", "Data3", "Data4", "Data5"];
	return (
	    <div>
		{   numbers.map((value, index) => {
				return ( <span key={value}>{value}</span> )
			});
		}
		</div>  
    );
}
```

---
### Props
> [[Props|Props]] : 부모 [[JavaScript/React/Parts/Component|컴포넌트]]의 [[State|State]]를 자식 [[JavaScript/React/Parts/Component|컴포넌트]]로 전송하여 사용할 수 있는 Props문법
- 규칙
	1. 자식 컴포넌트 사용하는 곳에 가서 `<자식컴포넌트 변수명={state이름}>` 로 기재
		- `<Modal 변수명={변수}>` 일반 변수, 함수 전송도 가능
		- `<Modal 변수명="강남우동맛집">` 일반 문자전송은 중괄호 없이 가능
	2. 자식 컴포넌트 만드는 함수로 가서 파라미터 등록 후 `파라미터.변수명` 사용
- 제약
	1. 자식 -> 부모 방향의 전송은 불가능
	2. 다른 자식 컴포넌트로의 전송도 불가능
```tsx
function App (){ 
	let [글제목, 글제목변경] = useState(['남자코트 추천', '강남 우동맛집', '파이썬독학']); 
	return (
		<div className="App">  
		{
			data.map((value, index, array)=>{  
				return (  
					<div className="list" key={index}>  
						<h4 onClick={() => {  
							if(modal[index]) { modal[index] = !modal[index] } 
							else { modal[index] = !modal[index] }  
							setModal([...modal])  
						}}>{value}</h4>  
						{modal[index]?<Modal i={index} p={data} set={setData}/>:null}  
					</div>  
				) 
			})
		}  
		</div>
	)
}
```

> Component
```Tsx
type Props = {  
    i:number;  
    p:string[];  
    set:React.Dispatch<React.SetStateAction<string[]>>;  
    c?:string;  
}
function Modal(props:Props) {  
	return (  
       <div className="modal" style={{color: props.c, background:"lightyellow"}}>  
          <h4>{props.p[props.i]}</h4>  
          <p>날짜</p>  
          <p>상세 내용</p>  
          <button onClick={() => {  
             props.p[props.i] = "여자 코트 추천";  
             props.set([...props.p])  
          }}>변경  
          </button>  
       </div>  
    )}
```
---
### Classic React
> [[Classic|Classic React]] : 과거의 React에서는 class 문법을 사용했었으나, 현재 function을 사용할 것을 권장
-  규칙
	1. `class 클래스이름`로 작성하고 컴포넌트 이름을 작명
	2. constructor, super, render함수 3개를 채움
	3. 컴포넌트는 길고 복잡한 HTML문법을 return에 작성
```TSX
type StateType = {  
    greet:string  
}  
type PropsType = {  
	name:string,  
	age:number  
}  
  
export class Classic extends React.Component<PropsType, StateType>{  
	constructor(props:Readonly<PropsType>|PropsType) {  
		super(props);  
		this.state = {  
			greet:"ㅎㅇ"  
		}  
	}
	componentDidMount(){ /*Classic 컴포넌트가 로드되고나서 실행할 코드*/ }
	componentDidUpdate(){ /*Classic 컴포넌트가 업데이트 되고나서 실행할 코드*/}
	componentWillUnmount(){ /*Classic 컴포넌트가 삭제되기전에 실행할 코드*/ }
    render(){  
       return (  
			<div>
				<button onClick={e=> {  
				    if(this.state.greet === 'ㅎㅇ') {  
						e.currentTarget.innerHTML = '입장'  
						this.setState({greet: 'ㅂㅇ'})  
				    } else {  
						e.currentTarget.innerHTML = '퇴장'  
						this.setState({greet: 'ㅎㅇ'})  
					}  
				}}>떠나기</button>
				<div>{this.state.greet}</div>  
				<div>{this.props.name} : {this.props.age}</div>  
			</div>  
		)    
	}  
}
```
	 TypeScript: 제너릭 타입을 지정하지 않을 시 오류발생(PropsType:props, StateType:state 타입)
	 - run dev 실행시 mount, unmount 함수를 각각 한번씩 우선 실행함

```TSX
function App (){ 
	return (
		<Classic name={'이선왕'} age={26}/>
	)
}
```
---

redux
React dom
Recoil
React Component Style