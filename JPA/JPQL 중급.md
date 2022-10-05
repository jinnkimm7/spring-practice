# JPQL 중급

## 경로 표현식
- 점(.)을 찍어 객체 그래프를 탐색하는 것
```
select m.username -> 상태필드
  from Member m
    join m.team t -> 단일 값 연관 필드
    join m.orders o -> 컬렉션 값 연관 필드
where t.name = '팀A'
```

### 경로 표현식 용어 정리
- 상태필드(state field): 단순히 값을 저장하기 위한 필드
- 연관 필드(association field): 연관관계를 위한 필드
  - 단일 값 연관 필드
    - @ManyToOne, @OneToOne, 대상이 엔티티(ex: m.team)
  - 컬렉션 값 연관 필드
    - @OneToMany, @ManyToMany, 대상이 컬렉션(ex: m.orders)

### 경로 표현식 특징
- 상태필드(state field): 경로 탐색의 끝이다. 더이상 탐색이 불가능하다.
- 단일 값 연관 경로: `묵시적 내부조인(inner join) 발생`이 발생한다. 탐색이 더 가능하다.
  - 참고: 실무에서는 묵시적인 내부조인을 피해야한다. 
- 컬렉션 값 연관 경로: 묵시적 내부 조인이 발생한다. 더 이상 탐색이 불가능하다.
  - FROM 절에서 명시적 조인을 통해 별칭을 얻으면 별칭을 통해 탐색가능 -> 명시적 조인을 사용해야 쿼리 튜닝하기 편하다.

### 명시적 조인, 묵시적 조인
- 명시적 조인: join 키워드 직접 사용
  - select m from Member m join Team t
- 묵시적 조인: 경로 표현식에 의해 묵시적으로 SQL 조인 발생(내부 조인만 가능)
  - select m.team from Member m

### 경로 표현식 예제
- select o.member.team from Order o -> 성공
- select t.members from Team t -> 성공
- select t.members.username from Team t -> 실패
  - 컬렉션에서는 더 이상의 탐색이 불가능하다.
- select m.username from Team t join t.members m -> 성공

### 경로탐색을 사용한 묵시적 조인 시 주의사항
- 항상 내부 조인
- 컬렉션은 경로탐색의 끝이다. 명시적 조인을 통해 별칭을 얻어야 추가적인 경로탐색이 가능하다.
- 경로 탐색은 주로 SELECT, WHERE 절에서 사용하지만, 묵시적인 조인으로 인해 SQL의 FROM (JOIN)절에 영향을 준다.

### 실무조언
- `가급적이면 묵시적 조인 대신에 명시적 조인을 사용하자`
- 조인은 SQL 튜닝에 중요 포인트이다.
- 묵시적 조인은 조인이 일어나는 상황을 한눈에 파악하기 어렵다.

---

## 페치조인 fetch join ⭐️⭐️
- 실무에서 엄청 중요하다.
- SQL 조인의 종류가 아니다.
- JPQL에서 `성능 최적화`를 위해 제공하는 기능
- 연관된 엔티티나 컬렉션을 `SQL 한번에 함께 조회`하는 기능
- join fetch 명령어 사용
- 페치 조인 ::=[LEFT [OUTER] | INNER] JOIN FETCH 조인경로

### 엔티티 페치 조인
- 회원을 조회하면서 연관된 팀도 함께 조회하고 싶을때?
- SQL을 보면 회원 뿐만 아니라 `팀(T.*)`도 함께 SELECT
- [JPQL]
  - select m from Member m `join fetch` m.team
- [SQL]
  - SELECT M.*, `T.*` FROM MEMBER M `INNER JOIN` TEAM T ON M.TEAM_ID = T.ID
- 페치조인으로 회원과 팀을 함께 조회해서 지연로딩이 발생하지 않는다.

### 컬렉션 페치 조인
- 일대다 관계, 컬렉션 페치 조인
- [JPQL]
  - select t from Team t `join fetch t.members` where t.name = '팀A'
- [SQL]
  - SELECT T.* , `M.*` FROM TEAM T INNER JOIN MEMBER M ON T.ID = M.TEAM_ID WHERE T.NAME = '팀A'

### 페치조인과 DISTINCT
- SQL의 DISTINCT는 중복된 결과를 제거하는 명령
- JPQL의 DISTINCT 2가지 기능 제공
  - 1. SQL에 DISTINCT를 추가
  - 2. 애플리케이션에 엔티티 중복 제거