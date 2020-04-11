# 이벤트 리스너(EventListener)
## 1. 이벤트 핸들링(Event Handling)
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

## 2. 바인딩(Binding)
요소와 이벤트를 연결해주는 바인딩을 하는 방법에는 여러가지가 있다.

### 1) HTML 이벤트 핸들러
HTML 요소의 onclick 속성에 JavaScript 함수를 호출하여 바인딩하는 방법이다.
```JavaScript
<button class="btn" onclick="myFunction()">
```
그런데 HTML과 JS코드는 분리시켜 관리하는 것을 권장하기 때문에 이 방식은 좋은 방법이 아니다.

### 2) DOM 이벤트 핸들러
DOM 요소에 이벤트를 바인딩하고 이벤트가 발생하면 실행될 코드를 함수로 작성하는 방법이다.
```JavaScript
element.on이벤트이름 = myFunction(){...}
```
여기서 "on이벤트이름"이란 JavaScript 이벤트 명에 on을 붙인 것이다.
예를 들어, click 이벤트를 바인딩 하려면 onclick이 되겠고, blur 이벤트를 바인딩 하려면 onblur가 될 것이다. 하지만
이 방식도 좋은 방법은 아니다.

### 3) DOM 이벤트 리스너
DOM 요소에 addEventListener 메서드를 호출하여 이벤트를 바인딩하고, 수행할 함수를 작성한다.
```JavaScript
element.addEventListener("이벤트이름", myfunction(){ ... }, [, boolean]);
```
해당 방식이 바닐라 JavaScript 기준으로 가장 많이 사용되는 방식이다.
"2) DOM 이벤트 핸들러" 방식과는 달리 "이벤트이름"에는 원래 JavaScript 이벤트명을 작성하면 된다.
예를 들어 click 이벤트라면 click을 그대로 작성하면 된다.
3번째 매개변수는 이벤트 흐름을 true로 할 것인지 false로 할 것인지 작성한다. 일반적으로 false를 사용한다.

## 3. 이벤트 흐름
### 1) 이벤트 버블링(Event Bubbling)
HTML의 어떤 요소는 또 다른 어떤 요소의 내부에 속해있다.
DOM 구조에서 최상위 요소는 document이고 그 아래에 head , body , a , ul , li 등이 있다.
즉, 모든 요소는 다른 어떤 요소에 속해있는 트리 구조이다.
그래서 어떤 요소에 대해 어떤 이벤트가 발생하면, 그 요소의 부모도 같은 이벤트의 영향을 받게됩니다.
이러한 이벤트 흐름을 Event Bubbling이라고 한다.
```JavaScript
<ul id="a">
    <li id="b">
        <button id="c">alert 발생</a>
    </li>
</ul>

<script>
    function showElement() {
        alert(this.innerHTML);
    }
    el = document.getElementById("a");
    el.addEventListener('click', showElement, false);

    el = document.getElementById("b");
    el.addEventListener('click', showElement, false);

    el = document.getElementById("c");
    el.addEventListener('click', showElement, false);
</script>
```
위의 예제는 id 속성 값 a, b, c에 대해 click 이벤트를 바인딩했을 때, a, b, c 중 어느 요소를 클릭하느냐에 따라 alert 창이 몇 번 발생하는지를 보여준다. 예를 들어, c를 클릭하면 이벤트 버블링되어, b, a도 클릭이 되는 효과가 있어 총 3번의 alert 창이 발생합니다. b를 클릭하면 a가 이벤트 버블링되어, 총 2번의 alert 창이 발생한다. 이와 반대되는 개념이 Event Capturing 이다. Event Capturing은 자식으로 이벤트가 연계되는 이벤트 흐름을 말한다.
DOM 리스너(addEventListener)에서 3번째 매개변수의 값의 의미는 다음과 같다.
* false   
Event Bubbling
* true   
Event Capturing

일반적으로 bubbling을 사용하기 때문에 false를 사용한다.
