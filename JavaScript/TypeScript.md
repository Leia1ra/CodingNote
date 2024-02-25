### 타입 정의
##### 타입 에너테이션(Type Annotation)
>TypeScript 에서 변수나 인수 뒤에 : 타입과 같이 타입 어노테이션(Type Annotation)이라 불리는 정보를 붙여 변수나 인수에 저장할 값을 제한 가능  
```TypeScript
/*
var 변수 : 타입 = 값  
let 변수 : 타입 = 값  
const 변수 : 타입 = 값
*/
function sayHello(firstName : string) : void {  
    console.log(`Hello ${firstName}`);  
}  
function sayHello2(firstName) {  
    console.log(`Hello ${firstName}`);  
}
let firstName1 :string = 'LEE';  
let firstName2 = 'KIM';  // 변수 데이터 타입이 명확한 경우 등 일부 조건에서는 생략할 수도 있음
sayHello(firstName1);  
sayHello(firstName2);
```
---
##### var, let, const의 차이
> - var로 정의된 경우, 변수의 스코프가 해당 변수를 포함하는 함수에서 까지 사용 가능
> - let으로 정의된 경우, 블록 스코프로 선언된 변수는 해당변수를 포함하는 블록 안에서만 사용 가능
> - const의 경우 let과 같은 스코프 규칙을 갖지만, 값을 재대입하면 에러 발생
```TypeScript
function scopeTest(scope : boolean) : void {  
    var x: number = 10;  
    let y = 10;  
    if(scope){  
        // var로 정의된 경우, 변수의 스코프가 해당 변수를 포함하는 함수에서 까지 사용가능
        var x: number = 20;  
		  
        // let으로 정의된 경우, 블록 스코프로 선언된 변수는 해당변수를 포함하는 블록 안에서만 사용 가능 
        let y : number = 20;  
		  
        // const의 경우 let과 같은 스코프 규칙을 갖지만, 값을 재대입하면 에러 발생  
        const num : number = 100;  
		  
        console.log(`x -> ${x} `); // 20  
        console.log(`y -> ${y} `); // 20  
    }  
    console.log(`x -> ${x} `); // 20  
    console.log(`y -> ${y} `); // 10  
}  
scopeTest(true);
```
---
##### 원시 타입
> 자바스크립트에서 주로 사용되는 원시타입인 number(숫자), boolean(논리), string(문자열)이 있음
```TypeScript
// number
let age : number = 36;  
let age2 = 36;  
// boolean
let isDone : boolean = false;  
let isDone2 = false;  
// string
let color : string = '파랑';  
let color2 = '파랑';
```
	다른 타입의 값을 넣으려고 할 시 에러 발생
---
##### 배열
```TypeScript
/* 배열 */
const array: string[] = ['a', 'b', 'c']  
array.push('abc');  
  
let a1 : Array<string> = ['a', 'b', 'c'];  
let a2 : Array<string> = new Array('a', 'b', 'c');  
  
const union : (string | number)[] = ['foo', 1, 'a', 2, 'b', 'c'] // union 타입  
const tuple : [string, number, boolean] = ['foo', 1, true]; // 튜플
```
---
##### 객체 타입
> 객체(object)는 key, value를 이용한 데이터 형식 인스턴스
```TypeScript
const user : {name : string, age : number} = {  
    name : 'Hana',  
    age : 36  
}  
  
function printName(obj : {firstName:string, lastName ?: string}) {  
    // 객체타입은 일부 또는 모든 속성을 '?:'를 사용해 옵셔널(Optional, 선택 가능) 속성으로 지정가능  
    // 옵셔널 속성으로 타입을 정의할 시, 해당 속성이 존재하지 않아도 문제없이 작동함  
}  
printName({firstName:'Hana'});  
printName({firstName:'Hana', lastName:'LEE'});
```
---
##### any
> 모든 타입을 허용하는 타입 -> 특정한 값의 타입 체크 구조를 적용하고 싶지 않을때 사용
```TypeScript
let anyUser :any = {firstName : 'Hana'};  
// 다음 행의 코드는 모두 컴파일러 에러가 발생하지 않음  
anyUser.hello();  
anyUser();  
anyUser.age=100;  
anyUser='hello';  
const n:number = anyUser;
```
	근데 이럴거면 typeScript왜써
