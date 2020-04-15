# position
position은 태그들의 위치를 결정하는 css이다.

## 1) static
모든 태그들의 default 값은 static이다. 차례대로 왼쪽에서 오른쪽, 위에서 아래로 쌓인다.
```css
span, div {
  background: yellow;
  border: 1px solid red;
}
```
```html
<span>span1</span>
<span>span2</span>
<span>span3</span>
<div>div1</div>
```
![position](https://github.com/haeyonghahn/TIL/blob/master/HTML-CSS/images/position01.PNG)

## 2) relative
태그들의 위치를 살짝 변경하고 싶을 때 position: relative; 속성을 사용한다. top, right, bottom, left 속성을 사용해 위치 조절이 가능하다.
```css
span, div {
  background: yellow;
  border: 1px solid red;
}

.top {
  position: relative;
  top: 5px;
  z-index: 1;
}

.right {
  position: relative;
  right: 5px;
}

.bottom {
  position: relative;
  bottom: 5px;
}

.left {
  position: relative;
  left: 5px;
}
```
```html
<span class="top">top</span>
<span class="right">right</span>
<span class="bottom">bottom</span>
<div class="left">left</div>
```
![position02](https://github.com/haeyonghahn/TIL/blob/master/HTML-CSS/images/position02.PNG)   
주의할 점은 #top 태그에 top: 5px;를 줬는데 아래로 이동했다. relative는 각각의 방향을 기준으로 태그 안쪽 방향으로 이동한다.
그러므로 바깥쪽으로 이동하게 만들기 위해선 5px 대신 음수 -5px를 줘야 한다.

또한, #top이 #left보다 위에 있다. 보통 태그는 나중에 나온 태그가 더 위에 배치되지만 z-index라는 속성을 #top 태그에 줬기 때문에
#left 태그보다 위로 올라갔다.

## absolute
relative가 static인 상태를 기준으로 픽셀만큼 움직였다면, absolute는 position: static; 속성을 가지고 있지 않은 부모를 기준으로 움직인다.
만약 부모 중 포지션이 relative, absolute, fixed인 태그가 없다면 가장 위의 태그인 body가 기준이 된다.
```css
#absolute {
  background: yellow;
  position: absolute;
  right: 0;
}

#parent {
  position: relative;
  width: 100px;
  height: 100px;
  background: skyblue;
}

#child {
  position: absolute;
  right: 0;
}
```
```html
<div>
  <div id="absolute">absolute</div>
</div>
<div id="parent">
  <div id="child">children</div>
</div>
```
![position03](https://github.com/haeyonghahn/TIL/blob/master/HTML-CSS/images/position03.PNG)   
위의 예시를 보면, #absolute는 조상 태그 중 postion: relative인 것이 없기 때문에 body를 기준으로 가장 오른쪽으로 달라붙었다. 반면 #child는 조상 태그인 #parent가 position: relative이기 때문에 그것을 기준으로 가장 오른쪽으로 달라붙었다.

참고로 absolute가 되면 div여도 width: 100%;가 아니다.

## fixed
항상 특정 위치에 고정될 수 있도록 지정할 수 있다. 마우스 스크롤을 내려도 항상 제 자리에 있도록 할 수 있는 속성이다.
```css
#fixed {
  background: yellow;
  position: fixed;
  right: 0;
}
```
```html
<div>
  <div id="fixed">fixed</div>
</div>
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
```
![position04](https://github.com/haeyonghahn/TIL/blob/master/HTML-CSS/images/position04.PNG)


