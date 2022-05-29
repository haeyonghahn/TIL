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

### JPA 표준 속성
- javax.persistence.jdbc.driver : JDBC 드라이버   
- javax.persistence.jdbc.user : 데이터베이스 접속 아이디
- javax.persistence.jdbc.password : 데이터베이스 접속 비밀번호
- javax.persistence.jdbc.url : 데이터베이스 접속 URL
### 하이버네이트 속성
- hibernate.dialect : 데이터베이스 방언(Dialect) 설정   
- H2 : org.hibernate.dialect.H2Dialect
- Oracle 10g : org.hibernate.dialect.Oracle10gDialect
- MySQL : org.hibernate.dialect.MySQL5InnoDBDialect
### 하이버네이트 전용 속성
- hibernate.show_sql : 하이버네이트가 실행한 SQL을 출력한다.
- hibernate.format_sql : 하이버네이트가 실행한 SQL을 출력할 때 보기 쉽게 정렬한다.
- hibernate.use_sql_comments : 쿼리를 출력할 때 주석도 함께 출력한다.
- hibernate.id.new_generator_mappings : JPA 표준에 맞춘 새로운 키 생성 전략을 사용한다.
## 엔티티 매니저 설정
JPA를 시작하려면 우선 persistence.xml 의 설정 정보를 사용해서 엔티티 매니저 팩토리를 생성해야
한다. 이때 Persistence 클래스를 사용하는데 이 클래스는 엔티티 매니저 팩토리를 생성해서 JPA를
사용할 수 있게 준비한다.
```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("jpabook");
```
이렇게 하면 META-INF/persistence.xml 에서 이름이 jpabook 인 영속성 유닛( persistenceunit )을 찾아서 엔티티 매니저 팩토리를 생성한다. 이때 persistence.xml 의 설정 정보를 읽어서
JPA를 동작시키기 위한 기반 객체를 만들고 JPA 구현체에 따라서는 데이터베이스 커넥션 풀도 생성하므로 엔티티 매니저 팩토리를 생성하는 비용은 아주 크다. 
따라서 엔티티 매니저 팩토리는 애플리케이션 전체에서 딱 한 번만 생성하고 공유해서 사용해야 한다.
### 엔티티 매니저 생성
```java
EntityManager em = emf.createEntityManager();
```
엔티티 매니저를 사용해서 엔티티를 데이터베이스에 등록/수정/삭제/조회할 수 있다.   
참고로 엔티티 매니저는 데이터베이스 커넥션과 밀접한 관계가 있으므로 쓰레드간에 공유하거나 재사용하면 안 된다.
### 트랜잭션 관리
JPA를 사용하면 항상 트랜잭션 안에서 데이터를 변경해야 한다. 트랜잭션 없이 데이터를 변경하면 예외가 발생한다. 
트랜잭션을 시작하려면 엔티티 매니저( em )에서 트랜잭션 API를 받아와야 한다.
```java
EntityTransaction tx = em.getTransaction();
```
## JPQL(Java Persistence Query Language)
JPA는 SQL을 추상화한 JPQL이라는 객체 지향 쿼리 언어를 제공한다. JPQL은 SQL과 문법이 거의
유사해서 SELECT, FROM, WHERE, GROUP BY, HAVING, JOIN 등을 사용할 수 있다. 둘의 가장 큰 차이점은 다음과 같다.
- JPQL은 엔티티 객체를 대상으로 쿼리한다. 즉, 클래스와 필드를 대상으로 쿼리한다.
- SQL은 데이터베이스 테이블을 대상으로 쿼리한다.
