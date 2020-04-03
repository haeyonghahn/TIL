# Spring Interceptor
Interceptor란 컨트롤러에 들어오는 요청 HttpRequest와 컨트롤러가 응답하는 HttpResponse를 가로채는 역할을 한다.   
인터셉터는 관리자만 접근할 수 있는 관리자 페이지에 접근하기 전에 관리자 인증을 하는 용도로 활용될 수 있다.   
컨트롤러에서 인터셉터를 활용하여 접근 권한을 하고 기술 침투적인 HttpSession을 제거하여 세션을 처리한다.   
인터셉터는 Servlet의 앞, 뒤에서 HttpRequest, HttpResponse을 가로채는 Filter와 유사한데 Filter와 Interceptor는 분명히 다르다.
