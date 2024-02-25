> Asyncronize Javascript And Xml
> - 서버와 비동기적으로 데이터를 주고 받는 자바스크립트의 기술을 의미함
```JavaScript
var ajax = new XMLHttpRequest();
ajax.onreadystatechange = function(){
	if(this.readyState == 4 && this.status == 200){
		~~
	}
}
/* Get 방식으로 name 파라미터와 함께 요청 */ 
ajax.open('GET', '/getAgeByName?inputName=' + inputName); 
/* Response Type을 Json으로 사전 정의 */ 
ajax.responseType = "json"; 
/* 정의된 서버에 요청을 전송 */ 
ajax.send();
```
## CORS


***
## Axios
> 브라우저와 서버를 위한 Promise API를 활용하는 HTTP 비동기 통신 라이브러리
- 특징
	- 라이브러리의 설치가 필요
	- `XSRF(서버간 요청 위조)공격`을 보호
	- 브라우저를 위해 `XMLHttpRequest` 생성
	- `Promise API`를 지원
	- 자동으로 JSON 데이터 변환을 지원
	- `Requset`취소와 `Request TimeOut`설정 가능
	- `HTTP Requests`의 Intercept 가능
	- download 진행에 대해 기본적인 지원을 한다.

- 규칙
	- 라이브러리를 준비한다.
		- `npm install axios`를 터미널에 입력하여 라이브러리 설치를 진행한다.
		- [AXIOS][https://axios-http.com/kr/docs/intro] 홈페이지에서 CDN을 통해 사용한다.
	- 필요한 코드를 작성한다.
```TypeScript
try{  
    const response = await axios({  
       method:"delete",  
       headers:{'Content-Type':'application/json'},  
       url:'/api/post/delete',  
       data:{id:value._id,}  
    })  
    console.log(response)  
    return response;  
} catch (error){  
    console.log(error)  
    return error  
}
```

##### Options
| Option | info |
| ---- | ---- |
| ==method== | 요청방식. (get이 디폴트) |
| ==url== | 서버 주소 |
| baseURL | url을 상대경로로 쓸대 url 맨 앞에 붙는 주소.<br>    - url이 `/post` 이고 `baseURL`이 `https://some-domain.com/api/` 이면,  <br>        `https://some-domain.com/api/post`로 요청 가게 된다. |
| ==headers== | 요청 헤더 |
| ==data== | 요청 방식이 'PUT', 'POST', 'PATCH' 해당하는 경우 body에 보내는 데이터 |
| ==params== | URL 파라미터 ( ?key=value 로 요청하는 url get방식을 객체로 표현한 것) |
| timeout | 요청 timeout이 발동 되기 전 milliseconds의 시간을 요청. timeout 보다 요청이 길어진다면, 요청은 취소되게 된다. |
| responseType | 서버가 응답해주는 데이터의 타입 지정 (arraybuffer, documetn, json, text, stream, blob) |
| responseEncoding | 디코딩 응답에 사용하기 위한 인코딩 (utf-8) |
| transformRequest | 서버에 전달되기 전에 요청 데이터를 바꿀 수 있다.  <br>    - 요청 방식 `'PUT'`, `'POST'`, `'PATCH'`, `'DELETE'` 에 해당하는 경우에 이용 가능  <br>    - 배열의 마지막 함수는 string 또는 Buffer, 또는 ArrayBuffer를 반환합  <br>    - header 객체를 수정 가능 |
| transformResponse | 응답 데이터가 만들어지기 전에 변환 가능 |
| withCredentials | `cross-site access-control` 요청을 허용 유무. 이를 true로 하면 cross-origin으로 쿠키값을 전달 할 수 있다. |
| auth | HTTP의 기본 인증에 사용. auth를 통해서 HTTP의 기본 인증이 구성이 가능 |
| maxContentLength | http 응답 내용의 max 사이즈를 지정하도록 하는 옵션 |
| validateStatus | HTTP응답 상태 코드에 대해 promise의 반환 값이 resolve 또는 reject 할지 지정하도록 하는 옵션 |
| maxRedirects | node.js에서 사용되는 리다이렉트 최대치를 지정 |
| httpAgent /  httpsAgent | node.js에서 http나 https를 요청을 할때 사용자 정의 agent를 정의하는 옵션 |
| proxy | proxy서버의 hostname과 port를 정의하는 옵션 |
| cancelToken | 요청을 취소하는데 사용되어 지는 취소토큰을 명시 |

##### Example
```TypeScript
axios({/* axios 파라미터 문법 예시 */
    method: "get", // 통신 방식
    url: "www.naver.com", // 서버
    headers: {'X-Requested-With': 'XMLHttpRequest'}, // 요청 헤더 설정
    params: { api_key: "1234", langualge: "en" }, // ?파라미터를 전달
    responseType: 'json', // default
    
    maxContentLength: 2000, // http 응답 내용의 max 사이즈
    validateStatus: function (status) {
      return status >= 200 && status < 300; // default
    }, // HTTP응답 상태 코드에 대해 promise의 반환 값이 resolve 또는 reject 할지 지정
    proxy: {
      host: '127.0.0.1',
      port: 9000,
      auth: {
        username: 'mikeymike',
        password: 'rapunz3l'
      }
    }, // proxy서버의 hostname과 port를 정의
    maxRedirects: 5, // node.js에서 사용되는 리다이렉트 최대치를 지정
    httpsAgent: new https.Agent({ keepAlive: true }), 
    // node.js에서 https를 요청을 할때 사용자 정의 agent를 정의
}).then(function (response) {
	console.log(response.data)       // 서버가 제공한 응답(데이터)
	console.log(response.status)     // `status`는 서버 응답의 HTTP 상태 코드
	console.log(response.statusText) // `statusText`는 서버 응답으로 부터의 HTTP 상태 메시지
	console.log(response.headers)    // `headers` 서버가 응답 한 헤더는 모든 헤더 이름이 소문자로 제공
	console.log(response.config)     // `config`는 요청에 대해 `axios`에 설정된 구성(config)
});
```

***
## Fetch

