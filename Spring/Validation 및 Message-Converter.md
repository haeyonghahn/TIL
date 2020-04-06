# Validation
validation이란 어떤 데이터의 값이 유효한지, 타당한지 확인하는 것을 의미한다.
예를 들어, 이메일 주소 양식은 admin@example.com인데 회원 가입을 할 때
이메일 양식이 일치하지 않으면 유효하지 않은 이메일이므로 회원 가입을 막을 수 있다.   

UI에서 javascript로 "이메일 양식이 일치하지 않는다"는 것은 UX 측면에서 사용자에게 편의를 주기 위함이다.
사용자가 잘못 입력하여 오타가 발생 했으니 다시 한 번 확인을 하라는 의미가 강하다.
즉 UI단에서 유효성 검사를 하는 것은 보안적인 측면에서 아무 효과가 없다는 것을 말할 수 있다.   

보안적인 측면에서 유효성 검사란 올바르지 않은 데이터가 서버로 전송되거나, DB에 저장되지 않도록 하는 것이다.   

여기서는 Hibernate에서 제공하는 어노테이션을 사용할 것이다.
* @NotEmpty : Empty값이 아닌가 ?   
* @Email : 이메일 형식   
* @URL : URL 형식
* @Length(min=, max=) : 문자열 길이 min과 max 사이인가 ?   
* @Range(min=, max=) : 숫자 범위 체크   
## 환경 설정 : pom.xml
```xml
<!-- validation -->
		<dependency>
			<groupId>javax.validation</groupId>
			<artifactId>validation-api</artifactId>
			<version>1.0.0.GA</version>
		</dependency>

		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-validator</artifactId>
			<version>4.2.0.Final</version>
		</dependency>
	</dependencies>
```
## Controller, vo 작성   
여기서는 회원 가입을 할 때 유효성을 검사를 확인할 것이다.
```java
@RequestMapping(value = "/join", method = RequestMethod.POST)
	public String join(@ModelAttribute @Valid UserVo vo, BindingResult result, Model model) {
		if(result.hasErrors()) {
			List<ObjectError> list = result.getAllErrors();
			for(ObjectError error : list) {
				System.out.println(error);
			}
			model.addAllAttributes(result.getModel());
			return "user/join";
		}
		userService.join(vo);
		return "redirect:/user/joinsuccess";
	}
```
파라미터에서 유효성 검사가 필요한 객체에 대해 @Valid 어노테이션을 추가한다.
그리고 BindingResult 객체는 검증 결과에 대한 결과 정보들을 담고 있다. 검증 결과 정보, 즉 BindingResult는 DispatcherServlet이 JSP에 넣어준다. 
즉 컨트롤러에서 뷰 이름을 반환하면 에러 내용을 바인딩해서 JSP에 넘겨줄 테니, 값을 사용자에게 보여주라는 의미의 forwarding 개념이 깔려있다.
파라미터에서 @Valid 어노테이션을 붙이는 유효성 검사에 대한 선언은 컨트롤러에서 하고 유효성 검사를 체크하는 그 근거는 UserVO 클래스에 작성한다.   