---
##### 함수
> 함수 선언 방법
```TypeScript
function methodTest1(b:string) { return b; }  
console.log(methodTest1('methodTest1 : 명명 함수 선언')); /* 1. 명명 함수 선언*/  

let methodTest2:(a:string) => string = function (a:string) { return a; }  
console.log(methodTest2('methodTest2 : 익명 함수 표현')); /* 2. 익명 함수 표현 */

let methodTest3:(a:string) => string = function originalMethod(a:string){return a;} 
console.log(methodTest3('methodTest3 : 명명 함수 표현')); /* 3. 명명 함수 표현 */

let methodTest4:(a:string) => string = (b:string) => { return b; }  
console.log(methodTest4('methodTest4 : 화살표 함수')); /* 4. 화살표 함수 */

let methodTest5:(a:string) => string = (function (b:string) { return b; })  
console.log(methodTest5('methodTest5 : 즉시 실행 표현')); /* 5. 즉시 실행 표현 */

let methodTest6:Function = new Function('b', 'return b')  
console.log(methodTest6('methodTest6 : constructor')) /* 6. function constructor */
```
###### 함수
> 1. 인수를 정의할 때 기본값을 지정할 수 있음  
> 2. 옵셔널 인수를 지정할 수 있음  
> 3. 인수의 반환값의 타입에 다양한 타입을 지정할 수 있음
```TypeScript
function formatName(name : string):string{  
    return `${name}님`  
}  
function fn(
	defaultArgs : string = 'user',
	formatter : (name : string) => string,
	optional ?: string
) : string {  
    let str : string = `${formatter(defaultArgs)} ${optional}`;  
    console.log(str);  
    return str;  
}  
fn('hana', formatName);  
fn('hana', formatName, 'hello');
```
###### 화살표 함수(Arrow Function)의 경우
```TypeScript
// (인수명 : 인수_타입) : 반환값_타입 => { 식 }
let sayHi = (name : string) : string => { return `Hi ${name}`; }
sayHi('lee');
```
###### 함수형 타입
```TypeScript
function genBridInfo(name : string) : string[]{  
    return name.split(',');  
}  
// singBirds는 인수가 string이고 반환값이 배열인 함수를 인수로 받음
function singBirds(input : string, birdInfo : (x : string) => string[]):string {  
    return birdInfo(input)[0] + ' 꽥';  
}  
console.log(singBirds('오리, 비둘기', genBridInfo));
```
---
### 타입 기능
##### 타입 추론
> 명시적인 변수의 초기화를 수행하면 '타입 추론'을 통해 자동적으로 타입이 결정됨
```TypeScript
const age3 = 10;  
const user3 = {  
    name : 'Hana',  
    age : 36  
}
```
---
##### Type Assertion
> 타입 표명(Type Assertion)
- 타입스크립트의 '타입추론'을 통하여 구체적인 타입을 추론할 수 없는 경우 사용
	- ex) HTMLElement(div, canvas, input등 상세 타입을 추론할 수 없음), null 등등
```TypeScript
const myCanvas1 : HTMLElement = document.getElementById('main')  
// console.log(myCanvas1.width)  
// Error TS2339: Property 'width' does not exist on type 'HTMLElement'  
let c1 : HTMLElement = document.getElementById('main') as HTMLCanvasElement;  
//console.log(c1.width); // -> 얘도 에러(Ts도 업케스팅의 개념이 있는거 같음)  
let c2 : HTMLCanvasElement = document.getElementById('main') as HTMLCanvasElement;  
let c3 = document.getElementById('main') as HTMLCanvasElement;  
console.log(c2.width);  
console.log(c3.width);  
  
// 타입 캐스팅  
const casting:any = 1;  
const caster : number = casting as number
```
---
##### Type Alias
> 타입 별명(Type Alias)
- 인라인 타입지정시 동일한 타입을 반복적으로 정의하기에는 코드의 기술이 복잡해지는 문제 발생
- 타입 앨리어스는  통하여 동일한 타입을 간략하게 재사용할 수 있다
```TypeScript
// type 타입명 = 값
type Name = string  
function printPoint(point : Point) {  
    console.log(`x 좌표는 ${point.x}, y 좌표는 ${point.y}`);  
}
  
// method 1 : 복잡하고 긺  
let lineObject : {x:number,y:number} = {  
    x:200,  
    y:200,  
}; printPoint(lineObject);  
  
// method 2 : 타입에 이름을 붙여 재사용성을 높임  
type Point = {  
    x:number;  
    y:number;  
}  
// 짦고 간결함  
let line:Point = {  
    // 타입이 맞아도 속성명이 다르면 에러  
    x:100,  
    y:100,  
}; printPoint(line);
```

