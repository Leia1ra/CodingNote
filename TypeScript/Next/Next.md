> 기본적인 문법은 [[React|React]]를 따르는 듯 함

---
#### [[Route|Route]]
>  URL로 페이지를 나누는것을 라우팅이라고 한다.
- 규칙(예시 `http://localhost:3000/list`)
	1. `app`폴더 안에 `list` 폴더 생성
	2. `list` 폴더 안에 `page.js`를 생성
	3. 필요시 `layout.js`를 생성
		- 상위 폴더의 layout안에 하위폴더의 layout을 포함하여 보여줌
```TSX
// /app/list/page.tsx
function List() {
    return (  
       <div>  
          <h4 className="title">상품목록</h4>  
          <div className="food">  
             <h4>상품1 $40</h4>  
          </div>  
          <div className="food">  
             <h4>상품2 $40</h4>  
          </div>  
       </div>  
    )}  
export default List;
```

```TSX
// /app/layout.tsx
import './globals.css'  
import Link from "next/link";  

export default function RootLayout({children}: { children: React.ReactNode }) {  
    return (  
        <html lang="en">  
            <body className={inter.className}>  
                <nav className="navbar">  
                    <Link href="/">홈</Link>  
                    <Link href="/list">List</Link>  
                    <Link href="/cart">cart</Link>  
                </nav>  
                {children}  
            </body>  
        </html>  
    )
}
```

##### Dynamic Route
- 규칙(예시 `http://localhost:3000/detail/[id]`)
	1. 기존 Route 규칙을 따라 `detail`폴더 생성
	2. `[id]`으로 \[대괄호\]를 사용하여 폴더 생성
	3. 그 안에 `page.js`와 `layout.js`를 생성한다.
```TSX
type props = { params:{ id:string }; searchParams:{ }; }  
type board = { title:string; content:string; }
  
export default async function Detail(props:props) {  
    const client = await connectDB;  
    const db = client.db("forum"); 
    let filter = {_id: new ObjectId(props.params.id)};  
	let result:WithId<board>|null 
		= await db.collection("post").findOne(filter) as WithId<board>;
    
    if(result != null){  
       return (  
          <div>  
             <h4>상세페이지</h4>  
             <h4>{result.title}</h4>  
             <p>{result.content}</p>  
          </div>  
       )  
    } else {  
       return <div>잘못된 요청입니다.</div>  
    }  
}  
```
	 props의 값은 { params: { id: '65a2886cda5f299f041b6e2b' }, searchParams: {} }
	 이렇게 되는데, params object 데이터 안의 id는 우리가 [id]라는 폴더를 사용했기 때문이다.
##### useRouter
- 규칙
	1. `'use client'`코드를 상단에 기술(`use~~ 문법은 client component에서만 작동한다.`)
```TSX
import {useParams, usePathname, useRouter, useSearchParams} from "next/navigation";  
export default function DetailLink(pr:{p:string}) {  
    let router = useRouter()  
    let a = usePathname();  
    let b = useSearchParams();  
    let c = useParams();  
    console.log(a);  
    console.log(b);  
    console.log(c);  
  
    return (  
       <div>  
          <button onClick={() => {router.push('/detail/'+pr.p)}}>링크</button>  
          <button onClick={() => {router.back()}}>뒤로가기</button>  
          <button onClick={() => {router.forward()}}>앞으로가기</button>  
          <button onClick={() => {router.refresh()}}>새로고침</button>  
          <button onClick={() => {router.prefetch(`/detail/${pr.p}`)}}>로드</button>
       </div>  
    )}
```
1. `router.push('/detail/'+pr.p)` : 괄호 안의 경로로 이동
2. `router.back()` : 뒤로가기
3. `router.forward()` : 앞으로 가기
4. `router.refresh()` : 이전 내용과 비교하여 바뀐부분만 새로고침
5. `router.prefetch('/detail/'+pr.p)` : 괄호안의 내용을 미리 로드
	- `<Link href={'/detail/'+pr.p)}>링크</Link>` 또한 미리 로드를 함
	- prefetch={false} 속성을 통해 미리로드 방지도 가능함
---
#### [[TypeScript/Next/Parts/Component|Component]]
> Next.js에는 2가지의 컴포넌트 방식이 존재
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

> 하나의 파일에서 다수의 파일을 export 할때
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
---
#### [[NextServer|Server]]
##### 3 Tier Architecture
![[Next/Datas/3TierArchitecture.png]]
- Client Layer <-> Server Layer <-> DataBase Layer
	- 사용자의 입력을 전적으로 신뢰해선 안됨
	- 사용자가 DB 입출력을 필요로 할 시 ==DB 입출력을 안전하게 대리로 해줄 수 있는== 프로그램이 필요하며 이를 ==Server==라고 한다.
- 규칙
	1. `프로젝트 루트`로부터 `pages/api` 폴더를 생성한다.
		- `src/app` 폴더 아래 `api`폴더를 만들어 작업하는 방법도 있는듯 싶으나 나사가 좀 빠진듯함
	2. 필요한 메서드를 작성한다.
```TypeScript
export default async function handler(req:NextApiRequest, res:NextApiResponse) {  
    const client = await connectDB;  
    const db = client.db("forum");  
    let result = await db.collection("post").find().toArray();  
	if(req.method =="POST"){  
	    console.log('POST 요청');  
	} else if(req.method =="GET"){  
	    console.log('GET 요청');  
	}
    console.log(req.body);
	  
    return res.status(200).json(result);  
}
```
	 '/pages/api/test.ts'

```TSX
export default function Write() {   
    return (  
       <div className="p-20">  
          <h4>글 작성</h4>  
          <form action="/api/test" method="POST">  
             <input name="title" placeholder="글제목"/>  
             <input name="content" placeholder="글내용"/>  
             <button type="submit">전송</button>  
          </form>  
       </div>  
    )}
```
	 /src/app/write/page.tsx
---
#### use@@@
##### usePathname()
> 현재 URL을 출력
##### useSearchParams()
> search parameter(query string)을 출력
##### useParams()
> Path Variable\[dynamic route]을 출력