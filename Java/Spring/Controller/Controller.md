- 규칙
	1. 상단에 `@Controller` 어노테이션을 부여한다.
	2. 경로를 Mapping한다
		1. `@RequestMapping(value = "/주소")`
			 - `method = RequsetMethod.GET`나 ~`.POST`, ~`.PUT`, ~`.DELETE`등 전송방식 지정
			 - `params = "#"`또한 지정 가능
		2. `@GetMapping("/주소")`
		3. `@PostMapping("/주소")`
	3. 내부 동작을 처리할 메서드와 필요한 데이터를 매개변수로 담는다.
		- ==Params== 참고 
```Java
@Controller  
public class ExampleController {
	// 메서드는 Params 탭에 기술함
}
```

##### Params
```HTML
<form action="/homeAction" method="get">  
    <label> 이름 : <input type="text" name="name"></label>  
    <label> 나이 : <input type="text" name="age"></label>
    <input type="submit" value="버튼A" name="a">  
    <input type="submit" value="버튼B" name="b">  
</form>
```
###### Request 활용
```Java
@RequestMapping(value = "/homeAction", method = RequestMethod.GET)  
public String homeAction(HttpServletRequest req){  
	String name = req.getParameter("name");  
	String age = req.getParameter("age");  
	String ip = req.getRemoteHost();  
	System.out.println(ip);  
	System.out.println("Get : 이름 : "+ name + ", 나이 : " + age);  
	return "redirect:/";  
}  
```
###### Parameter 활용
```Java
@RequestMapping(value = "/homeAction", method = RequestMethod.GET)  
public String homeAction(String name, int age){        
	System.out.println("Get : 이름 : "+ name + ", 나이 : " + age);        
	return "redirect:/";    
}
```
###### Vo 객체 활용
```Java
@RequestMapping(value = "/homeAction", method = RequestMethod.GET)    
public String homeAction(HomeVO vo){        
	System.out.println("Post : 이름 : "+vo.getName()+ ", 나이 : "+vo.getAge());  
	return "redirect:/";    
}
```
###### RequestParam 활용
```Java
@GetMapping("/homeAction")    
public String homeAction(
	@RequestParam(value = "name", required = false) String name, int age
){
	if(name==null){            
		name = "";        
	}
	System.out.println("이름 : " + name + ", 나이 : " + age);  
	return "redirect:/";    
}
```
###### Submit Event Name속성 활용
```Java
@GetMapping(value = "/homeAction", params = "a")  
public String submitA(HomeVO vo){  
    System.out.println("A");  
    System.out.println(vo.toString());  
    return "redirect:/homepage";  
}
@RequestMapping(value = "/homeAction", params = "b", method = RequestMethod.GET)
public String submitB(HomeVO vo){  
    System.out.println("B");  
    System.out.println(vo.toString());  
    return "redirect:/homepage";  
}
```
	 mapping 어노테이션의 params 값과 submit 이벤트의 name속성 값이 일치하는 경로가 연결된다.
###### PathVariable 활용
```Java
@GetMapping("/example/{num}")
public String exampleTest(@PathVariable int num){  
	System.out.println(num);  
	return "redirect:/";  
}  
```