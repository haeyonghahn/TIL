# Class
class는 생성자 함수의 업그레이드 버전이라고 보면 된다.
```JavaScript
function person (name, job) {
    this.name = name;
    this.job = job;
};
 
person.prototype.getName = function(){
    console.log(this.name);
};
 
function korean (name, job) {
    person.apply(this, arguments);
}
korean.prototype = new person();
 
var man_1 = new person('커쇼', '야구선수');
man_1.getName(); // 커쇼
 
var man_2 = new korean('류현진', '야구선수');
man_2.getName(); // 류현진
```
위의 코드를 보면 기존 자바스크립트에서는 생성자 함수와 프로토타입링크, 프로토
타입 객체를 이용하여 메소드를 상속할 수 있었다.

하지만, ES6에서는 class를 이용해 메소드의 상속을 쉽고 직관적으로 볼 수 있다.
```JavaScript
class Polygon [extends extendsTarget] {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}
```
class 에서 constructor 는 객체를 만드는 컨스트럭터로, 프로토타입의 컨스트럭터와 같은 역할을 한다.
상속은 class를 선언할 때, extends 를 선언하고 그 뒤에 extends 할 class를 지정해 주면 해당 class를 상속한다.
또한, 어떤 클래스를 상속했을 경우, constructor 안에서 this를 사용하기 전에
super([argument])를 이용해 부모 클래스를 호출해줘야 한다.
아래 예시를 통해 다시 확인해 보자.
```JavaScript
class Person {
    constructor (name, job) {
        this.name = name;
        this.job = job;
    }
    getName() {
        console.log(this.name);
    }
}
 
class Korean extends Person {
    constructor (name, job, position) {
        super(name, job);
        this.position = position;
    }
    getPosition() {
        console.log(this.position);
    }
}
 
var man_1 = new Person('커쇼', '야구선수');
man_1.getName(); // 커쇼
 
var man_2 = new Korean('추신수', '야구선수', '외야수');
man_2.getName(); // 추신수
man_2.getPosition(); // 외야수 
```
정리하자면, 기존 prototype 방식에서 메소드 상속은
1. prototype 객체에 메소드를 정의
2. 상속받을 함수의 prototype link를 생성자 함수를 이용해 변경

의 단계를 거쳤지만 ES6의 class는 extends를 이용해 간단하게 메소드를
상속받을 수 있다.








