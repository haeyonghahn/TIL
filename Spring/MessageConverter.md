# MessageConverter
HTTP 요청을 모델에 바인딩하고 클라이언트에 보낼 HTTP 응답을 만들기 위해 뷰를 사용했던 방식과는 달리
HTTP 요청 본문과 HTTP 응답 본문을 통째로 메세지로 다루는 방식이다. 주로 xml이나 json을 이용한 ajax 기능이나 웹 서비스를 개발할 때 사용된다.
```java
@ResponseBody // 응답
@RequestMapping(value= "/hello", method=RequestMethod.POST)
public String hello(@RequestBody String param){ // 요청
    return "result";
}
```
위와 같은 애노테이션을 명시해두게 되면 스프링은 메세지 컨버터라는 것을 사용하여 HTTP 요청이나 응답을 메세지로 변환하게 된다.
즉 위처럼 파라미터 부분에 @RequestBody를 입력할 경우, 파라미터 타입에 맞는 메세지 컨버터를 선택한 뒤 HTTP 요청 본문을 통째로 메세지로 변환하여 파라미터에 바인딩하는 것이다.
메서드의 상단에 @ResponseBody를 입력할 경우 또한 마찬가지로 리턴 타입에 맞는 메세지 컨버터를 선택한 뒤 리턴 값을 통째로 메세지로 변환한 뒤 리턴해주는 것이다.

참고로 GET 방식의 요청일 경우 HTTP 요청 본문이 없으므로 @RequestBody를 사용할 수 없다. @RequestParam이나 @ModelAttribute를 사용해야 한다.

## MessageConverter 종류
메세지 컨버터는 AnnotationMethodHandlerAdapter를 통해 등록할 수 있고, 디폴트로 4가지의 메세지 컨버터가 등록되어 있다.

### 1) ByteArrayHttpMessageConverter
지원하는 오브젝트 타입은 byte[]이고, 미디어타입은 모든 것을 다 지원한다.
즉 파라미터에 @RequestBody byte[] param과 같이 작성하면 모든 요청을 다 byte배열로 받을 수 있다.
그리고 리턴타입을 byte[]로 했을 경우 Content-Type이 applcation/octet-stream으로 설정되어 전달된다.

### 2) StringHttpMessageConverter
지원하는 오브젝트 타입은 String이고, 미디어타입은 모든 것을 다 지원한다. 파라미터에 사용할 경우 HTTP 본문을 그대로 String으로 가져올 수 있게되고,
리턴에 사용할 경우 단순 문자열을 그대로 전달해줄 수 있다. Content-Type은 text/plain으로 전달된다.

### 3) FormHttpMessageConverter
지원하는 오브젝트 타입은 MultiValueMap<String, String>이고, 미디어타입은 application/x-www-form-urlencoded만 지원한다.
즉 정의된 폼 데이터를 주고받을 때 사용할 수 있는데, 폼 데이터는 @ModelAttribute를 사용하는 것이 훨씬 유용하므로 이것 또한 자주 사용할 일은 없다.

