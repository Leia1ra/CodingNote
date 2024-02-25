> 부모 [[JavaScript/React/Parts/Component|컴포넌트]]의 [[State|State]]를 자식 [[JavaScript/React/Parts/Component|컴포넌트]]로 전송하여 사용할 수 있는 Props문법
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