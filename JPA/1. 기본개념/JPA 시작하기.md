# JPA 시작하기
## 객체 매핑   
`@Entity`   
이 클래스를 태이블과 매핑한다고 JPA에게 알려준다. 이렇게 @Entity가 사용된 클래스를 엔티티 클래스라 한다.   
`@Table`   
엔티티 클래스에 매핑할 테이블 정보를 알려준다. 여기서는 name 속성을 사용해서 테이블에 매핑한다. 이 어노테이션을 생략하면 클래스 이름을
테이블 이름으로 매핑한다.   
`@Id`   
엔티티 클래스의 필드를 테이블의 기본 키(Primary key)에 매핑한다. `@Id`가 사용된 필드를 식별자 필드라 한다.   
`@Column`   
필드를 컬럼에 매핑한다. `name` 속성을 사용해서 테이블 컬럼을 필드에 매핑한다.   
`매핑정보가 없는 필드`      
매핑 어노테이션을 생략하면 필드명을 사용해서 컬럼명으로 매핑한다. 만약 대소문자를 구분하는 데이터베이스를 사용하면 
`@Column(name="AGE")` 처럼 명시적으로 매핑해야 한다.
## persistence.xml 설정   
JPA는 persistence.xml 을 사용해서 필요한 설정 정보를 관리한다. 이 설정 파일이 META-INF/persistence.xml 클래스 패스 경로에 있으면 별도의 설정 없이 JPA가 인식할 수 있다.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence" version="2.1">
 <persistence-unit name="jpabook" > //**
 <properties>
 <!-- 필수 속성 -->
 <property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/>
 <property name="javax.persistence.jdbc.user" value="sa"/>
 <property name="javax.persistence.jdbc.password" value=""/>
 <property name="javax.persistence.jdbc.url"
 value="jdbc:h2:tcp://localhost/~/test"/>
 <property name="hibernate.dialect"
 value="org.hibernate.dialect.H2Dialect" />
 <!-- 옵션 -->
 <property name="hibernate.show_sql" value="true" />
 <property name="hibernate.format_sql" value="true" />
 <property name="hibernate.use_sql_comments" value="true" />
 <property name="hibernate.id.new_generator_mappings" value="true" />
 
 </properties>
 </persistence-unit>
</persistence> 
```
persistence.xml 의 내용을 분석해보자.
