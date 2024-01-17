1. html에 class 넣을땐 className
2. 변수를 html에 넣을때는 {중괄호}
3. html에 style속성을 넣고 싶다면 {Object Type}
```Typescript
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