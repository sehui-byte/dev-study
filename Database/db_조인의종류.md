/*
조인의 3가지 방법
1.INNER JOIN
: 벤 다이어그램 상으로 표현하자면, 교집합에 해당한다.
  테이블상의 컬럼이 같은 것을 표기하여 조인으로 끌어온다.
  INNER는 생략이 가능함.

2.LEFT JOIN, RIGHT JOIN

3.OUTER JOIN 

4.다중 조인 (3개 이상의 테이블)

*/

-- 들어가기에 앞서 ANSI JOIN 을 알아보도록 합니다.
-- ANSI JOIN 이란 기존의 SELECT * FROM TABLE TYPE ALIAS 1, TABLE TYPE ALIAS 2 WHERE 형태의 JOIN 에서
-- 국제표준 조인 쿼리인 SELECT * FROM TABLE TYPE ALIAS 1 JOIN(종류) TABLE TYPE ALIAS 2 ON WHERE 조건문의 형태로
-- DBMS 간의 문법적 상이함을 고려해 만들어진 조인입니다.
-- 3번째 행의 JOIN 종류라고 말을 하는 것은 INNER JOIN 기본을 토대로 외부조인 종류인 (LEFT JOIN, RIGHT JOIN, OUTER JOIN 등이 해당합니다.)

-- 1)INNER JOIN

```sql
SELECT * FROM KOSMOMEMBER; -- KNAME 을 조인조건으로. A
SELECT * FROM KOSMEMLIST; -- B
```



-- ON 조건을 기반으로 모든 컬럼을 조회하는 것 : SELECT * FROM 일 경우
-- 한쪽 테이블의 컬럼만 조회하는 경우 : A.* 같이 스캔범위를 지정할 경우
-- ANSI 조인 방법
SELECT B.* FROM KOSMOMEMBER A INNER JOIN KOSMEMLIST B
ON A.KNAME = B.KNAME

-- 조인 컬럼 전체 조회인 * 이고 INNER 생략
SELECT * FROM KOSMOMEMBER A JOIN KOSMEMLIST B
ON A.KNAME = B.KNAME

-- 기존의 INNER JOIN 형태
SELECT A.*, B.* FROM KOSMEMLIST A, KOSMOMEMBER B
WHERE A.KNAME = B.KNAME


-- 2)LEFT JOIN 과 RIGHT JOIN

-- 날짜를 기점으로 불러오기.
SELECT * FROM KOSMOBOARD;
SELECT * FROM KOSMOMEMBER;

-- 2-1)
-- LEFT JOIN : 왼쪽에 조인을 위치시킨 테이블을 기점으로 조건을 조회하여 데이터를 뽑은 후 
--              오른편의 테이블에 데이터를 넣는 과정에서 조건이 일치하지 않으면 NULL을 대신 삽입시키는 조인.

-- 왼쪽의 테이블의 기준대로 ROW를 읽어서 조인 과정을 진행함.
-- 모수 데이터가 많고 적고는 연관이 없는것 같음.
SELECT * FROM 
KOSMOBOARD A LEFT JOIN KOSMOMEMBER B 
ON A.KINSERTDATE = B.KINSERTDATE 
-- ROW의 결과 개수가 위랑 다름.
SELECT * FROM
KOSMOMEMBER A LEFT JOIN KOSMOBOARD B
ON A.KINSERTDATE = B.KINSERTDATE;


-- 2-2)
-- RIGHT JOIN : 오른편의 테이블을 기준으로 ROW를 조회하여 데이터를 읽고 값을 출력해줌.
-- 마찬가지로 다음 테이블 조회시 조건에 부합되는 컬럼이면 NULL을 대신 삽입함.
-- 역시 모수 데이터는 상관이 없는것 같음.
SELECT * FROM KOSMOBOARD;
SELECT * FROM KOSMOMEMBER;

SELECT * FROM
KOSMOBOARD A RIGHT JOIN KOSMOMEMBER B
ON A.KINSERTDATE = B.KINSERTDATE; 

-- ROW의 결과 개수가 위랑 다름.
SELECT * FROM 
KOSMOMEMBER A RIGHT JOIN KOSMOBOARD B
ON A.KINSERTDATE = B.KINSERTDATE;

SELECT * FROM KOSMEMLIST;
SELECT * FROM KOSMOMEMBER;

-- 3)아우터 조인
-- 아우터 조인 : 데이터가 없을 수도 있을 경우의 테이블 컬럼에 (+) 라는 표기를 하여
-- ROW 전체를 합하여 반환해준다.
-- 쓰는 목적은 주로 없을 데이터를 보기위한 것으로 알려져있다.
-- 해당 (+) 는 오라클 전용 아우터 조인의 표기법이다.
SELECT A.*, B.* 
FROM KOSMEMLIST A, KOSMOMEMBER B
WHERE A.KNAME = B.KNAME(+)

-- 3-2)아우터 조인 ANSI JOIN 형태
-- OUTER 방식 앞에 기준으로 삼을 테이블을 지정하여 읽을 방향을 선택해주어야 함.
-- LEFT 이거나 RIGHT 를 명시하여 작성함.
SELECT A.*, B.*
FROM KOSMEMLIST A LEFT OUTER JOIN KOSMOMEMBER B
    ON A.KNAME = B.KNAME


-- 4)다중 조인
-- 다중 조인 예시
-- 다중조인을 보게되는 경우는 실무에서의 데이터 인터페이스 과정에서의 
-- 테이블 조회를 여러개를 거치는 경우가 있습니다. 이 부분에서 종종 쓰입니다.

SELECT * FROM KOSMEMLIST; -- A 
                                --와 B 는 KINSERTDATE , K UPDATEDATE
SELECT * FROM KOSMOMEMBER; -- B 
                                -- 와 C 는 KLOCAL        
SELECT * FROM KOSMOBOARD; -- C

-- 3가지의 테이블로 구성된 다중조인
SELECT *
FROM KOSMEMLIST A JOIN KOSMOMEMBER B
    ON A.KNAME = B.KNAME 
INNER JOIN KOSMOBOARD C
    ON B.KINSERTDATE = C.KINSERTDATE
WHERE C.KPW LIKE '%11%' -- 11 이란 것이 데이터에 포함되어 있을 경우.
