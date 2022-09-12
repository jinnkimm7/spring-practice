# JPA 구동 방식

1. JPA는 `Persistence`라는 클래스가 있다.
2. `Persistence` 클래스가 설정정보를 조회한다.
    - META-INF/persistence.xml 
3. 설정정보를 읽은 후, `EntityManagerFactory`라는 클래스를 만든다.
4. 필요시, `EntityManagerFactory`에서 `EntityManger`를 생성한다.


JpaMain
```java
package hellojpa;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Persistence;

public class JpaMain {
    public static void main(String[] args) {
      // EntityManagerFactory는 애플리케이션 로딩 시점에 딱 하나만 생성해서 애플리케이션 전체에서 공유
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

      // 트랜잭션 단위마다(고객의 요청이 올 때마다) EntityManager 생성
      // 쓰레드간 공유X(사용하고 버려야한다.)
        EntityManager em = emf.createEntityManager();
        //code
        em.close();

        emf.close();
    }
}
```

persistence.xml
```java
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.2"
             xmlns="http://xmlns.jcp.org/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd">
    <persistence-unit name="hello">
        <properties>
            <!-- 필수 속성 -->
            <property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/>
            <property name="javax.persistence.jdbc.user" value="sa"/>
            <property name="javax.persistence.jdbc.password" value=""/>
            <property name="javax.persistence.jdbc.url" value="jdbc:h2:tcp://localhost/~/test"/>
            <property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/>
            <!-- 옵션 -->
            <property name="hibernate.show_sql" value="true"/>
            <property name="hibernate.format_sql" value="true"/>
            <property name="hibernate.use_sql_comments" value="true"/>
            <!--<property name="hibernate.hbm2ddl.auto" value="create" />-->
        </properties>
    </persistence-unit>
</persistence>
```

--- 

## 객체와 테이블을 생성하고 매핑하기
```sql
create table Member(
  id bigint not null,
  name varchar(255),
  primary key (id)
);
```

```java
package hellojpa;

import javax.persistence.Entity;
import javax.persistence.Id;

@Entity
public class Member {
    @Id
    private Long id;
    private String name;

// Getter, Setter --
}
```

```java
package hellojpa;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;

public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

        EntityManager em = emf.createEntityManager();

      // 주의 : 데이터의 모든 변경은 트랜잭션 안에서 실행
        EntityTransaction tx = em.getTransaction();
        tx.begin();

        try{
          Member member = new Member();
          member.setId(1L);
          member.setName("HelloA");

          em.persist(member);

          tx.commit();
        } catch (Exception e) {
          tx.rollback();
        } finally {
          em.close();
        }

        emf.close();
    }
}
```

- 쿼리를 직접 만든 것이 없고, JPA가 맵핑정보를 보고 값을 넣어줌.
- `@Entity`
  - JPA가 관리할 객체
  - JPA를 사용하는 아이구나! 관리할 대상이구나! 라고 인식
- `@Id`
  - 데이터베이스 PK와 매핑

---

## JPQL
- 가장 단순한 조회 방법은 아래와 같다.
  - EntityManager.find()
  - 객체 그래프 탐색(a.getB().getC())
- 하지만 나이가 18살 이상인 회원을 모두 검색하고 싶다면과 어떻게 조회할까? -> JPQL
```java
...
        try{
            List<Member> result = em.createQuery("select m from Member as m", Member.class)
                    .setFirstResult(5)
                    .setMaxResults(8)
                    .getResultList();
            for (Member member : result) {
                System.out.println("member.name = " + member.getName());
                
            }
...
```

- JPQL은 객체를 대상으로하는 객체지향 쿼리이다. 방언에 맞춰서 각 DB에 맞게 번역해준다.
- JPA를 사용하면 엔티티 객체를 중심으로 개발
- 문제를 검색 쿼리인데
- 검색을 할 때도 테이블이 아닌 엔티티 객체를 대상으로 검색
- 모든 DB 데이터를 객체로 변환해서 검색하는 것은 불가능
- 애플리케이션이 필요한 데이터만 DB에서 불러오면 결국 `검색 조건이 포함된 SQL이 필요하다.`

> 따라서, JPA는 SQL을 추상화한 JPQL이라는 객체 지향 쿼리 언어를 제공한다.

- SQL과 유사한 문법을 갖고 있다. (SELECT, FROM, WHERE, GROUP BY, HAVING, JOIN)
- `JPQL은 엔티티 객체를 대상으로 쿼리`를 한다.
  - 방언을 바꿔도 쿼리를 바꿀 필요가 없다.
- `SQL은 DB 테이블을 대상으로 쿼리`를 한다.


> JPQL
> - 테이블이 아닌 `객체를 대상으로 검색하는 객체 지향 쿼리`
> - SQL을 추상화해서 특정 데이터베이스에 의존하지 않는다.
> - JPQL을 한마디로 정의하면 객체 지향 SQL이다.
>   - 실행을 하면 방언과 합쳐져서 DB에 맞는 적절한 SQL이 나온다.
> - JPQL은 추후에 자세히 다룰 예정이다.
