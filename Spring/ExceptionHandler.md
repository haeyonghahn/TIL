# ExceptionHandler
### 예외 처리 과정
1. 예외에 대한 로그를 남긴다.
2. 클라이언트에게 에러 페이지를 보여준다.
### 예외를 처리하는 클래스 생성   
com.xxx.mysite.exception 패키지를 만들고 UserRepositoryException 클래스를 만들었다.
```java
package com.douzone.mysite.exception;

public class UserRepositoryException extends RuntimeException {

	private static final long serialVersionUID = 1L;

	public UserRepositoryException() {
		super("UserRepositoryException Occurs");
	}
	
	public UserRepositoryException(String message) {
		super(message);
	}
}
```
이제 Repository에서 직접 예외를 처리하던 (e.printStackTrce()메소드) try-catch 부분을 수정하겠다.
```java
public int insert(UserVo UserVo) {
  Connection connection = null;
  PreparedStatement pstmt = null;
	//int count = 0;

	try {
		connection = dataSource.getConnection();
    
		String sql = "insert into user values (null, ?, ?, ?, ?, now())";
		pstmt = connection.prepareStatement(sql);

		pstmt.setString(1, UserVo.getName());
		pstmt.setString(2, UserVo.getEmail());
		pstmt.setString(3, UserVo.getPassword());
		pstmt.setString(4, UserVo.getGender());

		count = pstmt.executeUpdate();
		// 5. 성공 여부
	} catch (SQLException e) {
       // e.printStackTrace();
		throw new UserRepositoryException("error :" + e);	
	} finally {
		try {
			// 6. 자원정리
			if (connection != null)
				connection.close();
			if(pstmt != null)
				pstmt.close();

		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	return count;
}
```
이렇게 작성을 하면 Repository에서 예외가 발생했을 경우 UserRepositoryException에서 처리를 할 수 있다.   
즉 Repository의 모든 메소드마다 로그를 남기고, 에러 페이지를 보여주는 과정을 예외로 던지면 UserRepositoryException에서 예외를 처리할 수 있다.  
   
Repository에서 예외가 발생하면 service 계층에서 예외를 처리해야 하는데, service 계층에서도 예외를 처리하지 않으면 Controller로 예외가 전달된다.   
따라서 Repository에서 발생한 예외를 Controller에서 받아 UserRepositoryException에서 예외가 처리되도록 Controller의 메서드를 작성한다.   
예외를 처리하라는 핸들러인 @ExceptionHandler 어노테이션을 작성하면 Repository에서 발생한 예외를 받을 수 있습니다.   
```java
  @ExceptionHandler(UserRepositoryException.class)
	public String handleException() {
		return "error/exception";
	}
```
이제 views/error 폴더에서 exception.jsp 파일을 작성해서 에러 페이지를 응답하는 것을 확인할 수 있다.

### Global Exception
하지만 위에 예외 처리에는 문제가 있다. 모든 컨트롤러마다 @ExceptionHandler 어노테이션이 붙은 메소드를 작성해야 한다.   
그래서 Controller마다 예외를 처리할 필요없이 예외를 처리하는 클래스에 @ControllerAdvice 어노테이션을 작성하여 글로벌하게 예외를 처리할 수 있도록 한다.
* GlobalExceptionHandler.java
```java
package com.douzone.mysite.exception;

import java.io.PrintWriter;
import java.io.StringWriter;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

@ControllerAdvice
public class GlobalExceptionHandler {

	@ExceptionHandler(Exception.class)
	public void handlerException(HttpServletRequest request, 
			HttpServletResponse response, Exception e) throws Exception {
		
		// 1. 로깅 ( logging)
		e.printStackTrace();
		StringWriter errors = new StringWriter(); // 버퍼
		e.printStackTrace(new PrintWriter(errors));
		
		// 2. 안내페이지 가기(정상종료)
		request.setAttribute("exception", errors.toString());
		request
			.getRequestDispatcher("/WEB-INF/views/error/exception.jsp")
			.forward(request, response);
		
		
	}
}
```
* 환경설정 : spring-servlet.xml 
```xml
<context:component-scan
		base-package="com.douzone.mysite.controller, com.douzone.mysite.exception"/>
```
GlobalExceptionHandler 클래스는 com.xxx.mysite.exception 패키지에 있으므로 어노테이션을 스캐닝하는 base-package에 추가한다.   
그러면 GlobalExceptionHandler 클래스의 @ControllerAdvice 어노테이션을 읽어 들이고 이 클래스는 예외 처리를 하는 클래스로 읽어 들이게 된다.   
   
GlobalExceptionHandler 클래스에서 request scope로 exception 객체를 전달했으므로 error 내용을 보기위해 jsp 파일에서 값을 확인한다.
* exception.jsp   
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>Exception Occurs</h1>
	<pre style="color:red">
		${exception }
	</pre>
</body>
</html>
```   
![ErrorMessage](https://github.com/haeyonghahn/TIL/blob/master/Spring/images/ErrorMessage.PNG)   
### 공통 에러 메시지 작성
404 error나 500 error는 공통적으로 발생하는 에러 메시지이다.   
views/error 폴더 안에 404.jsp와 500.jsp 페이지를 만든다.
* 404.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>Not Found(404) - Oooops</h1>
	<p> 죄송합니다 .페이지를 찾을 수 없습니다. </p>
</body>
</html>
```
* 500.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>예기치 오류(500) - Oooops</h1>
	<p>
		죄송합니다. 오류가 발생했습니다.<br>
		잠시후, 다시 시도해 주세요.
	</p>

</body>
</html>
```   
* 환경 설정 : web.xml
```xml
<!-- 공통 에러 페이지 -->
	<error-page>
		<error-code>404</error-code>
		<location>/WEB-INF/views/error/404.jsp</location>	
	</error-page>
	<error-page>
		<error-code>500</error-code>
		<location>/WEB-INF/views/error/500.jsp</location>	
	</error-page>
```
