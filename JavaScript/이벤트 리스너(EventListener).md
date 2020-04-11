# 이벤트 리스너(EventListener)
## 1) 이벤트 리스너 등록 방법
```JavaScript
window.onload = function () {
  alert('I\'m loaded');
};
```
이벤트 리스너는 해당 이벤트에 대해 대기중인 것이다. 해당 이벤트가 발생했을 때 등록했던 이벤트 리스너가 실행된다.
이벤트 리스너는 항상 on + '이벤트명'으로 명명된다.

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
```JavaScript
document.getElementById('clickMe').onclick = function () {
  alert('I\'m clicked!');
};
```
window 말고 여러 태그에 각각 이벤트를 설정할 수 있다.
## 2) 이벤트 리스너 등록 방법
이벤트를 붙이는 다른 방법으로 addEventListener가 있다. addEventListener 방식은 여러 이벤트를 등록할 수도 있고
특정 이벤트를 제거할 수도 있다.
```JavaScript
function onClick() {
  alert('I\'m clicked!');
}
function onClick2() {
  alert('또다른 이벤트');
}
document.getElementById('clickMe').addEventListener('click', onClick); // 이벤트 연결
document.getElementById('clickMe').addEventListener('click', onClick2); // 또 하나의 이벤트 연결
document.getElementById('clickMe').removeEventListener('click', onClick); // 연결할 이벤트 중 하나 제거
```
