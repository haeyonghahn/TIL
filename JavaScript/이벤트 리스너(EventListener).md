# 이벤트 리스너(EventListener)
## 1) 이벤트 핸들링(Event Handling)
이벤트란 대표적으로 클릭, 키보드 입력 등 사용자의 어떤 행위를 의미한다.
자주 쓰이는 이벤트 목록은,
* onblur : 객체가 focus를 잃었을 때
* onchange : 객체의 내용이 바뀌고 focus를 잃었을 때
* onclick : 객체를 클릭했을 때
* ondblclick : 더블클릭할 때
* onerror : 에러가 발생했을 때
* onfocus : 객체에 focus가 되었을 때
* onkeydown : 키를 눌렀을 때
* onkeypress : 키를 누르고 있을 때
* onkeyup : 키를 눌렀다 뗏을 때
* onload : 문서나 객체가 로딩되었을 때
* onmouseover : 마우스가 객체 위에 올라왔을 때
* onmouseout : 마우스가 객체 바깥으로 나갔을 때
* onreset : reset 버튼을 눌렀을 때
* onresize : 객체의 크기가 바뀌었을 때
* onscroll : 스크롤바를 조작할 때
* onsubmit : 폼이 전송될 때

이러한 이벤트를 처리하는 것을 이벤트 핸들링이라고 한다.

이벤트 핸들링은 다음과 같은 과정으로 이뤄진다.
1. 이벤트를 받아줄 요소를 선택한다.
2. 그 요소가 어떤 이벤트에 반응할지, 즉 요소와 이벤트를 연결해주는 바인딩을 한다.
3. 이벤트가 발생했을 때 실행될 코드를 작성한다.

## 2) 바인딩(Binding)