###### 함수타입 별명(Function Type Alias)
```TypeScript
function fnFormat(a:string):string { return `${a}씨`; }  
type Formatter = (a:string) => string // 이게 함수타입  
type fnType = (object:{fName:string, format:Formatter}) => void; // 이게 함수타입  
type fnInputs = {  
    fName:string;  
    format:Formatter;  
}  
  
let fnObject:fnInputs = { fName:'Hana', format:fnFormat }  
function fnType1(object:fnInputs):void { /* 1. 명명 함수 선언*/  
    console.log(object.format(object.fName));  
} fnType1(fnObject);  
  
let fnType2:fnType = function (object:fnInputs):void { /* 2. 익명 함수 표현 */
    console.log(object.format(object.fName));  
}; fnType2(fnObject);  
  
let fnType3:fnType = function orgFn(object:fnInputs):void { /* 3. 명명 함수 표현 */
    console.log(object.format(object.fName));  
}; fnType3(fnObject);  
  
let fnType4:fnType = (object:fnInputs):void => { /* 4. 화살표 함수 */
    console.log(object.format(object.fName));  
}; fnType4(fnObject);  
  
let fnType5:fnType = (function(object:fnInputs):void { /* 5. 즉시 실행 표현 */
    console.log(object.format(object.fName));  
}); fnType5(fnObject);
```
###### 인덱스 타입
-  객체의 키 이름을 명시하지 않고 Type Alias를 정의할 수 있음
	- {[] : 타입명}
```TypeScript
type Label = {  
    [key:string] : string  
}  
const labels:Label={  
    topTitle : '톱 페이지의 제목입니다',  
    topSupTitle : '톱 페이지의 하위 제목입니다',  
    topFeature1 : '톱페이지의 기능 1 입니다',  
    topFeature2 : '톱페이지의 기능 2 입니다',  
}
```
---
##### Interface
###### 인터페이스(interface)
- interface는 Type Alias와 비슷한 기능이지만 보다 확장성이 높은 열린 기능을 가짐
	- Type Alias : 객체의 타입 그 자체를 의미함
	- Interface : 매치하는 타입이라도 그 값 이외에 다른 필드나 메서드가 있음을 전제
```TypeScript
interface PointInterface {  
    x:number;  
    y?:number; // Optional속성, 생략가능
}  
function printPointInterface(point:PointInterface):void {  
    console.log(`x좌표는 ${point.x}, y좌표는 ${point.y}, z좌표는 ${point.z}`);  
}  
interface PointInterface {  
    z:number;  
}  
printPointInterface({x:100,z:100});
printPointInterface({x:100,y:100,z:100});
```
###### 인터페이스 확장
> 확장시 Optional속성의 인수는 생략하더라도 에러가 발생하지 않음
```TypeScript
interface Colorful {  
    color:string;  
}  
interface Circle {  
    radius:number;  
} 
type testInterfaceFn = ()=>void;  
interface ColorfulCircle extends Colorful, Circle{  
    gradation?:boolean;  
    test:testInterfaceFn; // test:Function // -> 이렇게 써도 됨
}  
  
const cc: ColorfulCircle = {  
    test(): void {  
        console.log("ㅎㅇ");  
    },  
    color:'Red',  
    radius:10  
    // gradation:true,  
};  
console.log(cc);  
cc.test();  
class MyCircle implements ColorfulCircle {  
    color: string;  
    radius: number;  
  
    test(){  
        console.log(`${this.radius} : ${this.color}`);  
    }  
    // gradation: boolean;  
}
let myCircleTest = new MyCircle();  
myCircleTest.color = 'red'; myCircleTest.radius = 5;  
myCircleTest.test();
```
---
##### Class
```TypeScript
class PointClass {  
    x:number;  
    y:number;  
  
    /*생성자*/  
    constructor(x:number=0, y:number=0){ // 인수가 없는 경우의 초기값 지정  
        this.x = x;  
        this.y = y;  
    }  
  
    moveX(n:number):void{  
        // 반환값이 없는 함수를 정의할 때는 void를 지정한다.  
        this.x += n;  
    }  
    moveY(n:number):void{  
        this.y += n;  
    }  
}  
const pClass = new PointClass();  
pClass.moveX(10);  
console.log(`x:${pClass.x} <-> y:${pClass.y}`);
```
###### 상속
> extends를 사용해 다른 클래스를 상속할 수 있다.
```TypeScript
class Point3DClass extends PointClass {  
    z:number;  
    constructor(x:number = 0, y:number = 0, z:number = 0) {  
        super(x, y);  
        this.z = z;  
    }  
    /*Overriding*/  
    moveX(n: number) {  
        super.moveX(n+10);  
    }  
    moveZ(n:number):void{  
        this.z += n;  
    }  
}
```

