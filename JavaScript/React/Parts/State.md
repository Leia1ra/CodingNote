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