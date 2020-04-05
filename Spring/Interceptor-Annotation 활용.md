# Interceptor-Annotation

## 1. 접근 권한 - @Auth 어노테이션 생성
@Auth 어노테이션은 클라이언트가 로그인된 회원인지 아닌지, 그리고 일반 회원인지 관리자인지 구분할 용도로 만든 어노테이션이다.
```java
package com.douzone.security;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface Auth {
	public enum Role {USER, ADMIN}
	
	String value() default "USER";

	public Role role() default Role.USER;
	boolean test() default false;
}
```   
1. @Retention : 어떤 시점까지 어노테이션이 영향을 미칠 것인지 결정한다.   
2. @Target : 어노테이션이 적용할 위치를 결정한다.     
```java
@Retention(RetentionPolicy.RUNTIME) // 컴파일 이후에도 JVM에 의해서 참조가 가능합니다.   
// @Retention(RetentionPolicy.CLASS) // 컴파일러가 클래스를 참조할 때까지 유효합니다.   
// @Retention(RetentionPolicy.SOURCE) // 어노테이션 정보는 컴파일 이후 없어집니다.   
@Target({   
        ElementType.PACKAGE, // 패키지 선언시   
        ElementType.TYPE, // 타입 선언시   
        ElementType.CONSTRUCTOR, // 생성자 선언시   
        ElementType.FIELD, // 멤버 변수 선언시   
        ElementType.METHOD, // 메소드 선언시   
        ElementType.ANNOTATION_TYPE, // 어노테이션 타입 선언시   
        ElementType.LOCAL_VARIABLE, // 지역 변수 선언시   
        ElementType.PARAMETER, // 매개 변수 선언시   
        ElementType.TYPE_PARAMETER, // 매개 변수 타입 선언시   
        ElementType.TYPE_USE // 타입 사용시   
})   
public @interface MyAnnotation {   
    /* enum 타입을 선언할 수 있습니다. */   
    public enum Quality {BAD, GOOD, VERYGOOD}   
    /* String은 기본 자료형은 아니지만 사용 가능합니다. */   
    String value();   
    /* 배열 형태로도 사용할 수 있습니다. */   
    int[] values();   
    /* enum 형태를 사용하는 방법입니다. */   
    Quality quality() default Quality.GOOD;   
}   
```
접근 권한이 필요한 메소드 위에 @Auth 어노테이션을 작성하여 접근 권한이 있는 사용자인지 아닌지 판별할 수 있다. 예를 들어,   
어떤 경로에 비회원, 회원, 관리자가 접근할 수 있는 범위가 다르다면 @Auth 어노테이션을 컨트롤러의 메소드 위에 명시해서 접근 권한을 둘 수 있다.    
### AuthInterceptor 생성
```java
package com.douzone.security;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import org.springframework.web.method.HandlerMethod;
import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;

import com.douzone.mysite.vo.UserVo;

public class AuthInterceptor extends HandlerInterceptorAdapter {

	@Override
	public boolean preHandle(
		HttpServletRequest request,
		HttpServletResponse response,
		Object handler) throws Exception {
		
		//1. handler 종류 확인
    // 우리가 Handler할 메소드인지 아닌지 체크. false이면 우리가 Controller할 메소드가 아니다.
		if(handler instanceof HandlerMethod == false) {
			// DefaultServletHandler가 처리하는 경우(보통, assets의 정적 자원 접근)  
			return true;
		}
		
		//2. casting
		HandlerMethod handlerMethod = (HandlerMethod)handler;
		
		//3. Method의 @Auth 받아오기
		Auth auth = handlerMethod.getMethodAnnotation(Auth.class);
		
		//4. Method에 @Auth가 없으면 Type에 붙어 있는 지 확인한다(과제)
		if(auth == null) {
			auth = handlerMethod.getMethod().getDeclaringClass().getAnnotation(Auth.class);
		}
		
		//5. Type이나 Method 둘 다 @Auth가 적용이 안되어 있는 경우,
		if(auth == null) {
			return true;
		}
		
		//6. @Auth가 붙어 있기 때문 인증(Authentification) 여부 확인
		HttpSession session = request.getSession();
    if(session == null) {
			response.sendRedirect(request.getContextPath() + "/user/login");
			return false;
		}
		UserVo authUser = (UserVo)session.getAttribute("authUser");
		if(authUser == null) {
			response.sendRedirect(request.getContextPath() + "/user/login");
			return false;
		}
		
		//6. 권한(Authorization) 체크를 위해서 @Auth의 role 가져오기 ("USER", "ADMIN")
		String role = auth.value();
		System.out.println(role);
		System.out.println(authUser.getRole());

		//7. @Auth의 role이 "USER" 인 경우에는 authUser의 role이 "USER" 이든 "ADMIN" 상관이 없음.
		if("USER".equals(role)) { 
			return true;
		} 
		
		//8. @Auth의 role이 "ADMIN" 인 경우에는  반드시 authUser의 role이 "ADMIN" 여야 한다.
		if("ADMIN".equals(authUser.getRole()) == false) { 
			response.sendRedirect(request.getContextPath());
			return false;
		}
		
		// @Auth의 role => "ADMIN"
		// authUser의 role => "ADMIN"
		// 관리자 권한이 확인
		return true;
	}
}
```
return true이면 컨트롤러 요청이 진행되고, false이면 컨트롤러로 진행하지 않는다.
### 환경 설정 : spring-servlet.xml
```xml
  <mvc:interceptors>
    <mvc:interceptor>
			<mvc:mapping path="/**"/>
			<mvc:exclude-mapping path="/user/auth"/>
			<mvc:exclude-mapping path="/user/logout"/>
			<mvc:exclude-mapping path="/assets/**"/>
			<bean class="com.douzone.security.AuthInterceptor"></bean>
		</mvc:interceptor>
	</mvc:interceptors>
```   
로그아웃을 처리하는 Interceptor와 deaultServlet이 처리하는 /assets/** 경로를 제외한다.
### Controller 접근 권한 체크   
```java
@Auth
@RequestMapping(value = "/update", method = RequestMethod.GET)
public String update(@AuthUser UserVo authUser, Model model) {
  /////////////////////// 접근제어 ////////////////////////////
  //  UserVo authUser = (UserVo) session.getAttribute("authUser");
  //	if (authUser == null)
  //			return "redirect:/";
	//////////////////////////////////////////////////////////
	Long no = authUser.getNo();
	UserVo vo = userService.getUser(no);
	model.addAttribute("userVo", vo);
	return "/user/update";
}
```
## 2. 접근 권한 - 관리자 접근   
관리자만 접근할 수 있는 경로라면 컨트롤러의 메소드 위에 작성할 @Auth 어노테이션을 다음과 같이 작성한다.
### adminController.java
```java
package com.douzone.mysite.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import com.douzone.security.Auth;

@Auth("ADMIN")
@Controller
@RequestMapping("/admin")
public class AdminController {

	@RequestMapping("")
	public String main() {
		return "admin/main";
	}
	
	@RequestMapping("/guestbook")
	public String guestbook() {
		return "/admin/guestbook";
	}
	
	@RequestMapping("/board")
	public String board() {
		return "admin/board";
	}
	
	@RequestMapping("/user")
	public String user() {
		return "admin/user";
	}
}
```
## 3. 세션 처리 - @AuthUser   
접근 권한에서 사용했던 Controller에서 HttpSession 객체를 사용하여 세션을 관리한다.   
이 부분은 Spring답지 못한 코드이므로 @AuthUser 어노테이션으로 대체하여 관리한다.
### @AuthUser 어노테이션 생성
```java
package com.douzone.security;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.PARAMETER)
public @interface AuthUser {
}
```
@AuthUser 메소드는 파라미터에 작성할 어노테이션이므로 내용은 아무것도 작성하지 않는다.   
### HandlerMethodArgumentResolver 클래스 작성   
@AuthUser 어노테이션에 대하여 수행할 클래스이다. Controller 매개변수에 @AuthUser 어노테이션이 있는지 확인하고   
매개변수의 타입이 UserVo인지 확인 후 HttpSession 객체를 이용하여 세션 객체를 반환한다.
```java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;

import org.springframework.core.MethodParameter;
import org.springframework.web.bind.support.WebArgumentResolver;
import org.springframework.web.bind.support.WebDataBinderFactory;
import org.springframework.web.context.request.NativeWebRequest;
import org.springframework.web.method.support.HandlerMethodArgumentResolver;
import org.springframework.web.method.support.ModelAndViewContainer;

import com.douzone.mysite.vo.UserVo;

public class AuthUserHandlerMethodArgumentResolver implements HandlerMethodArgumentResolver {

	@Override
	public Object resolveArgument(
		MethodParameter parameter,
		ModelAndViewContainer mavContainer,
		NativeWebRequest webRequest,
		WebDataBinderFactory binderFactory) throws Exception {
		
    // 1. 파라미터에 @AuthUser가 붙어 있는지, 타입이 UserVo인지 확인
		if(supportsParameter(parameter) == false) {
      // 해석할 수 있는 파라미터가 아니라는 의미이다.
			return WebArgumentResolver.UNRESOLVED;
		}
		
    // 여기까지 진행이 되었다면 @AuthUser가 붙어있고 타입이 UserVo인 경우다.
		HttpServletRequest request = (HttpServletRequest)webRequest.getNativeRequest();
		HttpSession session = request.getSession();
		if(session == null) {
			return null;
		}
		
		return session.getAttribute("authUser");
	}

	@Override
	public boolean supportsParameter(MethodParameter parameter) {
		// @AuthUser가 붙어있는지 확인
    AuthUser authUser = parameter.getParameterAnnotation(AuthUser.class);
		
		// @AuthUser가 안붙여 있으면,
		if(authUser == null) {
			return false;
		}
		
		// 파라미터 타입이 UserVo가 아니면,
		if(parameter.getParameterType().equals(UserVo.class) == false) {
			return false;
		}
		
		return true;
	}
	
}
```
### 환경 설정 : spring-servlet.xml
ArgumentResolver가 처리할 수 있도록한다.
```xml
<!-- validator, conversionService, messageConverter를 자동으로 등록 -->
	<mvc:annotation-driven> 
		<mvc:argument-resolvers>
			<bean class="com.douzone.security.AuthUserHandlerMethodArgumentResolver"></bean>
		</mvc:argument-resolvers>
	</mvc:annotation-driven>
```
