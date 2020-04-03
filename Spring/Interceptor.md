# Spring Interceptor
![Interceptor](https://github.com/haeyonghahn/TIL/blob/master/Spring/images/Interceptor.PNG)   
Interceptor란 컨트롤러에 들어오는 요청 HttpRequest와 컨트롤러가 응답하는 HttpResponse를 가로채는 역할을 한다.   
인터셉터는 관리자만 접근할 수 있는 관리자 페이지에 접근하기 전에 관리자 인증을 하는 용도로 활용될 수 있다.   
컨트롤러에서 인터셉터를 활용하여 접근 권한을 하고 기술 침투적인 HttpSession을 제거하여 세션을 처리한다.   
인터셉터는 Servlet의 앞, 뒤에서 HttpRequest, HttpResponse을 가로채는 Filter와 유사한데 Filter와 Interceptor는 분명히 다르다.
### Filter와 Interceptor의 차이
1. 호출 시점   
Filter는 DispatcherServlet이 실행되기 전, Interceptor는 DispatcherServlet이 실행된 후
2. 설정 위치   
Filter는 web.xml, Interceptor는 spring-servlet.xml
3. 구현 방식   
Filter는 web.xml에서 설정을 하면 구현이 가능하지만, Interceptor는 설정은 물론 메서드 구현이 필요하다.
### 환경 설정 : spring-servlet.xml   
'''
<!-- Interceptors -->
	<mvc:interceptors>
		<!-- <mvc:interceptor>
			<mvc:mapping path="/board/**"/>
			<bean class="com.douzone.mysite.interceptor.MyInterceptor02"></bean>
		</mvc:interceptor> -->
		<mvc:interceptor>
			<mvc:mapping path="/user/auth"/>
			<bean class="com.douzone.security.LoginInterceptor"></bean>
		</mvc:interceptor>
		
		<mvc:interceptor>
			<mvc:mapping path="/user/logout"/>
			<bean class="com.douzone.security.LogoutInterceptor"></bean>
		</mvc:interceptor>
		
		<mvc:interceptor>
			<mvc:mapping path="/**"/>
			<mvc:exclude-mapping path="/user/auth"/>
			<mvc:exclude-mapping path="/user/logout"/>
			<mvc:exclude-mapping path="/assets/**"/>
			<bean class="com.douzone.security.AuthInterceptor"></bean>
		</mvc:interceptor>
	</mvc:interceptors>
'''
