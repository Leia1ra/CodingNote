#### [[JavaScript/React/Parts/Component|Component]]
> Next.js에는 2가지 방식이 존재
- Server Component
	- 장점
		1. 로딩속도가 빠르다
		2. 검색엔진 노출에 유리하다
	- 단점
		1. HTML상에 자바스크립트 기능 넣기가 불가능하다
		2. useState, useEffect등 사용이 불가능하다

- Client Component
	- 규칙 : 상단에 `'use client'`라는 코드를 넣으면 된다.
	- 장점
		1. HTML상에 자바스크립트 기능 사용이 가능하다
		2. useState, useEffect등 사용이 가능하다
	- 단점
		1. 로딩속도가 느리다
			1. 자바스크립트가 많이 필요하다
			2. hydration이 필요로 하다
				- HTML을 사용자에게 보낸 후 JavaScript로 HTML을 다시 읽고 분석하는 일
		1. 검색엔진 노출에 불리하다

##### Import & Export 문법
```TSX
// export 'data.tsx'
let age = 20; 
export default age;

// import 'page.tsx'
import 작명 from './data' 
console.log(작명)
```


```TSX
// export 'data.tsx'
let age = 20;  
function Hello() {  
	return ( <h1>안녕하세요</h1> )
}  
export {age, Hello};

// import 'page.tsx'
import {age, Hello} from "./data"
```