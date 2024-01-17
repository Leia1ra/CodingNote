#### [[Layout|Layout]]
1. html에 class 넣을땐 className
2. 변수를 html에 넣을때는 {중괄호}, ==데이터 바인딩==이라 명함
3. html에 style속성을 넣고 싶다면 {Object Type}
```Tsx
function App():JSX.Element {  
    const post:string = '강남 우동 맛집';  
    return (  
       <div className="App">  
          <div className="black-nav">  
             <h4 id={post}>블로그임</h4>  
          </div>  
          <h4 style={{color: 'red', fontSize:'16px'}}>{post}</h4>
          {/*데이터 바인딩이라고 함*/}  
       </div>  
    );  
}
```
	css의 속성은 font-size가 아닌, fontSize로 카멜 표기법(Camel Case)에 준한다
##### 이벤트 처리
> 이벤트 처리에 있어서 JSX에서는 이하와 같은 규칙이 있음
1. 이벤트 method 명은 카멜 표기법(Camel Case)에 준한다
2. {중괄호}를 사용한다.
3. 단순 코드가 아닌 함수를 넣어야 한다.
###### 예제
1. 명명 함수 선언
```TypeScript
function heartFn(){ console.log(1) }  
```

```Tsx
<h4>{data[0]} <span onClick={heartFn}>👍</span></h4>  
```
2. 익명 함수 선언
```Tsx
<h4>{data[1]} <span onClick={function () {console.log(1)}}>👍</span></h4>  
```
3. 화살표 함수
```Tsx
<h4>{data[2]} <span onClick={()=>{console.log()}}>👍</span></h4>   
```
---
#### [[State|State]]
> state는 변동사항이 생기면 해당 state를 사용하는 HTML도 ==자동으로 재렌더링== 한다.
> 따라서 자주 변경하는 html부분은 state로 만들어놓는게 좋다
```TypeScript
/*Destructuring 문법*/
const [data, setData] = useState("남자 코트 추천");
const [data, setData]:[string,React.Dispatch<React.SetStateAction<string>>] 
	= useState("남자 코트 추천"); // 정확한 타입명 기재한 방법
```
	 자료를 잠깐 저장할 시, 단순 변수를 활용하는 편이 더 좋음
##### SetState함수
```TypeScript
const [heart, setHeart] = useState(0);  
function heartFn(){ setHeart(heart+1); }  
```
	 setState를 할땐 HTML 이 재렌더링 되므로 콜백함수, 혹은 특정한 이벤트로 호출이 되지 않고서는 
	 무한 loop에 걸릴 가능성이 있다.
###### Refference Data Type
> 참조형 데이터 타입의 문제
```Tsx
const [data, setData] = useState(["남자 코트 추천", "강남 우동 맛집", "리엑트 독학"]);
return (
	<div className="list">  
	    <button onClick={()=>{  
	       data[0] ='여자 코트 추천';  
	       setData(data);  
	    }}>변경</button>
	      
	    <h4>{data[0]}</h4>
	</div>
)
```
	 참조형 변수는 선언시 데이터를 저장하는 주소를 가리킨다.
	 내부적으로 무언가 달라졌다고 해도  가리키는 주소는 변하지 않는다.
	 따라서 useState를 통한 HTML 재랜더링을 기대하기 어렵다

>해결
```Tsx
const [data, setData] = useState(["남자 코트 추천", "강남 우동 맛집", "리엑트 독학"]);
return (
	<div className="list">  
	    <button onClick={()=>{  
	    data[0] ='여자 코트 추천';  
	    setData([...data]); /*Spread Operator*/
	    /*...은 괄호를 한번 벗겨달라는 식인가봄*/  
	}}>변경</button>
	    <h4>{data[0]}</h4>
	</div>
)
```
	 Spread 연산자 : []를 제거해주는 문법

###### 이벤트 처리
- setState함수는 늦게 처리된다.
```TSX
<input type="text"  
       onChange={(event)=>{  
          setInputData(event.target.value);  
          console.log(inputData)  
       }}/>
```
---
#### [[TypeScript/React/Parts/Component|Component]]
> 복잡한 HTML을 한 단어로 치환할 수 있는 Component문법
- 장점
	1. 반복적인 HTML 축약할 때
	2. 큰 페이지
	3. 자주 변경되는 UI
- 단점
	1. State를 가져다 쓰기 어렵다
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
##### Fragment 문법
`하나의 태그에 감싸지지 않은 다수의 부모요소를 Component에 기술할 경우 React는 오류가 발생한다.`
- `<> </>`와 같이 빈 태그를 이용하여 하나의 태그로 반환할 수 있다. 
> Other Component
```Tsx
return(
	<>
		<Modal></Modal>
		<Modal/>
	</>
)
```
---
#### [[Props|Props]]
> 부모 [[TypeScript/React/Parts/Component|컴포넌트]]의 [[State|State]]를 자식 [[TypeScript/React/Parts/Component|컴포넌트]]로 전송하여 사용할 수 있는 Props문법
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
							if(modal[index]) {  
								modal[index] = !modal[index]  
							} else {  
								modal[index] = !modal[index]  
							}  
								setModal([...modal])  
							}}>  
							{value}  
						</h4>  
						{modal[index] ? 
							<Modal i={index} p={data} set={setData}/> : null}  
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
#### [[Classic|Classic]]
> 과거의 React에서는 class 문법을 사용했었으나, 현재 function을 사용할 것을 권장
-  규칙
	1. `class 클래스이름`로 작성하고 컴포넌트 이름을 작명
	2. constructor, super, render함수 3개를 채움
	3. 컴포넌트는 길고 복잡한 HTML문법을 return에 작성
```TSX
type StateType = {  
    name:String;  
    age:number  
}  
  
class Modal2 extends React.Component<any, StateType>{  
    constructor(props?:Readonly<any>|any) {  
       super(props);  
       this.state = {  
          name:'kim',  
          age:20  
       }  
    }  
    render() {  
       return (  
          <section>  
             <article>  
                안녕 State {this.state.name} {this.state.age}  
             </article>  
             <article>  
                안녕 Props {this.props.p.name} {this.props.p.age}  
             </article>  
             <button onClick={()=>{  
                this.setState({age:21})  
             }}>버튼</button>  
          </section>
       );  
    }  
}
```
	 TypeScript기준 제너릭 타입을 지정해주지 않으면 오류가 발생함(any:props, StateType:state 타입)

```TSX
function App (){ 
	return (
		<Modal2 p={{name:"lee",age: 26}}/>
	)
}
```
---

React dom
Recoil
React Component Style