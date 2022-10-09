# DTO, VO 그리고 Entity
## DTO
DTO(Data Transfer Object) 로서 계층(Layer) 간 데이터 교환을 위해 사용하는 객체이다.   
데이터 교환만을 위해 사용하므로 `로직을 갖지 않고, getter/setter 메소드` 만 갖는다.   
```java
@Data
public class UserDto {
    private String email;
    private String name;
    private String pwd;
    private String userId;
    private Date createAt;

    private String encryptedPwd;

    private List<ResponseOrder> orders;
}
```
## VO
VO(Value Object) 는 `값 그 자체를 표현하는 객체이다.`   
로직을 포함할 수 있으며, 객체의 `불변성(객체의 정보가 변경하지 않음)` 을 보장한다.   
서로 다른 이름을 갖는 VO 인스턴스라도 모든 속성 값이 같다면 두 인스턴스는 같은 객체라고 할 수 있다.   
이를 위해 VO 에는 Object 클래스의 `equals()` 와 `hashCode()` 를 오버라이딩해야 한다.
```java
public class RGBColor {
    private final int red;
    private final int green;
    private final int blue;

    public RGBColor(int red, int green, int blue) {
        this.red = red;
        this.green = green;
        this.blue = blue;
    }
  
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        RGBColor rgbColor = (RGBColor) o;
        return red == rgbColor.red && green == rgbColor.green && blue == rgbColor.blue;
    }

    @Override
    public int hashCode() {
        return Objects.hash(red, green, blue);
    }
}
```
### `equals()` 를 오버라이딩 하기 전
```java
public class RGBColorTest {

    @Test
    @DisplayName("vo_비교_테스트")
    public void vo_비교_테스트() {
        RGBColor color1 = new RGBColor(124, 1, 0);
        RGBColor color2 = new RGBColor(124, 1, 0);

        boolean actual = color1.equals(color2);

        assert(color1.equals(color2));
    }
}
```
![image](https://user-images.githubusercontent.com/31242766/194737507-876de7e0-8f33-474d-85fd-c9b8dbff1be8.png)

### `equals()` 를 오버라이딩한 후
![image](https://user-images.githubusercontent.com/31242766/194737560-39cbbaca-3e2b-44d6-95ad-df1fa20d0069.png)

## Entity
실제 `DB의 테이블과 매핑되는 객체이다.`     
VO와 마찬가지로 로직을 가질 수 있다.   
```java
@Entity
@Table(name = "users")
public class UserEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, length = 50, unique = true)
    private String email;
    @Column(nullable = false, length = 50)
    private String name;
    @Column(nullable = false, unique = true)
    private String userId;
    @Column(nullable = false, unique = true)
    private String encryptedPwd;
}
```
## 정리
- DTO : 계층(Layer) 간 데이터 이동을 위해 사용되는 객체   
- VO : 값을 갖는 순수한 도메인    
- Entity : 이를 DB 테이블과 매핑하는 객체     

|    |DTO|VO|Entity|
|----|------------------------|-------------------------|-----------|
|용도|레이어간의 데이터 전송|의미 있는 값을 표현|DB 테이블과 매핑되는 클래스|
|가변/불변|가변객체 생성 후 <br> 상태를 변경할 수 있다.|불변객체 생성 후 <br> 상태 변경이 없다.|가변객체 생성 후 <br> 상태를 변경할 수 있다.|
|로직 포함 여부|로직을 포함할 수 없다.|로직을 포함할 수 있다.|로직을 포함할 수 있다.|

## 출처
https://youngjinmo.github.io/2021/04/dto-vo-entity/   
