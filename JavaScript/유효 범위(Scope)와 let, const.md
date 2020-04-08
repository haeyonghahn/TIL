# 유효 범위(Scope)와 let, const
JavaScript에서 변수를 선언하는 방법에는 var, let, const 키워드가 있다.
## 1. var - 함수 레벨 범위 (Function Level Scope)
var 키워드를 사용하여 변수를 선언하면 그 변수는 함수 레벨 범위를 갖는다.
```JavaScript
function foo() {
  if(true) {
    var x = 10;
  }
  console.log(x);
}
foo();  // 10
console.log(x); // error
```
변수 x가 Block Level Scope라면, if 문이 끝날 떄 변수 x에 대한 참조도 사라지게 된다. 따라서 foo() 함수 내부에 있는
console.log(x);는 출력될 수 없다.   

그러나 var 키워드는 변수를 함수 레벨 범위(Function Level Scope)로 정의하기 때문에 if문이 끝나도 함수 내에서 x가 유효하게 된다.
그런데 foo() 함수를 벗어나면 x에 대한 참조를 할 수 없기 때문에 foo() 함수 외부에 있는 console.log(x)는 에러가 발생한다.

아래 코드를 보자.
```JavaScript
if(true) {
  var x = 10;
}
console.log(x);
```
이 코드는 에러를 발생하지 않는다. x는 if 내부에 선언되었지만 if는 함수가 아니기 때문에 x는 Global Scope이다.
따라서 정상적으로 10을 출력한다.
## 2. let, const - 블록 레벨 범위 (Block Level Scope)
let, const 키워드를 사용하면 그 변수는 Block Level Scope를 갖는다.
* let으로 선언된 변수는 값을 수정할 수 있다.
* const로 선언된 변수는 값을 수정할 수 없는 상수이다.   
```JavaScript
// let
if(true) {
  let x = 10;
}
console.log(x); // error

// const
if(true) {
  const x = 10;
}
console.log(x); // error
```
let과 const로 선언한 변수는 Block Level Scope이기 때문에 if문을 벗어나면 x에 대해 참조할 수 없다.
## 3. let과 const의 특징
### 1) const 값 변경
```JavaScript
const x = 10;
x= 20;  // error
console.log(20);
```
const는 상수이므로 값을 변경할 수 없기 때문에 상수로 선언한 변수에 할당하려 한다면 error가 발생한다.
### 2) const 초기화
```JavaScript
const x;  // error
x = 10;
console.log(x);
```
const로 선언한 변수는 선언하자마자 초기화해야 한다.
### 3) let 중복 선언
```JavaScript
var x = 10;
var x = 20;
console.log(x); // 20
```
var는 위와 같이 같은 변수에 대해 중복 선언이 가능하다. 그러나 let은 ?
```JavaScript
let x = 10;
let x = 20; // error
console.log(x);
```
x는 이미 선언된 변수라 오류가 발생한다.
## 정적 스코프 (Static Scope)
문맥에 의해 유효 범위가 결정되는 것을 의미한다.
### 예제 1
```JavaScript
var x = 10;

function foo() {
  var x = 20;
  goo();
}

function goo() { console.log(x); }  // 10

foo();
```
### 예제 2
```JavaScript
var x = 10;

function foo(){
    var x = 20;
    goo();

    function goo() { console.log(x); }  // 20
}

foo();
```
두 예제의 차이는 goo() 함수의 위치이다. 예제 1은 출력 결과 10이고 예제 2는 출력 결과 20이다.
이와 같이 함수가 선언된 시점에서 변수의 유효범위가 어떻게 결정되는지에 따라 값이 달라지는 것이 확인 가능하다.
이러한 유효 범위를 갖는 scope를 Static Scope 또는 Lexical Scope라고 한다.
