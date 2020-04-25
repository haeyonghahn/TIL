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

## 전역 변수와 지역 변수
전역변수의 사용은 최대한 지양해야 한다. 전역변수란 JavaScript에서
제일 바깥 범위(함수 안에 포함되지 않은)에 변수를 생성하는 것이다.
즉, window 객체에 변수를 만드는 것이다.
```JavaScript
var x = 'global';
function ex() {
  var x = 'local';
  x = 'change';
}
ex(); // x를 바꿔본다.
alert(x); // 여전히 'global'
```
위의 상황에서 지역 변수는 아무리 해도 전역변수에 영향을 끼칠 수 없다.
이유는 함수 스코프 때문이다.

## 스코프 체인
내부 함수에서는 외부 함수의 변수에 접근 가능하지만 외부 함수에서는
내부 함수의 변수에 접근할 수 없다.
```JavaScript
var name = 'zero';
function outer() {
  console.log('외부', name);
  function inner() {
    var enemy = 'nero';
    console.log('내부', name);
  }
  inner();
}
outer();
console.log(enemy); // undefined
```
inner() 함수는 name 변수를 찾기 위해 먼저 자기 자신의 스코프에서 찾고
없으면 한 단계 올라가 outer() 함수 스코프에서 찾고 없으면 다시
올라가 결국 전역 스코프에서 찾는다. 다행히 전역 스코프에서 name
변수를 찾아서 'zero'라는 값을 얻었다. 만약 전역 스코프에도 없다면
변수를 찾지 못하는 에러가 발생한다. 이렇게 꼬리를 물고 계속 범위를
넓히면서 찾는 관계를 스코프 체인이라고 부른다.

## 렉시컬 스코핑(lexical scoping)
스코프는 함수를 호출할 때가 아니라 선언될 때 생성된다.
렉시컬 스코핑의 예제를 확인해 보자.
```JavaScript
var name = 'zero';
function log() {
  console.log(name);
}

function wrapper() {
  var name = 'nero';
  log();
}
wrapper();
```
log() 함수 안에 name은 wrapper 안의 지역변수 name이 아니라 전역변수 name을 가리키고 있는 것이다.

함수를 처음 선언하는 순간, 함수 내부의 변수들은 자기 스코프로부터
가장 가까운 곳에 있는 변수를 계속 참조하게 된다. 위에 예제에서 log() 함수 안에
name 변수는 선언 시 가장 가까운 전역변수 name을 참조하게 된다.
그래서 wrapper() 함수 안에 name = 'nero'를 참조하는 게 아니라
그대로 전역변수 name의 값인 zero가 나오는 것이다.
