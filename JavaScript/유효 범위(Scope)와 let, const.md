# 유효 범위(Scope)와 let, const
JavaScript에서 변수를 선언하는 방법에는 var, let, const 키워드가 있다.
## 1. var - 함수 레벨 범위   
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
