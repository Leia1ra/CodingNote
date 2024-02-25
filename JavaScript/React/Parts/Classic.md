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