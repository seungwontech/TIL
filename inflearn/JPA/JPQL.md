# JPQL 소개

- JPQL은 객체지향 쿼리 언어다.
- 따라서 테이블을 대상으로 쿼리하는 것이 아니라 엔티티 객체를 대상으로 쿼리한다.
- JPQL은 SQL을 추상화해서 특정 데이터베이스 SQL에 의존하지 않는다.
- JPQL은 결국 SQL로 변환된다.

## 결과 조회 API

- query.getResultList()
    - 결과가 하나이상일 때 리스트 변환
    - 결과가 없으면 빈 리스트 반환
- query.getSingleResult()
    - 결과가 정확히 하나, 단일 객체 반환
    - 결과가 없으면: javax.per~.NoResultException
    - 둘 이상이면: javax.per~.NonUniqueResultException

## 프로젝션

- select 절에 조회할 대상을 지정하는 것
- 프로젝션 대상: 엔티티, 임베디드 타입, 스칼라 타입(숫자, 문자등 기본 데이터 타입)
- select m from member m -> 엔티티 프로젝션
- select m.team from member m -> 엔티티 프로젝션
- select m.address from member m -> 임베디드 타입 프로젝션
- select m.username, m.age from member m -> 스칼라 타입 프로젝션
- Distinct로 중복 제거

## 페이징 API

- JPA는 페이징을 다음 두 API로 추상화
- setFirstResult(int startPosition): 조회 시작 위치
- setMaxResults(int maxResult): 조회할 데이터 수
- 