### 4) SourceHttpMessageConvreter
지원하는 오브젝트 타입은 DomSource, SAXSource, StreamSource이고, 미디어타입은 application/xml, application/*+xml, text/xml 세가지를 지원한다.
XML 문서를 Source 타입의 오브젝트로 변환하고 싶을 떄 사용할 수 있다. 하지만 요즘은 OXM 기술이 많이 발달되었으므로 이 또한 잘 쓰이지 않는다.


* 이 밖에도 디폴트가 아닌 메시지 컨버터들이 있다. 아래 컨버트를 확인해 보자.
### Jaxb2RootElementHttpMessageConverter
JAXB의 @XmlRootElement와 @XmlType이 붙은 클래스를 이용해 XML과 오브젝트 사이의 메세지 변환을 지원한다.
지원하는 미디어 타입은 SourceHttpMessageConvreter와 동일하다.

### MashallingHttpMessageConverter
스프링 OXM 추상화의 Mashaller와 Unmarshaller를 이용해서 XML과 오브젝트 사이의 변환을 지원한다. 이 컨버터를 등록할 때는 marshaller와 unmarshaller를 설정해줘야 한다.
지원하는 미디어 타입은 SourceHttpMessageConvreter와 동일하다.

### MappingJacksonHttpMessageConverter
Jackson의 ObjectMapper를 이용해서 JSON과 오브젝트 사이의 변환을 지원한다.
지원하는 미디어 타입은 application/json이다.
변환하는 오브젝트 타입의 제한은 없지만 프로퍼티를 가진 자바빈 스타일이나 HashMap을 이용해야 정확한 변환 결과를 얻을 수 있다.


* 이번 예제에서는 http 본문에서 string으로 가져올 수 있도록 해보고 jackson 라이브러리를 사용하여 Map으로 가져올 수 있도록 해볼 것이다.
## 환경 설정 : pom.xml
```xml
<!-- jackson -->
		<dependency>
		    <groupId>com.fasterxml.jackson.core</groupId>
		    <artifactId>jackson-databind</artifactId>
		    <version>2.9.8</version>
		</dependency>
```

## 환경 설정 : spring-servlet.xml
StringHttpMessageConverter와 MappingJackson2HttpMessageConverter 클래스를 사용할 것이다.
```xml
<!-- validator, conversionService, messageConverter를 자동으로 등록 -->
	<mvc:annotation-driven>
		<mvc:message-converters>
			<bean
				class="org.springframework.http.converter.StringHttpMessageConverter">
				<property name="supportedMediaTypes">
					<list>
						<value>text/html; charset=UTF-8</value>
					</list>
				</property>
			</bean>
			<bean
				class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
				<property name="supportedMediaTypes">
					<list>
						<value>application/json; charset=UTF-8</value>
					</list>
				</property>
			</bean>
		</mvc:message-converters>
	</mvc:annotation-driven>
```
### View
```JavaScript
<script type="text/javascript">
$(function(){
	$("#btn-checkemail").click(function(){
		var email = $("#email").val();
		if(email == ''){
			return;
		}
		$.ajax({
			url: '${pageContext.request.contextPath }/api/user/checkemail?email=' + email,  // 클라이언트가 요청을 보낼 서버의 url 주소
			type: 'get',  // HTTP 요청 방식 (GET, POST)
			// contentType: 'application/json'
			data:'',  // HTTP 요청과 함께 서버로 보낼 데이터
			dataType: 'json', // 서버에서 보낼줄 데이터의 타입
			success: function(response){
				if(response.result == 'exist'){
					alert('존재하는 이메일입니다.');
					$("#email")
						.val('')
						.focus();
					return;
				}
				
				$('#btn-checkemail').hide();
				$('#img-checkemail').show();			
			},
			error: function(XHR, status, e){
				console.error(status + ":" + e);
			}
		});
	});
});
</script>
```
위의 JavaScript의 코드의 내용을 참고하려면 JavaScript/jQuery/ajax의 내용을 참고하라.

### Controller
컨트롤러 패키지가 존재하지만, 별도로 api 통신을 위한 패키지를 하나 만들어 그 안에 사용자 이메일이 존재하는지 중복 체크 여부를 판단할 수 있는
컨트롤러를 서버 컨트롤러와 클래스명이 같은 컨트롤러를 하나 생성할 것이다.
```java
@Controller("UserApiController")
@RequestMapping("/api/user")
public class UserController {

	@Autowired
	private UserService userService;
	
	@ResponseBody
	@RequestMapping("/checkemail")
	public Map<String, Object> checkEmail(
		@RequestParam(value="email", required=true, defaultValue="") String email) {
		boolean exist = userService.existUser(email);
		
		Map<String, Object> map = new HashMap<>();
		map.put("result", exist ? "exist" : "not exist");
		return map;
	}
}
```
UserController는 이미 존재하므로 오류가 나지 않도록 Controller명을 지정해 주었다. 또한, json 형식으로 응답을 받아 이메일이 존재 여부를
체크할 수 있도록 작성했다.



