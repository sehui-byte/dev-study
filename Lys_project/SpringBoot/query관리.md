# JPA

## Repository CRUD
- **INSERT** : save
- **SELECT** : findById, getOne
 - findById() : 실행 순간에 SQL문 처리 됨
 - getOne() : 실제 객체를 사용하는 순간에 SQL이 동작
- **UPDATE** : save
- **DELETE** : deleteById, delete 


## SpringBoot specification
springBoot 는 jpa로 query 관리해서 개발자가 쿼리 관리x
기본적인 쿼리말고 조건 걸거나 할 때 specification 이용한다.
![Untitled](https://user-images.githubusercontent.com/78526031/121526324-3a4a7380-ca34-11eb-957e-a4a56b8841b5.png)


이거 말고는 @Query 이용하는 방법이 있다 


## @Query 어노테이션

@Query 의 value는 JPQL(Java Persistence Query Language) 로 작성 '객체 지향 쿼리'로 불림

파라미터 바인딩 →

- '?1, ?2' 와 1부터 시작하는 파라미터의 순서를 이용하는 방식
- ':xxx' 처럼 ':파라미터 이름' 을 활용하는 방식
- ':#{}' 와 같이 자바 빈 스타일을 활용하는 방식

Native SQL  → nativeQuery = true 지정하면 일반 SQL 사용가능


Querydsl도 있다는데 이건 뭔지 찾아봐야 할 듯