## vo 작성
```java
package com.douzone.mysite.vo;

import org.hibernate.validator.constraints.Email;
import org.hibernate.validator.constraints.Length;
import org.hibernate.validator.constraints.NotEmpty;

public class UserVo {
	private Long no;
	
	@NotEmpty
	@Length(min=2, max=8)
	private String name;
	
	@NotEmpty
	@Email
	private String email;
	
	@NotEmpty
	@Length(min=2, max=8)
	private String password;
	
	private String gender;
	private String joinDate;
	private String role;
	 
	public Long getNo() {
		return no;
	}
	public void setNo(Long no) {
		this.no = no;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	public String getGender() {
		return gender;
	}
	public void setGender(String gender) {
		this.gender = gender;
	}
	public String getJoinDate() {
		return joinDate;
	}
	public void setJoinDate(String joinDate) {
		this.joinDate = joinDate;
	}
	public String getRole() {
		return role;
	}
	public void setRole(String role) {
		this.role = role;
	}
	
	@Override
	public String toString() {
		return "UserVo [no=" + no + ", name=" + name + ", email=" + email + ", password=" + password + ", gender="
				+ gender + ", joinDate=" + joinDate + ", role=" + role + "]";
	}
}
```
## jsp에 메시지 출력
컨트롤러에서 Validation을 선언하고, VO 객체에서 검증 조건을 작성하면 유효한 데이터만 받을 수 있다.
그런데 검증 실패 원인에 대해서 콘솔로 에러메시지를 출력할 뿐만 아니라, 사용자가 알 수 있도록 JSP에 에러 메시지를 출력하면 좋다 좋다.   
```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn" %>
<%@ taglib uri="http://www.springframework.org/tags" prefix="spring" %>

이름 :  <input name="name" type="text" value="">
        <spring:hasBindErrors name="userVO">
            <c:if test="${errors.hasFieldErrors('name') }">                                     
               <strong>${errors.getFieldError( 'name' ).defaultMessage }</strong>
            </c:if>
        </spring:hasBindErrors>
```   
이름의 유효성 검사가 실패했을 경우 에러 메시지를 출력하도록 작성했다.
서버에서 전달해준 BindingResult 객체를 사용하기 위해서는 태그 라이브러리를 추가해야 한다.
```jsp
<%@ taglib uri="http://www.springframework.org/tags" prefix="spring" %>
```
<spring:hasBindErrors name="userVO">에서 userVO는 UserVO 객체의 이름이다.
UserVO의 앞 글자 U를 소문자 u로 바꿔야 하는데, 그 이유는 잘 모르겠지만 userVO가 변수로서 사용되니 관례로서 바꿔주는 것 같다.
defaultMessage는 유효성 검사가 실패했을 경우, 검증 조건을 알려주는 기본 메시지이다.
테스트를 해보면 메시지가 영어이다. 그래서 메시지를 커스터마이징하겠다.

# 메시지 커스터마이징 (Message-Converter)
### 환경 설정 : spring-servlet.xml
```xml
<!-- MessageSource -->
	<bean id="messageSource"
		class="org.springframework.context.support.ResourceBundleMessageSource">
		<property name="basenames">
			<list>
				<value>messages/messages_ko</value>
			</list>
		</property>
	</bean>
```
메시지를 커스터마이징 하기 위해서는 위와 같이 bean을 생성해야 한다.
한국어로 바꿀 것이므로 messages/messages_ko를 value로 작성한다.   

이제 메시지를 어떤 내용으로 바꿀 것인지에 대한 설정 파일을 생성해야 한다.
/src/main/resources/messages 폴더를 생성한 후 messages_ko.properties 이름으로 file을 생성한다.
spring-servlet.xml 파일에서 그렇게 정의를 했으므로 꼭 폴더 이름과 파일 이름을 위와 같이 작성해야 한다.  
### message_ko.properties
```
NotEmpty.userVo.name=\uC774\uB984\uC774 \uBE44\uC5B4\uC788\uC2B5\uB2C8\uB2E4.
Length.userVo.name=\uC774\uB984\uC740 2~8\uC790 \uC0AC\uC774\uC5EC\uC57C \uD569\uB2C8\uB2E4.

NotEmpty.userVo.email=\uC774\uBA54\uC77C\uC774 \uBE44\uC5B4\uC788\uC2B5\uB2C8\uB2E4.
Email.userVo.email=\uC774\uBA54\uC77C \uD615\uC2DD\uC774 \uC544\uB2D9\uB2C8\uB2E4.

NotEmpty.userVo.password=\uBE44\uBC00\uBC88\uD638\uAC00 \uBE44\uC5B4\uC788\uC2B5\uB2C8\uB2E4.
Length.userVo.password=\uBE44\uBC00\uBC88\uD638\uB294 2~8\uC790 \uC0AC\uC774\uC5B4\uC57C \uD569\uB2C8\uB2E4.
```   
NotEmpty라는 검증 조건.검증 조건이 있는 객체 이름.변수명"의 양식으로 출력될 에러 메시지를 입력하면 위와 같이 UTF-8
로 인코딩 되어 메시지를 등록할 수 있다.   
* 예시 : NotEmpty.userVo.name=이름이 비어있습니다.   

