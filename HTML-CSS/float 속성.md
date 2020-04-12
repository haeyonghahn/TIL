# float
float는 이름 그대로 떠있다는 의미이다. 위에서 아래로, 왼쪽에서 오른쪽으로 태그들이
순서대로 배치되다가 그 중에 한 태그 공중에 뜨는 것이다. 그렇다면 그 뒤에 태그들은
그 자리를 메우려 한다
```css
#floatLeft {
  float: left;
  background: yellow;
}
.normal {
  background: skyblue;
}
#floatRight {
  float: right;
  background: yellowgreen;
}
.clear {
  clear: both;
  background: skyblue;
}
```
```html
<div id="floatLeft">float-left</div>
<div class="normal">normalnormalnormalnormalnormal</div>
<div id="floatRight">float-right</div>
<div class="normal">normalnormalnormalnormalnormal</div>
<div id="floatLeft">float-left</div>
<div class="clear">clearclearclearclearclear</div>
```
![float01](https://github.com/haeyonghahn/TIL/blob/master/HTML-CSS/images/float01.PNG)   

원래대로라면 normal 태그는 block 속성을 가지고 있으므로 한 줄을 통째로 차지해야 한다.
하지만 float 때문에 문단에 왼쪽과 오른쪽에 배치되었다.

만약 float-left와 float-right처럼 올라가는 것을 방지하고 싶다면
clear 속성을 통해서 float을 제거해 줄 수 있다. clear:left; 또는 clear:right;
또는 clear:both;를 통해 더 이상 겹치지 않게 할 수 있다.

## float 속성의 문제점
### 1) 문제점   
float의 문제점은 부모 태그가 float 속성의 자식 태그를 인식하지 못한다는 것이다.
따라서 높이(height)가 0이 되어버리는 문제가 발생한다.
```css
#parent {
  background: yellow;
}

.float {
  float: left;
}

#parent-overflow {
  background: skyblue;
  overflow: auto;
  clear: both;
}
```
```html
<div id="parent">
  <div class="float">float</div>
</div>
<div id="parent-overflow">
  <div class="float">float</div>
</div>
```
![float02](https://github.com/haeyonghahn/TIL/blob/master/HTML-CSS/images/float02.PNG)   
#parent .float의 div 태그는 배경이 노란색이 되어야 함에도 #parent의 자식인 .float의
float 속성때문에 인식하지 못해 height가 0이 되어 배경색이 나타나지 않는다.
float 속성의 자식을 인식하게 하려면 overflow: auto나 overflow: hidden을 주어야 부모 태그가
정상적으로 자식 태그의 높이를 인식한다.

### 2) 문제점
display: inline-block의 간격 문제를 해결하는 데도 float:left가 사용된다.
display: inline-block 대신 float: left를 사용한 것이다.
```css
#parent {
  background: yellow;
  height: 200px;
  width: 100%;
}

.child {
  vertical-align: middle;
  float: left;
  position: relative;
  top: 50%;
  transform: translateY(-50%);
  background: skyblue;
}
```
```html
<div id="parent">
  <div class="child">child1</div>
  <div class="child">child2</div>
  <div class="child">child3</div>
</div>
```
![float03](https://github.com/haeyonghahn/TIL/blob/master/HTML-CSS/images/float03.PNG)

