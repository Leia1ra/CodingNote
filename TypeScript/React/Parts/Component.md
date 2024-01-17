> 복잡한 HTML을 한 단어로 치환할 수 있는 Component문법
- 장점
	1. 반복적인 HTML 축약할 때
	2. 큰 페이지
	3. 자주 변경되는 UI
- 단점
	1. State를 가져다 쓰기 어렵다

> Component
```TypeScript
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
```TypeScript
return(
	<>
		<Modal></Modal>
		<Modal/>
	</>
)
```