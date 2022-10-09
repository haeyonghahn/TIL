# Equals와 hashCode
- `equals()` : 객체의 값의 일치여부(boolean) 을 반환하는 타입이다.
- `hashCode()` : 객체의 주소값(int) 을 이용하여 `객체 고유의 해시코드`를 리턴하는 함수이다.

## Equals()
```java
public void string_비교_테스트() {
    String str1 = new String("hello");
    String str2 = new String("hello");

    System.out.printf("%s : %b\n", "str1==str2", (str1==str2));
    System.out.printf("%s : %b\n", "str1.equals(str2)", (str1.equals(str2)));
}
```
```console
str1==str2 : false
str1.equals(str2) : true
```
`==` 연산자는 리터럴 값을 비교하는데 `new String()` 과 `new String()` 은 서로 다른 주소값을 참조하는 객체이기 때문에 false 를 반환한다.   

## hashCode
### Card 클래스
```java
public class Card {
    String name;
    String number;
    public Card(String name, String number) {
        this.name = name;
        this.number = number;
    }

    @Override
    public String toString() {
        return "Card{" +
                "name='" + name + '\'' +
                ", number=" + number +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Card card = (Card) o;
        return Objects.equals(name, card.name) && Objects.equals(number, card.number);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, number);
    }
}
```
### hashCode 오버라이드 하기 전
```java
public void set_비교_테스트() {
    Set<Card> cardSet = new HashSet<>();
    cardSet.add(new Card("Hyundai","0000-0000-0000"));
    cardSet.add(new Card("Hyundai","0000-0000-0000"));

    for (Card card : cardSet) {
        System.out.println(card.toString());
    }
    System.out.println("size : "+ cardSet.size());
}
```
```console
Card{name='Hyundai', number=0000-0000-0000}
Card{name='Hyundai', number=0000-0000-0000}
size : 2
```
`Set 은 중복을 허용하지 않는다.` 따라서 똑같은 속성을 지닌 Card 객체를 HashSet 에 삽입했다면, HashSet 에선 중복되는 데이터 하는 삭제하고 `size()` 로 `1` 을 반환했어야 한다. 그런데 `size()` 를 `2` 로 반환했다는 건 `toString()` 을 통해서도 확인했지만, `new` 로 선언한 두 Card 객체를 컴파일러에서 다르다고 판단했다. 그럼 왜 다른지 `hashCode` 를 출력해서 확인해보자.
```java
for (Card card : cardSet) {
    System.out.println(card.hashCode());
}
```
```console
1195942137
1259639178
size : 2
```
hashCode 가 달랐기 때문에 컴파일러에선 같은 속성을 지닌 Card 객체들을 다르게 본 것이다. `equals()` 와 `hashCode()` 를 생성하고 다시 실행시켜보자. HashSet 에서 똑같은 속성을 지닌 `Card` 객체를 중복으로 인식하여 똑같은 객체는 삭제하고 하나의 `Card` 객체만 갖고 있는 것을 확인할 수 있다. 여기서 중요한 건 `equals()` 만 오버라이딩해서는 안되고 `hashCode()` 까지 오버라이딩해야 정상 동작한다.
```console
-335758241
size : 1
```

## 출처
https://youngjinmo.github.io/2021/05/equals-hashcode/    

