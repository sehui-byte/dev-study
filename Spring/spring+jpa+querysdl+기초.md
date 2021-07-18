### ORM(object-relational mapping)

- object-relational mapping (객체와 RDB 매핑) → **<u>객체와 RDB의 테이블이 매핑</u>**을 이루는 것.
- 즉, **RDB 테이블을 객체지향적으로 사용하기 위한 기술**이다.



이전에 MyBatis를 쓴 적이 있었는데 이는 SQL Mapper이다. **ORM은 <u>객체를 매핑</u>**하는 것이고, SQL Mapper는 쿼리를 매핑하는 것이다.



> **MyBatis 예제** 
>
> MyBatis의 경우 SQL문이 xml로 작성되어 자바 코드로부터 완전히 분리되고, SQL쿼리를 매핑한다.
>
> (https://github.com/sehui-byte/spring-study/blob/main/ksh/2021-01-12.md)

```java
@Repository
public class LoginDAOImpl implements LoginDAO {

	@Autowired(required=false)
	private SqlSessionTemplate sqlSession;
	
	@Override
	public List<MemberVO> loginCheck(MemberVO mvo) {
		return sqlSession.selectList("loginCheck", mvo);
	}
}
```

**login.xml**

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
			"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="a.b.c.com.login.dao.LoginDAO">
	<select id="loginCheck" parameterType="memberVO"
		resultType="memberVO">
		SELECT A.SMID SMID
		,A.SMPW SMPW
		FROM SPRING_MEMBER A
		WHERE SMID = #{smid, jdbcType=VARCHAR}
		AND SMPW = #{smpw, jdbcType=VARCHAR}
	</select>
</mapper>
```

이렇게 xml로 분리된 sql쿼리문과 매핑되는 방식을 확인할 수 있다.



------------------



### JPA

- JPA (Java Persistence API) : <u>**자바 ORM 기술에 대한 API 표준 명세.**</u>

- <u>JPA는 **단순 명세**</u>이기 때문에 구현이 없고, JPA를 정의한 `javax.persistence`package의 대부분은 `interface`, `enum`, `Exception`, 그리고 각종 `Annotaion`으로 이루어져있다.

- `EntityManager`도 interface로 구현되어있다.

  > Interface used **to interact with the persistence context.**
  > An EntityManager instance is associated with a persistence context. **A persistence context is a set of entity instances in which for any persistent entity identity there is a unique entity instance.** **<u>Within the persistence context, the entity instances and their lifecycle are managed. The EntityManager API is used to create and remove persistent entity instances, to find entities by their primary key, and to query over entities.</u>**
  > The set of entities that can be managed by a given EntityManager instance is defined by a persistence unit. A persistence unit defines the set of all classes that are related or grouped by the application, and which must be colocated in their mapping to a single database.

  ```java
  package javax.persistence;
  
  import ...
  
  public interface EntityManager {
  
      public void persist(Object entity);
  
      public <T> T merge(T entity);
  
      public void remove(Object entity);
  
      public <T> T find(Class<T> entityClass, Object primaryKey);
  
      // More interface methods...
  }
  ```

  

- **Hibernate, OpenJPA 등...은 이 <u>JPA를 구현한 구현체</u>**이다. (JPA )

  

```java
@Getter
@Setter
@NoArgsConstructor
@Entity
@Table(name="STUDENT")
public class Student {
    
    @Id
    private String id;
    private String name;
    private String age;
    
}
```



![JPA, Hibernate, 그리고 Spring Data JPA의 차이점](https://suhwan.dev/images/jpa_hibernate_repository/overall_design.png)

---------



### spring-data-JPA

- Spring Data JPA는 spring에서 제공하는 모듈 중 하나로, 개발자가 JPA를 더 쉽고 편하게 사용할 수 있도록 도와준다.  
- **JPA를 한단계 추상화시킨 `Repository` 인터페이스를 제공**함으로써 이루어진다. 
- **개발자는 `Repository`인터페이스에 정해진 규칙대로 메소드를 입력하면 spring이 알아서 해당 메소드 이름에 적합한 쿼리를 날리는 구현체를 동적으로 생성해서 `bean`으로 등록(주입)**해준다.

```java
package org.springframework.data.repository;

import org.springframework.stereotype.Indexed;

@Indexed
public interface Repository<T, ID> {

}
```



>### 4.1. Core concepts
>
>The **central interface in the Spring Data repository abstraction is `Repository`**. It takes the domain class to manage as well as the ID type of the domain class as type arguments. This interface acts primarily as a marker interface to capture the types to work with and to help you to discover interfaces that extend this one. **The [`CrudRepository`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html) interface provides sophisticated CRUD functionality for the entity class that is being managed.**

>  We also provide persistence technology-specific abstractions, such as `JpaRepository` or `MongoRepository`. Those interfaces extend `CrudRepository` and expose the capabilities of the underlying persistence technology in addition to the rather generic persistence technology-agnostic interfaces such as `CrudRepository`



``` java
public interface CrudRepository<T, ID> extends Repository<T, ID> {

  <S extends T> S save(S entity);      

  Optional<T> findById(ID primaryKey); 

  Iterable<T> findAll();               

  long count();                        

  void delete(T entity);               

  boolean existsById(ID primaryKey);   

  // … more functionality omitted.
}
```



**application.yml**

```yaml
jpa:
    hibernate.ddl-auto: none
    generate-ddl: false
```



#### hibernate.ddl-auto

- `none`
- `create-drop`
- `create`
- `update`
- `validate`



-----



#### queryDSL

- spring data jpa에서 기본으로 제공해주는 쿼리만으로는 **다양한 조회 기능을 사용하기에 한계**가 있어 이 문제를 해결하기 위해 **정적 타입을 지원하는 조회 프레임워크**를 사용한다. 이에 가장 유명한 조회 프레임워크가 바로 `querydsl`이다.

  

----------



### spring data JPA 와 QueryDSL

- **SQL 쿼리를 메소드 기반으로 작성**할 수 있도록 도와주는 프레임워크.
  - JPA를 사용하면서 **복잡한 조회 쿼리의 경우 `QueryDSL`을 사용하면 편리**하다.
  - **컴파일 타임에 문법 오류**를 잡을 수 있고, **IDE의 도움**을 받을 수 있어 편리하다.
- `QueryDSL`로 쿼리문을 작성하기 위해 `Q타입 클래스`(QueryDSL전용 객체, prefix 'Q'가 붙는 클래스들 자동생성) 를 사용한다.
- `JPAQuery` 클래스를 사용하면 `EntityManager`를 통해서 질의가 처리되고, 이때 사용하는 쿼리문은 `JPQL`이다.



아래와 같이 설정을 하면, 해당 프로젝트 어느 곳에서나 `JPAQueryFactory`를 주입받아 `QueryDSL`을 사용할 수 있게 된다.

``` java
@Configuration
public class QuerydslConfig {

    @PersistenceContext
    private EntityManager entityManager;

    @Bean
    public JPAQueryFactory jpaQueryFactory() {
        return new JPAQueryFactory(entityManager);
    }
}

```



```
JPAQuery queryDsl = new JPAQuery(entityManager);
```





#### QuerySDL - dynamic query

- **BooleanBuilder**
  - 조건들을 이어붙여서 `where`에 넣어주는 방식.
  - **쿼리의 형태를 예측하기 어렵다**는 단점이 있다.

```java
public List<Student> findDynamicQuery(String name, String age){
    BooleanBuilder builder = new BooleanBuilder();
    
    if(!StringUtils.isEmpty(name)) {
        builder.and(student.name.eq(name));
    }

    if(!StringUtils.isEmpty(age)) {
        builder.and(student.age.eq(age));
    }	
    return queryFactory.selectFrom(student).where(builder).fetch();
}

```



- **BooleanExpression**

  - `Querydsl`의 `where`은 `null`이 파라미터로 올 경우 조건문에서 제외한다.

    

아래 코드에서 만약 `age` 값이 없으면 해당 조건 제외.

```java
public List<Student> findDynamicQueryAdvance(String name, String age) {
	return queryFactory
		.selectFrom(student)
		.where(eqName(name),
				eqAge(age))
		.fetch();
}

private BooleanExpression eqName(String name) {
	return StringUtils.isEmpty(name) ?  null : student.name.eq(name);
}

private BooleanExpression eqAge(String age) {
	return StringUtils.isEmpty(age) ?  null : student.name.eq(age);
}
```





----

### 참고자료

- https://suhwan.dev/2019/02/24/jpa-vs-hibernate-vs-spring-data-jpa/

- https://goddaehee.tistory.com/209

- [lombok 기능정리-getter, setter, MoArgsConstructor, RequiredArgsConstructor, AllArgsConstructor](https://dingue.tistory.com/14)

- https://ict-nroo.tistory.com/117

- http://ojc.asia/bbs/board.php?bo_table=LecJpa&wr_id=341

- https://perfectacle.github.io/2018/01/14/jpa-entity-manager-factory/

- **[[Querydsl] 다이나믹 쿼리 사용하기](https://jojoldu.tistory.com/394)**

- https://velog.io/@aidenshin/Querydsl-%EB%8F%99%EC%A0%81-%EC%BF%BC%EB%A6%AC

- [spring jpa repository](https://araikuma.tistory.com/329)

- **[jpa, hibernates, spring data jpa차이점](https://suhwan.dev/2019/02/24/jpa-vs-hibernate-vs-spring-data-jpa/)**

- https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repositories

- [spring boot data jpa프로젝트에 querydsl적용하기](https://jojoldu.tistory.com/372)