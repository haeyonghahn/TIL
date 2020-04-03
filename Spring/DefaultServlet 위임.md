# DefaultServlet 위임   
톰캣은 클라이언트의 요청 URL을 보고 Servlet Mapping에 따라 URL에 매핑된 Servlet이 처리를 하는 구조이다.   
그리고 URL에 매핑되는 Servlet이 없다면, 예를들어 CSS, image 파일 같은 정적자원들은 defaultSevlet이 처리하도록 되어 있다.   
즉 CSS, image 파일들은 서버 외부에서 직접 접근 할 수 없는 /WEB-INF/assets 폴더 아래에 위치하는 것이 일반적인데,   
CSS, image 파일에 접근하기 위한 Servlet Mapping을 하지 않았으면 톰캣이 defaultServlet으로 처리하여 정적 파일에 접근을 한다.
일반적으로 정적 파일에 대해 Servlet Mapping을 하지 않는다.   
스프링에서는 DispatcherServlet이 모든 요청을 받아 들인 후 Handler mapping table에 따라 컨트롤러로 분기 한다.   
그렇기 때문에 DispatcherServlet은 정적 파일에 대해 톰캣이 defaultServlet으로 실행할 수 있는 기회를 뺏어간다.   
모든 요청은 일단 DispatcherServlet에서 처리해버리기 때문이다.       
정적 자원에 접근하기 위한 경로 설정을 JSP/Servlet과 똑같이 해도 스프링에서는 경로를 읽지 못한다.   
다시 정리하면, 
### JSP/Servlet에서는 톰캣이 default Servlet이 있기 때문에 처리가 가능하지만,   
### 스프링에서는 Dispatcher Servlet이 모든 요청을 받아들이기 때문에 톰캣의 default Servlet이 정적 파일을 처리할 수 없다.   
그래서 이것을 위임해야 한다.

### 환경 설정 : spring.xml   
```xml
<!-- validator, conversionService, messageConverter를 자동으로 등록 -->
	<mvc:annotation-driven></mvc:annotation-driven>
<!-- 서블릿 컨테이너의 디폴트 서블릿 위임 핸들러 -->
	<mvc:default-servlet-handler />
```   