> Interface를 통하여 클래스에 대한 구현을 강제할 수 있습니다.
```TypeScript
interface IUser {  
    name:string,  
    age:number;  
    sayHello:() => string;  
}  
class User implements IUser{  
    age: number;  
    name: string;  
	  
    constructor() {  
        this.name = '';  
        this.age = 0;  
    }  
	  
    sayHello(): string {  
        return `안녕하세요. 저는 ${this.name}이며, ${this.age}살입니다.`;  
    }  
}
```

---
##### 접근제어자
> 접근 수정자(Access Modifier)
- 맴버나 메서드의 접근 범위를 제어할 수 있습니다.
	- public
	- protected
	- private
```TypeScript
class BasePoint3D{  
    public x : number;  
    protected y : number;  
    private z : number;  
}  
  
const basePoint = new BasePoint3D();  
basePoint.x  
// basePoint.y // 컴파일시 에러가 발생, Protected이므로 접근할 수 없다.  
// basePoint.z // 컴파일시 에러가 발생, Priavte는 접근할 수 없다  
  
// 클래스를 상속했을 때의 접근 제어 예  
class ChildPoint extends BasePoint3D{  
    constructor() {  
        super();  
        this.x  
        this.y  
        // this.z // 컴파일시 에러가 발생, Private는 접근할 수 없다.  
    }  
}
```
---
##### 열거형
> 열거형 (Enumeration)
```TypeScript
enum Direction {  
    Up,  
    Down,  
    Left,  
    Right  
}  
  
// enum Direction을 참조  
let direction: Direction = Direction.Left;  
console.log(direction);  
  
// enum을 대압한 변수에 다른 타입의 값을 대입하려고 하면 에러가 된다.  
enum DirectionStr {  
    Up = "UP",  
    Down = "DOWN",  
    Left = "LEFT",  
    Right = "RIGHT"  
}  
  
// API의 파라미터로 문자열이 전달된 경우, 문자열에서 Enum 타입으로 변환하는 방법  
const value = 'DOWN';  
const enumValue = value as DirectionStr;  
  
if(enumValue === DirectionStr.Down){  
    console.log("Down is Selected");  
}
```
---
##### 제너릭
> 제너릭(Generic)
- 클래스와 함수에 대해, 그 안에서 사용하는 타입을 추상화해 외부로부터 구체적인 타입을 지정할 수 있는 기능
```TypeScript  
class Queue<T>{  
    // 내부에서 T 타입의 배열을 초기화 한다.  
    private array:T[] = new Array<T>()  
	
    // T타입의 값을 배열에 추가한다.  
    push(item:T){  
        this.array.push(item);  
    }  
	  
    pop():T|undefined{  
        return this.array.shift();  
    }  
}  
const q = new Queue<number>();  
q.push(111)  
q.push(112)  
q.push(113)  
// q.push('foo'); // number Type이 아니므로 컴파일 시 에러가 발생  
// let str:string = q.pop() // str의 타입은 number 타입이 아니므로 컴파일시 에러가 된다.
```
---
##### Union&Intersection
```TypeScript
type Identity = {  
    id:number|string;  
    name:string;  
}  
type Contact = {  
    name:string;  
    email:string;  
    phone:string;  
}  
```
###### Union
> 유니온(Union)
- 합집합을 의미함
```TypeScript
type IdentityOrContact = Identity | Contact  
const id:IdentityOrContact = {  
    id:'111',  
    name:'Hana'  
}  
const contact:IdentityOrContact = {  
    id:'111',  
    name:'Hana',  
    email:'test@example.com',  
    phone:'0123456789'  
}
```
###### Intersection
> Intersection
- 교집합을 의미함
```TypeScript
type Employee = Identity&Contact  
const employee : Employee ={  
    id:'111',  
    name:'Hana',  
    email:'email',  
    phone:'0123456789'  
}  
/*
const empContact:Employee = { // Error : Contact 정보만으로 변수를 정의할 수 없음  
    name:'Hana',
	email:'email',
	phone:'0123456789'
}
*/
```
---
##### 리터럴 타입
> 리터럴(Literal) 타입
- 데이터를 구분하는 타입, 정해진 문자열이나 수치만을 대입할 수 있는 타입으로 제어
```TypeScript
let postStatus:'draft'|'published'|"deleted"  
postStatus = 'draft'  
// postStatus = 'inputs' // Error : 정의되지 않은 문자열 할당  
  
function compare(a:number, b:number): -1|0|1 {  
    return (a===b) ? 0 : (a>b) ? 1:-1;  
}
```
---
##### Never 타입
> Never 타입
































