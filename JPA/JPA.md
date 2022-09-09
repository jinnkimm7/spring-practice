# JPA

## `Java Persistence API`
## 자바 진영의 `ORM` 기술 표준

> ORM?
> - Object-relational mapping(객체 관계 매핑)
> - 객체는 객체대로 설계
> - 관계형 데이터베이스는 관계형 데이터베이스대로 설계
> - ORM 프레임워크가 중간에서 매핑
> - 대중적인 언어에는 대부분 ORM 기술이 존재

- 객체를 데이터베이스에 저장, 조회하려면 복잡한 JDBC API와 SQL을 직접 작성해야 했다.
- JDBC Template이나 MyBatis와 같은 SQL 맵퍼가 등장해서 코드는 많이 줄었지만, SQL은 직접 작성해야한다.
- 하지만, JPA등장 이후 SQL을 작성할 필요가 사라졌다. JPA가 적절한 SQL을 생성하고 데이터베이스에 실행해서 객체를 저장하거나 불러오게 된다.

### `JPA에서 제일 중요한 것`
> 객체랑 테이블을 잘 설계하고 Mapping을 정확하게 하는 것이다.

### 목표1 : 객체와 테이블 설계 매핑
- 객체와 테이블을 제대로 설계하고 매핑하는 방법
- 기본키와 외래키 매핑
- 1:N, N:1, 1:1, N:M 매핑

### 목표2 : JPA 내부 동작 방식 이해
- JPA가 어떤 SQL을 만들어내는지 이해
- JPA가 언제 SQL을 실행하는지 이해 

---

## JPA를 왜 사용해야하는가?
- SQL 중심적인 개발에서 객체 중심으로 개발
- 생산성
  - 저장 : jpa.persist(member)
  - 조회 : Member member = jpa.find(memberId)
  - 수정 : member.setName("변경할 이름")
  - 삭제 : jpa.remove(member)
- 유지보수
  - 기존: 필드변경시 모든 SQL 수정
  - JPA : 필드만 추가하면 됨, SQL은 JPA가 처리
- 패러타임의 불일치 해결
- 성능
- 데이터 접근 추상화와 벤더 독립성
- 표준

> ORM은 객체와 RDM 두 기둥위에 있는 기술 -> 둘다 잘 알아야 된다.