이렇게 각 변수의 검증 조건에 대하여 메시지를 커스터마이징한 후 검증에 실패하면 기본 메시지로 위에서 작성한 메시지들이 출력된다.

### 문제점
하나의 입력 값에 대해서 유효성 검증에 실패하면 모든 입력 값이 날아간다. 그래서 아래 이미지처럼 입력 값을 유지할 수 있는 방법이 필요하다.   
![form 유지](https://github.com/haeyonghahn/TIL/blob/master/Spring/images/form%20%EC%9C%A0%EC%A7%80.PNG)   
## form 양식 유지   
```jsp
<%@ taglib uri="http://www.springframework.org/tags/form" prefix="form" %>

<form:form modelAttribute="userVo" id="join-form" name="joinForm" method="post" action="${pageContext.request.contextPath }/user/join">
		<label class="block-label" for="name">이름</label>
		<form:input path="name"/>
		<p style="font-weight:bold; color:#f00; text-align:left; padding-left:0">
		<spring:hasBindErrors name="userVo">
			<c:if test='${errors.hasFieldErrors("name") }'>
				<spring:message code='${errors.getFieldError("name").codes[0] }'></spring:message>
			</c:if>
		</spring:hasBindErrors>
		</p>
					
		<label class="block-label" for="email">이메일</label>
		<form:input path="email"/>
		<input type="button" value="id 중복체크">
		<p style="font-weight:bold; color:#f00; text-align:left; padding-left:0">
			<form:errors path="email" />
		</p>
					
		<label class="block-label">패스워드</label>
		<form:password path="password"/>
		<p style="font-weight:bold; color:#f00; text-align:left; padding-left:0">
			<form:errors path="password" />
		</p>
					
		<fieldset>
			<legend>성별</legend>
			<label>여</label> <input type="radio" name="gender" value="female" checked="checked">
			<label>남</label> <input type="radio" name="gender" value="male">
		</fieldset>
					
		<fieldset>
			<legend>약관동의</legend>
			<input id="agree-prov" type="checkbox" name="agreeProv" value="y">
			<label>서비스 약관에 동의합니다.</label>
		</fieldset>
					
		<input type="submit" value="가입하기">					
	</form:form>
```   
위의 코드를 정리하자면,
* form 태그 라이브러리를 추가하고 기존의 <form> 태그를 위와 같이 <form:form> 태그로 바꿔준다. form 태그 라이브러리는 태그 같지만 실제로는 java 코드가 돌고 있다. 태그가 아니다.   
* modelAttribute 속성에는 유효성 검증을 하고자 하는 객체를 작성하면 된다. 마찬가지로 실제로는 UserVO지만, userVO를 작성한다.   

또한,   

이때 form.jsp 페이지를 반환하는 Controller에서 UserVo 객체가 없다면 에러가 발생하기 때문에 GET방식 Controller에 UserVo 객체를 추가해야 한다.
```java
@RequestMapping(value = "/join", method = RequestMethod.GET)
	public String join(@ModelAttribute UserVo vo) {
		return "user/join";
	}
```
처음 "회원가입" 버튼을 클릭했을 때 form.jsp 페이지의 <form: 태그의 @ModelAttribute에는 userVO가 없지만, 사용자가 회원 가입을 할 때 유효성 검사에 실패한다면, 컨트롤러로부터 userVO가 담겨서 오므로, 사용자가 입력했던 값이 유지된다.   
default message를 출력하는 spring-message 태그를 사용했을 때 보다 form 태그를 사용하니까 에러 메시지를 출력하기 위한 코드가 상당히 짧아졌고, 가독성도 좋아진다.   

정리하자면,
1) @Valid 어노테이션과 검증 조건을 추가하는 방법
2) 메시지를 커스터마이징하는 방법
3) form 태그 라이브러리로 입력 값을 유지하면서 가독성을 좋게 하는 방법

