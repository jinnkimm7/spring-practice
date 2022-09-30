# Spring + JPA

[김영한의 스프링 + JPA 완전정복](https://www.inflearn.com/roadmaps/373) 강의를 듣고, [토비의 스프링](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788960773431)을 참고하며, 공부한 내용을 기록하는 레포지토리입니다.

---
## 목차

### 스프링 핵심 원리
- IoC와 DI 
  - [IoC](https://github.com/jinnkimm7/spring-practice/blob/main/%EC%8A%A4%ED%94%84%EB%A7%81%20%ED%95%B5%EC%8B%AC%20%EC%9B%90%EB%A6%AC/IoC.md)
  - [DI](https://github.com/jinnkimm7/spring-practice/blob/main/%EC%8A%A4%ED%94%84%EB%A7%81%20%ED%95%B5%EC%8B%AC%20%EC%9B%90%EB%A6%AC/Dependency%20Injection.md)
- [스프링 컨테이너와 스프링 빈](https://github.com/jinnkimm7/spring-practice/blob/main/%EC%8A%A4%ED%94%84%EB%A7%81%20%ED%95%B5%EC%8B%AC%20%EC%9B%90%EB%A6%AC/%EC%8A%A4%ED%94%84%EB%A7%81%20%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88%EC%99%80%20%EC%8A%A4%ED%94%84%EB%A7%81%20%EB%B9%88.md)
- [싱글톤 컨테이너](https://github.com/jinnkimm7/spring-practice/blob/main/%EC%8A%A4%ED%94%84%EB%A7%81%20%ED%95%B5%EC%8B%AC%20%EC%9B%90%EB%A6%AC/%EC%8B%B1%EA%B8%80%ED%86%A4%20%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88.md)
- [컴포넌트 스캔](https://github.com/jinnkimm7/spring-practice/blob/main/%EC%8A%A4%ED%94%84%EB%A7%81%20%ED%95%B5%EC%8B%AC%20%EC%9B%90%EB%A6%AC/%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%20%EC%8A%A4%EC%BA%94.md)
- [의존관계 자동주입](https://github.com/jinnkimm7/spring-practice/blob/main/%EC%8A%A4%ED%94%84%EB%A7%81%20%ED%95%B5%EC%8B%AC%20%EC%9B%90%EB%A6%AC/%EC%9D%98%EC%A1%B4%EA%B4%80%EA%B3%84%20%EC%9E%90%EB%8F%99%20%EC%A3%BC%EC%9E%85.md)
  - [의존관계 주입방법](https://github.com/jinnkimm7/spring-practice/blob/main/%EC%8A%A4%ED%94%84%EB%A7%81%20%ED%95%B5%EC%8B%AC%20%EC%9B%90%EB%A6%AC/IoC.md)
- [빈 생명주기 콜백](https://github.com/jinnkimm7/spring-practice/blob/main/%EC%8A%A4%ED%94%84%EB%A7%81%20%ED%95%B5%EC%8B%AC%20%EC%9B%90%EB%A6%AC/%EB%B9%88%20%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0%20%EC%BD%9C%EB%B0%B1.md)
- [빈 스코프](https://github.com/jinnkimm7/spring-practice/blob/main/%EC%8A%A4%ED%94%84%EB%A7%81%20%ED%95%B5%EC%8B%AC%20%EC%9B%90%EB%A6%AC/%EB%B9%88%20%EC%8A%A4%EC%BD%94%ED%94%84.md)

--- 

### 자바 ORM 표준 JPA 프로그래밍
- [JPA란?](https://github.com/jinnkimm7/spring-practice/blob/main/JPA/JPA.md)
- [JPA의 구동 방식](https://github.com/jinnkimm7/spring-practice/blob/main/JPA/JPA%20%EA%B5%AC%EB%8F%99%20%EB%B0%A9%EC%8B%9D.md)
- ⭐️ [영속성 컨텍스트](https://github.com/jinnkimm7/spring-practice/blob/main/JPA/JPA%20%EA%B5%AC%EB%8F%99%20%EB%B0%A9%EC%8B%9D.md)
- ⭐️ Object와 RDM를 Mapping하기 (ORM)
  - [엔티티 매핑](https://github.com/jinnkimm7/spring-practice/blob/main/JPA/%EC%97%94%ED%8B%B0%ED%8B%B0%20%EB%A7%A4%ED%95%91.md)
  - [연관관계 매핑 기초](https://github.com/jinnkimm7/spring-practice/blob/main/JPA/%EC%97%B0%EA%B4%80%EA%B4%80%EA%B3%84%20%EB%A7%A4%ED%95%91%20%EA%B8%B0%EC%B4%88.md)
  - [다양한 연관관계 매핑](https://github.com/jinnkimm7/spring-practice/blob/main/JPA/%EB%8B%A4%EC%96%91%ED%95%9C%20%EC%97%B0%EA%B4%80%EA%B4%80%EA%B3%84%20%EB%A7%A4%ED%95%91.md)
  - [상속관계 매핑](https://github.com/jinnkimm7/spring-practice/blob/main/JPA/%EC%83%81%EC%86%8D%EA%B4%80%EA%B3%84%20%EB%A7%A4%ED%95%91.md)
  - [프록시와 연관관계](https://github.com/jinnkimm7/spring-practice/blob/main/JPA/%ED%94%84%EB%A1%9D%EC%8B%9C%EC%99%80%20%EC%97%B0%EA%B4%80%EA%B4%80%EA%B3%84.md)
- [값타입](https://github.com/jinnkimm7/spring-practice/blob/main/JPA/%EA%B0%92%20%ED%83%80%EC%9E%85.md)
- JPQL
  - [JPQL 기본 문법]()
  - [JPQL 중급 문법]()