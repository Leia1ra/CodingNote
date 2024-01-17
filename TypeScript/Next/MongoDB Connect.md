1. MongoDB 라이브러리를 설치한다
	`npm install mongodb`

2. MongoDB를 연결하는 파일을 만든다.
```TypeScript
import {MongoClient} from "mongodb";  
  
const url = 'mongodb+srv://pofol:cVyCxuaB2onxekMO@portfolio.cdrpbfl.mongodb.net/?retryWrites=true&w=majority';  
const options = {useNewUrlParser:true}  
let connectDB:Promise<MongoClient>;  
  
if(process.env.NODE_ENV === 'development'){  
    if(!global._mongo){  
       global._mongo = new MongoClient(url).connect();  
    }  
    connectDB = global._mongo;  
} else {  
    connectDB = new MongoClient(url).connect();  
}  
export {connectDB}
```
	 next.js의 경우 개발시 파일저장시 마다 자바스크립트 파일들이 실행되기 때문에 
	 MongoDB Client connect가 동시에 실행될 수 있어, 입출력이 매우 느려지기 때문에 미연에 방지하는 코드

3. Server Component에서만 사용하도록 한다.
```TSX
import { connectDB } from "/util/database.js" 
async function Home() { 
	let client = await connectDB; 
	const db = client.db('forum'); 
	let result = await db.collection('post').find().toArray(); 
	
	return ( 
		<main> {result[0].title} </main> 
	)
}
export default Home;
```
	 client Component에서 사용시 일반 유저도 쉽게 볼 수 있는 문제가 있음
---
### CRUD
#### Create

---
#### Read
##### find
> 전체 정보를 찾을때
```TypeScript
let result = await db.collection('post').find().toArray();
```

##### findOne
> 특정한 하나의 정보를 찾을때 사용
```TypeScript
let filter = {_id: new ObjectId(props.params.id)};
let result = await db.collection("post").findOne(filter);
```
	let result:WithId<board>|null 
		= await db.collection("post").findOne(filter) as WithId<board>;
	 구체적인 타입을 작성하고 싶다면 위와같은 예시를 참고할 것
---
#### Update

---
#### Delete

---