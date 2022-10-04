# JPQL

## JPQL 소개
- 필요한 데이터만 DB에서 불러오려면 결국 검색 조건이 포함된 SQL이 필요한데, JPA는 SQL을 추상화한 JPQL이라는 `객체 지향 쿼리 언어`를 제공한다.
  - JPQL은 특정 DB SQL에 의존하지 않는다.
  - JPQL은 결국 매핑정보와 방언정보가 조합이 되어 SQL으로 변환된다.
- JPQL은 테이블이 아니라 `엔티티 객체를 대상으로 쿼리`한다.

예시
```java
...
em.createQuery("select m from Member m where m.name = '%kim%'", Member.class).getResultList();
...
```

## JPQL 문법
```
select_문 :: =
  select_절
  from_절
  [where_절]
  [groupby_절]
  [having_절]
  [orderby_절]

update_문 :: = update_절 [where_절]
delete_문 :: = delete_절 [where_절]
```

- select m from Member as m where m.age > 18
- 엔티티와 속성은 대소문자를 구분한다.
- JPQL 키워드는 대소문자 구분하지를 않는다.(SELECT, FROM, where)
- 엔티티 이름을 사용한다. (Member)
  - 테이블이름이 아니다.
- 별칭은 필수로 사용한다.(m)
  - as는 생략가능하다.
- 집합과 정렬(GROUP BY, HAVING, ORDER BY) 모두 사용가능하다.

### TypeQuery, Query
- TypeQuery: 반환 타입이 명확할때 사용
- Query: 반환 타입이 명확하지 않을 때 사용

### 결과조회 API
- query.getResultList(): 결과가 `하나 이상`일때, 리스트 반환
  - 결과가 없으면 빈 리스트 반환
- query.getSingleResult(): 결과가 `정확히 하나`, 단일 객체 반환
  - 결과가 없으면: javax.persistence.NoResultException
  - 결과가 둘 이상이면: javax.persistence.NonUniqueResultException

### 파라미터 바인딩 - 이름기준, 위치기준

이름기준
```java
em.createQuery("select m from Member m where m.username = :username", Member.class)
  .setParameter("username", "member1")
  .getSingleResult();
```

- 위치기반은 위치가 바뀌면 버그가 발생할수 있기때문에 사용하지말자.

## 프로젝션
- SELECT절에 조회할 대상을 지정하는 것
- 프로젝션 대상: 엔티티, 임베디드 타입, 스칼라 타입(숫자, 문자등 기본 데이터 타입)
- SELECT `m` FROM Member m -> 엔티티 프로젝션
- SELECT `m.team` FROM Member m -> 엔티티 프로젝션
- SELECT `m.address` FROM Member m -> 임베디드 타입 프로젝션
- SELECT `m.username, m.age` FROM Member m -> 스칼라 타입 프로젝션
- DISTINCT로 중복 제거

### 프로젝션 - 여러 값 조회

```java
em.createQuery("select m.username, m.age from Member m").getResultList();
```

위 쿼리문은 값 타입이 두가지가 나오는데 어떻게 조회해야할까?

1. Query 타입으로 조회
2. Object[] 타입으로 조회
3. new 명령어로 조회

## 페이징 API
- JPA는 페이징을 다음 두 API로 추상화
- setFirstResult(int startP): 조회 시작 위치(0부터 시작)
- setMaxResult(int maxResult): 조회할 데이터 수

예시
```java
em.createQuery("select m from Member m order by m.age desc", Member.class)
  .setFirstResult(1)
  .setMaxResult(10)
  .getResultList();
```

## 조인
- 내부 조인
  - SELECT m FROM Member m JOIN m.team t
- 외부 조인
  - SELECT m FROM Member m LEFT JOIN m.team t
- 세타 조인(cross join)
  - SELECT count(m) FROM Member m, Team t where m.username = t.name

### 조인 - ON절
1. 조인 대상 필터링
- 회원과 팀을 조인하면서, 팀 이름이 A인 팀만 조인
  - JPQL
    - SELECT m, t FROM Member m LEFT JOIN m.team t `on` t.name = 'A'
  - SQL
    - SELECT m.*, t.* FROM Member m LEFT JOIN Team t `ON` m.TEAM_ID = t.id and t.name = 'A'

2. 연관관계 없는 엔티티 외부 조인
- 회원의 이름과 팀의 이름이 같은 대상 외부 조인
  - JPQL
    - SELECT m.t FROM Member m LEFT JOIN Team t `on` m.username = t.name
  - SQL
    - SELECT m.*, t.* FROM Member m LEFT JOIN Team t `ON` m.username = t.name

## 서브 쿼리
- 나이가 평균보다 많은 회원
  - select m from Member m where m.age > (select avg(m2.age) from Member m2)
- 한 건이라도 주문한 고객
  - select m from Member m where (select count(o) from Order o where m = o.member) > 0

### 서브 쿼리 지원 함수
- [NOT] EXISTS (subquery): 서브쿼리에 결과가 존재하면 참
  - {ALL | ANY | SOME}(subquery)
  - ALL: 모두 만족하면 참
  - ANY, SOME: 같은 의미, 조건을 하나라도 만족하면 참
- [NOT] IN (subquery): 서브쿼리의 결과 중 하나라도 같은 것이 있으면 참

- 예제
  - 팀A 소속인 회원
    - select m from Member m where `exists` (select t from m.team t where t.name = '팀A')
  - 전체 상품 각각의 재고보다 주문량이 많은 주문들
    - select o from Order o where o.orderAmount > `ALL` (select p.stockAmount from Prouduct p)
  - 어떤 팀이든 팀에 소속된 회원
    - select m from Member m where m.team = `ANY` (select t from Team t)

### JPA 서브 쿼리 한계
- JPA는 where, having 절에서만 서브쿼리 사용 가능하다.
- SELECT절도 사용 가능하다.
- FROM절의 서브쿼리는 현재 JPQL에서 불가능
  - 조인으로 풀 수 있으면 풀어서 해결하자