# ajax(Asynchronous Javascript And XML)
ajax는 비동기식 통신이다. 웹에서 네이버 메일을 사용했다면, 삭제할 메일을 체크하고 삭제버튼을 누르면 메일이 삭제된다.
그 때 주소를 보면 변화가 없다. 우리가 웹을 이용하며 여기저기 서핑을 하면 주소가 항상 변한다. 페이지별로 이동하기 때문이다.
여기서 네이버 메일의 방식을 보면 서버와 ajax로 비동기식 통신을 하여 삭제가 되었을 때 메일리스트 부분만 새로고침한다.
또한 메일 리스트 하단의 페이징 기법도 똑같은 방식이다.

만약 저 부분을 ajax로 사용하지 않고 페이지에 직접 요청하여 링크를 변경시킨다면 매우 복잡한 로직과 매번 화면의 새로고침을 보게 된다.
로직을 간단하게 짤 수도 있지만 새로고침은 피할 수 없다. 즉, 메일을 삭제해도 화면이 깜빡이고 페이지를 다른 곳으로 넘겨도 깜빡이게 된다.

예를 들어, ajax에 대해 알아보자.
```JavaScript
$(function(){
	$("#btn-checkemail").click(function(){
		var email = $("#email").val();
		if(email == ''){
			return;
		}
		$.ajax({
			url: '${pageContext.request.contextPath }/api/user/checkemail?email=' + email,
			type: 'get',
			// contentType: 'application/json'
			data:'',
			dataType: 'json',
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
```
위의 코드는 #btn-checkemail 태그가 달린 버튼을 클릭하면 데이터베이스에 사용자 이메일이 존재하는지 여부를 ajax을 통해 인증 체크를 하는 코드이다.
여기서 ajax의 옵션에 대해 알아보자.   
* type : 통신 타입을 정한다. POST 또는 GET을 선택할 수 있다.   
* url : 요청할 URL을 기입한다.   
* data : 서버에 요청 시 보낼 파라미터를 기입한다.   
* dataType : 응답받을 데이터 타입을 선택한다. (xml, text, html, json, ...) (dataType은 필수 사항이 아니다. 알아서 정해준다.)   
* success : 요청 및 응답에 성공했을 때 행위를 기입한다.
* error : 요청 및 응답에 성공했을 때 행위를 기입한다

위의 코드로 다시 정리하면, 현재 위치에서 ${pageContext.request.contextPath }/api/user/checkemail?email=' + email 로 ''이라는
파라미터를 get 방식으로 요청한다. 그럼 /api/user/checkemail?email=' + email 페이지에서 ''이라는 파라미터를 받고 xml을 만들어서
다시 클라이언트에게 보내준다.
