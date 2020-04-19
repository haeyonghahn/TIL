# 클로저(Closure)
클로저란 이미 생명 주기가 끝난 외부 함수의 변수를 클로저라고 한다.
```JavaScript
function outer() {
  var x = 10;
  
  function inner() {
    x++;
    console.log(x);
  }
  return inner;
}
```
예를 들어, outer() 함수가 선언될 당시에 그 내부에는 x라는 변수와 inner() 함수를 정의하고 있고 outer() 함수는 inner 함수를 반환한다.
만약 outer() 함수 외부에서 outer() 함수를 호출하면, 다음과 같은 실행 순서를 따른다.    
1. inner 함수가 반환되어 inner() 함수 실행
2. outer() 함수에서 정의된 변수 x를 참조해서 ++ 연산자를 수행한다.   
즉, outer 함수를 호출하면 inner 함수에서 변수 x가 자신의 유효 범위가 아님에도, outer() 함수에 정의된 변수를 참조한다.
이 때 inner() 함수를 클로저라고 하며, outer() 함수에 정의된 변수 x를 자유 변수라고 한다.

## 클로저를 통한 캡슐화
클로저를 통해 변수 또한 함수를 private으로 활용할 수 있다. 즉, 클로저는 변수의 유효범위를 제한하려는 용도로 사용할 수 있다.
```JavaScript
function Outer(){
    var x = 10;

    this.getX = function(){
        return x;
    }

    this.setX = function(newNum){
        x = newNum;
    }
}

var foo = new Outer();
console.log(foo.getX());
console.log(foo.x)

foo.setX(20);
console.log(foo.getX());
```
Outer() 함수에 정의된 변수 x는 캡슐화 되어있다. 객체지향 프로그래밍 언어(자바)와 대응 시켜보면 아래와 같이 선언된 것으로 생각할 수 있다.
```Java
private int x = 10;
```
그래서 getter를 통해 x 값을 얻을 수 있으며, setter를 통해 x 값을 설정할 수 있도록 했다. 만약 직접 x에 접근하면, undefined를 반환된다.
즉, 클로저는 단순히 생성 시점 유효 범위의 환경을 순간 포착하는 것 뿐만 아니라, 외부에는 노출시키지 않으면서 선언 당시 유효 범위의 접근을 가능하게 하고 상태를 수정할 수 있게 해주는 정보 은닉 수단으로 활용 할 수도 있다.

## 클로저로 인한 문제
비동기로 동작하는 함수를 사용하는 함수 내에서 반복문을 작성할 때, 클로저로 인해 문제가 발생할 수 있다.

### 1) 해결책 - 즉시 실행함수
```JavaScript
function count() {
    for (var i = 1; i < 10; i++) {
        (function(count){
            setTimeout(function(){
                console.log(count);
            }, 1000);
        })(i);
    }
}
count();
```
즉시 실행 함수를 수행하면, 반복문의 단계가 수행할 때마다 i 값을 인자로 넘겨주기 때문에 해당 값을 출력하게 된다.
### 2) 블록 스코프
```JavaScript
function count() {
    for (let i = 1; i < 10; i++) {
        setTimeout(function(){
            console.log(i);
        }, 1000);
    }
}
count();
```
for 문에서 변수 선언을 var가 아닌 let 키워드를 사용한다. let은 블록 스코프( block scope )이기 때문에, 반복문의 각 단계가 같은 변수 i를 공유하고 있지 않다.
