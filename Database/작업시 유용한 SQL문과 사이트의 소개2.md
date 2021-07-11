###### 이번주의 주제는 현업에서 디비 작업시 유용한 SQL문과 사이트의 소개입니다.

###### 해당 이미지들은 DBeaver의 화면이므로 1주차 소개를 참고해주시면 감사하겠습니다.

###### 



###### 주제 목차는 다음과 같습니다.

###### 1.explain 기능

###### 2.case when 기능

###### 3.프로시저의 개념 소개 및 기능

###### 4.sql 정렬 사이트 소개 및 기능



* 1.explain 기능

  - 1-1)SQL  문 작성시 select 앞에 explain 이라고 작성하면 해당 쿼리문의 실행 순서를 알려줍니다.

    - 실행 순서란 쿼리문을 조회하기 위한 조건 filter 및 reading 순서를 의미합니다.

    ![](https://user-images.githubusercontent.com/77149239/118398118-25c3c700-b692-11eb-86b9-27480ec9a9cf.PNG)

    - 위와 같은 형태로 작성하면 되고, 아래의 결과창에 Query Plan 이라고 하여 "이 순서대로 쿼리를 읽어나갈 것이다" 를 알려줍니다.

      

  - 1-2)다음은 explain 보다는 더 좋은 기능을 하는 explain analyse 기능입니다.

    ![](https://user-images.githubusercontent.com/77149239/118398121-278d8a80-b692-11eb-8643-cec5bd9cbbf1.PNG)

    * 위의 explain 과는 다르게 실행까지 하여 execution 시간까지 출력해줍니다. 1번의 플랜은 동일하게 출력을 해줍니다.
    * 오라클은 plan_table 이라고 하여 해당 내용을 저장해주는 테이블이 있습니다. 오라클 10g 버전부터는 기본적으로 제공을 해주기 때문에 따로 생성하지 않아도 되지만, 미만 버전에서는 생성을 해주어야 합니다. 방법은 어렵지 않고 구글에서 쉽게 검색하실 수 있습니다.
    * 해당 내용은 관리하기 힘든 쿼리문 같은 것을 분석하여 따로 정리해두면 좋은 기능이라고 생각합니다.
    * 위의 analyse 내용은 postgreSql 에서의 기능이고, 같은 기능을 하는 명령어는 Oracle 에서는 explain plan  입니다. explain 자체는 동일합니다.



* 2.case when then else end 구문

  * 기본적인 구조는 case when 'column1 의 조건'  then 'column1 을 이 조건으로 선언'  else '다른 조건으로 선언' end '최종 컬럼명을 선언하는 곳' 입니다.

    * 말로 다시 풀어쓰면, ""컬럼1이 when  이하일 경우 then  이하로 쓰고 아니라면 else 이 조건으로 표기하되 컬럼명은 end 이하로 명명합니다 " 가 되겠습니다.

    ![](https://user-images.githubusercontent.com/77149239/118398127-2bb9a800-b692-11eb-8432-a7305b1f5d55.PNG)

    * 위의 이미지는 podStatuscd 라는 컬럼이 001~004 값이며 sum = sum 조건이라면 true 로 표기하고, 그렇지 않으면 false 로 표기하며 컬럼명 표기는 QTY_CHECK 로 한다는 내용의 쿼리입니다.



* 3.프로시저란?

  * 프로시저는 plsql 이라는 sql 확장문 기능을 하는 언어입니다.

    * plsql 이란 "Oracle's Procedural Language extension to SQL" 이라고도 불리며 SQL의 확장된 개념으로써 Oracle 에서 지원하는 프로그래밍 언어의 특성을 포함한 SQL 확장문 형태입니다.

      해당 SQL은 블록 구간별로 구조를 이루고 있고, 각 구간마다 DML(데이터 조작 명령어), Query(검색문), If 와 loop 등을 순서대로 사용하여 절차적 프로그래밍이 구현되는 트랜잭션 언어의 기능을 하는 것으로 알려져 있습니다.

      

  * 3-1)프로시저의 구성

    * 프로시저의 구성 흐름은 크게 선언부 -> 실행부의 begin ~ end -> 예외처리 부분으로 나뉩니다.
      각 구간부의 기능은 다음과 같습니다.
    * 선언부 : 모든 변수나 상수를 선언하는 부분인 Declare 파트
    * 실행부 : begin ~ end 구간에 해당하는 제어, 반복, 함수 등의 로직을 구성하는 Execution 파트
    * 예외부 : 실행 도중에 에러 발생시 해결을 위한 명령들을 기술하는 Exception 파트
    * 단 작성시 유의할 점으로, Declare, begin, exception 키워드들은 ;을 붙이지 않고, 나머지 문장들은 ;으로 처리해야 하는 규칙이 존재합니다.

  

  * 3-2)프로시저의 구성 예시
    * 프로시저의 실제 구성 화면은 다음과 같습니다.
    * ![](https://user-images.githubusercontent.com/77149239/118398103-193f6e80-b692-11eb-9e9e-c4bae489e5a9.PNG)
    * 첫번째 이미지 입니다. Declare 선언부에 사용하고자 하는 변수 혹은 상수를 선언했습니다.
    * ![](https://user-images.githubusercontent.com/77149239/118398107-1d6b8c00-b692-11eb-89d7-51f1aeebf6b2.PNG)
    * ![](https://user-images.githubusercontent.com/77149239/118398109-1f354f80-b692-11eb-841e-1f8f685cc5cf.PNG)
    * ![](https://user-images.githubusercontent.com/77149239/118398112-2197a980-b692-11eb-8ddd-df096a702af2.PNG)
    * 실행부인 begin ~ end 구간입니다. 실제 로직이 돌기 위한 쿼리문과 조건 등이 입력되고 실행되는 파트라고 생각하시면 됩니다.
    * 예외처리부는 해당 프로시저 실행문에서는 없었기 때문에 소개하지는 않았습니다.

  

  * 3-3)프로시저를 생성하는 방법

    * ex)	CREATE OR REPLACE PROCEDURE EX_PROC
        				(
        				   P_DEPARTMENT IN VARCHAR2,  -- 문자형 데이터
        				   P_STUDENT_CNT IN NUMBER    -- 숫자형 데이터
        				)
        				IS
        				P_UNIVERSITY VARCHAR2(100)  := '서울대학교';

      			BEGIN
      	
      			INSERT INTO UNIVERSITY1 (UNIVERSITY, DEPARTMENT, STUDENT_CNT)
      			VALUES (P_UNIVERSITY, P_DEPARTMENT, P_STUDENT_CNT);
      			COMMIT;
      	
      			END EX_PROC;

    * 위와 같은 형태로 파라미터로 받을 값을 프로시저 이름인 EX_PROC () 안에다 선언해주고, 변수를 받을 내용이 있으면 is 뒤에 선언해주면됩니다. begin end 부분에 실행할 쿼리문을 입력해줍니다.

    * end 구간 뒤에는 프로시저 이름을 적어주고 ; 마침표를 찍습니다. 해당 사항은 3-1 프로시저의 구성 내용중 4번째 세부항목을 참고해주세요.

      

  * 3-4)프로시저의 실행 방법

    * 위의 예시를 들어서 EXEC EX_PROC('서울대','물리학과',500); 라고 입력후 실행을 시키고 테이블 조회시 데이터가 출력됩니다. 괄호 안은 위에서 생성한 예시로 프로시저의 파라미터를 선언한 컬럼의 데이터 타입에 맞는 값을 선언한 것입니다.
    * ![](https://user-images.githubusercontent.com/77149239/118398918-9f10e900-b695-11eb-8b24-6e5be51f219c.png)
    * 조회시 출력이 이런 형태로 나오게됩니다.

    

  * 3-5)프로시저의 장점

    * 한번 SQL을 실행하여 다량의 쿼리문을 수행하여 데이터를 읽어낼 수 있습니다.
    * 테이블의 데이터 구조와 Database의 컬럼 특성을 준수하여 동적으로 변수를 선언 할 수 있습니다.
    * 사용자 정의 예외처리를 이용할 수 있습니다.
    * 처리부분은 자신이 하고싶은대로 작성하면 됩니다. 기본적으로 자바에서 많이들 익혔던  exception 처리랑 비슷하게 흐릅니다.

    

  * 3-6프로시저의 단점

    * sql의 확장언어 이기도 하고, 블록구간 별로 다양한 변수랑 쿼리문의 조건이 대량 삽입되어 현업에서 쓰이는 경우가 많습니다.	

      따라서 쿼리가 무거운 것을 쓰면 매우 느려질 위험도 있고, 유지 및 보수에 이용할 때 SM 파트 작업에서 수정 개선 등의 작업이 복잡하고 어려운 측면이 있습니다.



* 4.쿼리문 정렬 사이트
  * SQL 문을 작성할때 복잡한 로직을 다루거나 할 때 보기 좋게 정렬해주는 사이트가 있습니다.
  * https://www.dpriver.com/pp/sqlformat.htm
    * 사이트의 기능으로는 쿼리문을 복사해서 붙여주고 밑에있는 Format SQL 버튼을 누르면 자동으로 정렬을 해줍니다.
    * ![](https://user-images.githubusercontent.com/77149239/118398173-63285480-b692-11eb-94b2-76614c339b0e.PNG)
    * 위의 내용처럼 정렬이 된 모습을 확인할 수 있고, 아웃풋의 기능으로는 자신이 보고싶은 형태의 값으로 출력해주는 기능도 있습니다.
    * 또한 우측의 대소문자 구분도 정해서 출력 해주는 기능도 있습니다.
    * ![](https://user-images.githubusercontent.com/77149239/118398185-6de2e980-b692-11eb-87c5-2feed6d542bd.PNG)
    * 아웃풋 콤보박스에서 Java String Buffer 로 지정하고 출력을 누른 결과입니다.
