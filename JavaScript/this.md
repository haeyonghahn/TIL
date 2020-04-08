# this
메소드를 호출한 객체가 저장되는 것이다. 변수는 물론 함수, 브라우저의 window 객체 등이 될 수도 있다.
브라우저 콘솔을 켜고 this를 입력해본다.
```JavaScript
this; // window {}
```   
함수 안에 넣어서 사용해본다.
```JavaScript
function a() { console.log(this); }
a();  // window {}
```
* 여기서 한 가지 알 수 있는 사항은 this는 기본적으로 window라는 것을 알 수 있다.   

## this가 window가 아닌 경우
### 객체의 메소드
```JavaScript
var obj = {
  a: function() { console.log(this); }
};
obj.a(); // obj
```
객체 메소드 a 안에 this는 객체를 가리킨다. 이것은 객체의 메소드를 호출할 때 this를 내부적으로 바꿔주기 때문이다.
단, 위의 예제에서 다음과 같이 하면 결과가 달라진다.
```JavaScript
var a2 = obj.a;
a2();  // window
```   
호출할 때, 호출하는 함수가 객체의 메소드인지 그냥 함수인지 중요하다. a2는 obj.a를 꺼내온 것이기 때문에 더 이상
obj의 메소드가 아니다.
```JavaScript
var obj2 = { c: 'd' };
function b() {
  console.log(this);
}
b();  // window
b.bind(obj2).call();  // obj2
b.call(obj2); // obj2
b.apply(obj2);  // obj2
```
명시적으로 this를 바꾸는 함수 메소드인 bind, call, apply를 사용하면 this가 객체를 가리킨다.
### 생성자 함수   
```JavaScript
function Person(name, age) {
  this.name = name;
  this.age = age;
}
Person.prototype.hi = function() {
  console.log(this.name, this.age);
};

Person('haeyong', 27);
console.log(window.name, window.age); // haeyong 25
```
그냥 함수에서 this는 window를 가리킨다고 위에서 설명되었다. 그래서 this.name과 this.age는
window.name, window.age가 되어버린다. 이를 막기 위해서
```JavaScript
var hero = new Person('haeyong', 27); // Person {name: 'haeyong', age: 27 }
hero.hi();  // haeyong 27
```
new 연산자를 붙여서 this가 생성자를 통해 생성된 인스턴스(hero 자신)가 된다.

### 이벤트 리스너 및 기타 라이브러리
* 문제점 1
```JavaScript
document.body.onclick = function() {
  console.log(this);  // <body>
};
```
그냥 함수이지만 this가 window가 아니라 <body>로 바뀐다. 객체 메소드도 아니고 bind한 것도 아니고 new를
붙인 것도 아니지만 이벤트가 발생할 때 내부적으로 this가 바뀐다.   
* 문제점 2
```JavaScript
$('div').on('click', function() {
  console.log(this);  // <div>
  function inner() {
    console.log('inner', this); // inner window
  }
  inner();
});
```
방금 전 클릭 이벤트에서 jQuery가 내부적으로 this를 바꿔버린다고 했다. 하지만 inner 함수 호출 시에는
this가 window이다. 그 이유는 함수의 this는 기본적으로 window라는 원칙을 따른 것이다. 이러한 문제를
해결하기 위해서
```JavaScript
$('div').on('click', function() {
  console.log(this); // <div>
  var that = this;
  function inner() {
    console.log('inner', that); // inner <div>
  }
  inner();
});
```
정리하자면,

1) this는 기본적으로 window이지만, 객체 메소드, bind call apply, new일 때 this가 바꾼다.
2) 이벤트 리스너나 기타 라이브러리처럼 this를 내부적으로 바꿀 수도 있으니 항상 this를 확인해야한다.
3) 선언한 function의 this는 항상 window라는 점이다.
