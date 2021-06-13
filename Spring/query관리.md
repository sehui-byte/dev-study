# JPA

**ORM** : Application과 Database를 자동으로 Mapping 시켜줌

**JPA** : ORM 사용 위한 인터페이스를 모아 놓은 것

**Hibernate** : JPA 실제 구현 클래스 모아놓은 것

**spring data Jpa** : 그 중 자주 쓰는 거

- JPA를 통해서 관리하게 되는 객체 → 엔티티 객체
- Repository → 엔티티 객체를 처리하는 기능 가짐

```java
package org.zerock.ex2.entity;

import lombok.*;
import javax.persistence.*;

@Entity
@Table(name="tbl_memo") // 테이블 정보 담는 어노테이션
// 테이블 이름 지정하지 않은경우 CLASS 이름과 동일한 이름으로 테이블 생성됨
@ToString
@Getter
@Builder // 객체 생성
// @Builder 사용 시  @AllArgsConstructor와 @NoArgsConstructor를
// 항상 같이 처리해야 컴파일 에러가 발생하지 않는다
@AllArgsConstructor
@NoArgsConstructor // Default 생성자
public class Memo {
    @Id // PK에 해당하는 필드 지정
    // ID가 사용자가 입력하는 값을 사용하는 것이 아니면
		//자동으로 생성되는 번호 사용하기 위해 @GeneratedValue 사용
    @GeneratedValue(strategy = GenerationType.IDENTITY) // PK 자동생성 위해 사용
    private Long mno;

    @Column(length = 200, nullable = false)
    private String memoTex
    // @Column(columnDefinition = "varchar(255) default 'Yes' ") 기본값 지정 가능

}
```

(pk 여러개 설정시에는 pk담는 클래스 따로 만들어서 implements Serializable하기)

```java
package org.zerock.ex2.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.zerock.ex2.entity.Memo;

public interface MemoRepository extends JpaRepository<Memo, Long> {
    // JpaRepository 를 상속받는 것으로 작업 끝남 JpaRepository<클래스타입, @Id타입>
}
```

여기에 컨트롤러랑 화면단은 평소처럼 만들면 됨

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

Native SQL → nativeQuery = true 지정하면 일반 SQL 사용가능

Querydsl도 있다는데 이건 뭔지 찾아봐야 할 듯

### Query Max+1말고 Sequence 사용하는 이유

Max+1은 동시에 입력되었을때 중복됨
Sequence는 중복처리되지않음
