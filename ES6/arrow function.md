# Arrow Function

## Arrow 함수란
arrow 함수는 함수를 정의할 때 화살표(=>)를 사용하여 정의하는 것을 의미한다.
```JavaScript
// 기본 함수 선언 방식
function() { ... }

// Arrow 함수 선언 방식
() => { ... }
```

## Arrow 함수 사용하기
### 1) 함수에 이름 짓기
```JavaScript
let foo = () => { ... }
```
함수를 선언해서 변수에 할당하는 방법이다. function 키워드가 사라지고, =>가 생성된 것을 알 수 있다.
### 2) 여러 매개변수 할당하기
```JavaScript
let add = (a, b) => {
  return a + b;
}

console.log(add(3, 5));
```
함수에 매개변수를 정의하는 방법이다.
### 3) 즉시 실행함수
```JavaScript
((a,b) => {
  console.log(a + b);
})(3, 5)
```
즉시 실행이 가능하며, 즉시 실행 시 파라미터를 넘겨주는 예제이다.

## function 키워드로 선언한 함수와 Arrow로 선언한 함수 비교
### 1) prototype
```JavaScript
let foo = ( ) => {
    this.name = "victolee"
}

let goo = new foo() // error : foo is not a constructor
console.log(goo.name)
```
Arrow 함수는 new 키워드로 함수를 생성할 수 없다. 그 이유는
```JavaScript
let foo = ( ) => {
    this.name = "victolee"
}

function goo(){
    this.name = "victolee"
}

console.dir(foo)
console.dir(goo)
```
![arrow01](https://github.com/haeyonghahn/TIL/blob/master/ES6/images/arrow01.PNG)    
Arrow 함수(foo)에는 prototype이 없기 때문에 생성자 함수를 만들 수 없었던 것이다.   

### 2) argument 사용 불가
```JavaScript
let foo = ( ) => {
    console.log(arguments[0])
    console.log(arguments[1])
    console.log(arguments[2])
}
foo(1,2,3); // error : Identifier 'foo' has already been declared

function goo(){
  console.log(arguments[0])
  console.log(arguments[1])
  console.log(arguments[2])
}
goo(1,2,3);
```
함수에 매개변수를 정의하지 않아도 매개변수가 넘어오면 인자들을 저장하고 있는 arguments 객체가 있다. 그런데 arrow 함수에는 arguments가 정의되어 있지 않기 때문에 위와 같이 arguments를 사용하려고 하면 에러가 발생한다.

### 3) prototype에서 this
```JavaScript
// 1. arrow 함수
let Foo = function(){
    this.number = 10
}
Foo.prototype = {
    add : (num) => {
        return this.number + num
    }
}
let foo = new Foo()
console.log(foo.add(20))



// 2. 일반적인 함수
let Goo = function(){
    this.number = 10
}
Goo.prototype = {
  add : function(num){
    return this.number + num
  }
}
let goo = new Goo()
console.log(goo.add(20))
```
Foo 함수, Goo 함수의 prototype으로 add 속성을 추가했다. add는 각각 일반적인 함수와 arrow 함수를 통해 함수를 생성한 것이며, this를 통해 number 프로퍼티를 참조한다. 이 때 일반적인 함수 Goo.add() 함수 호출 결과 30을 얻지만, arrow 함수인 Foo.add()는 NaN을 얻는다. 그 이유는 Foo.add()를 선언한 방식인 arrow 함수에서 this는 window를 가르키기 때문이다. 따라서 window + 20의 결과는 NaN이기 때문에 NaN이라는 결과를 얻은것이다. 반면, Goo.add()에서 this는 자신을 호출한 객체를 가르킨다. 따라서 10 + 20의 값인 30이 출력된다.
