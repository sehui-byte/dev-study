###### 6월 2주차

###### 금주의 주제 : 서브쿼리의 종류



/*
6월 2주차 - 서브쿼리


-  1)단일 행 서브쿼리
사용법 : 서브쿼리의 문장에서 하나의 행과 열을 검색할 때 씁니다.
의미 : 실행결과가 항상 1건이하인 서브쿼리를 의미합니다. > 서브쿼리의 결과가 2건 이상이면 런타임 에러 납니다.
문법 : 단일 행 연산자를 사용하여 서브쿼리 오른쪽에 기술합니다. > (=, <, >, <=, >=, !=)

2)다중 행 서브쿼리
사용법 : 서브쿼리의 결과로 2건 이상 가져올 때 '단일 행 서브쿼리' 는 비교 연산자 등 만으로는 처리가
        불가능 하기 때문에 에러가 나옵니다. 이 때는 '다중 행 서브쿼리'에 맞는 비교 연산자를 사용합니다.
        
의미 : 연산자의 차이를 제외하면 단일 행 조회 서브쿼리와 구조는 같습니다.

문법 :  >> IN(서브쿼리) : 서브쿼리의 결과에 존재하는 임의의 값과 동일한 조건으로, 모든 결과값에 대해 
                        일치하는 값이 있는지 체크합니다.
       >> ANY() : 서브쿼리의 결과에 존재하는 어느 하나의 값이라도 있으면 불러옵니다.
       >> ALL() : 서브쿼리의 결과에 모든 값을 만족하는 조건입니다. 여기서 주의할 점은 ANY와 ALL은 IN 연산자랑은 달리
                  특정한 값이 아닌 범위로 비교연산을 처리합니다.
                   
3)다중 열, 컬럼 서브쿼리
사용법 : 서브쿼리의 결과로 여러 개의 컬럼이 반환되어 메인 쿼리의 조건과 동시에 비교를 하는 방식으로, 반드시 비교 대상 컬럼과
        1:1 대응이 되어야 합니다. 

문법 : '='도 사용 가능하지만 주로 IN을 사용합니다.
        
4)연관 서브쿼리
사용법 : EXISTS 서브쿼리는 항상 연관 서브쿼리로 수행됩니다. 조건을 만족하는 건이여도 1개만 찾으면 더 이상 찾아오지 않습니다.
의미 : 메인 쿼리의 컬럼이 서브쿼리 안에 사용된 형태를 의미합니다.

5)스칼라 서브쿼리(SELECT 절에다 사용하는 쿼리)
사용법 : SELECT 안에 서브쿼리의 구현을 합니다.
의미 : 한 행, 한 컬럼만 반환하는 서브쿼리를 의미합니다.(SELECT 안에 존재)

6)인라인 뷰(FROM 절에다 사용하는 쿼리)
사용법 : 서브쿼리의 결과가 실행할 때 동적으로 생성된 테이블인 것처럼 사용이 가능합니다.
의미 : SQL문이 실행될 때만 임시적으로 생성되는 동적인 뷰, 일반적인 뷰를 정적인 뷰, 인라인 뷰를 동적인 뷰라고 칭합니다.
문법 : 일반적인으로 메인 쿼리보다 먼저 수행되므로 SQL 문장 내에서 절차적인 의미를 부여하도록 

7)HAVING 절에다 사용하는 서브쿼리
사욛법 : HAVING 절이 GOURP BY 와 같이 쓰이므로 그룹핑된 결과에 부가 조건을 줍니다.
의미 : 부가적인 조건을 건 그룹핑 기능입니다.
문법 : SQL 문의 맨 끝에 나타나는 HAVING < 또는 > 연산 이후에 괄호로 서브쿼리를 묶어서 기술합니다.

*/

-- 서브쿼리 하기 위한 임시 테이블 작성

DROP TABLE RECEIPT;
CREATE TABLE RECEIPT
(  MENUDATE           VARCHAR2(100)     PRIMARY KEY
  ,PRICE              VARCHAR2(100)
  ,CUSNAME            VARCHAR2(100)  
  ,CUSHIS             VARCHAR2(100)
        );
        
-- 서브쿼리 수행하기 위한 임시 데이터 삽입.
INSERT INTO RECEIPT
(MENUDATE, PRICE, CUSNAME, CUSHIS)
VALUES
('210522', '12000', 'CJS', '10');   -- 

-- 데이터 확인 후
SELECT * FROM RECEIPT;

--1)단일 행 서브쿼리.
SELECT * FROM KOSMENU
WHERE MENUDATE = (SELECT MENUDATE FROM RECEIPT) --() 안의 서브쿼리가 1건만 가져오므로 단일 행 서브쿼리에 해당합니다.

--2)다중 행 서브쿼리.
SELECT * FROM KOSMENU
WHERE MENUDATE IN (SELECT MENUDATE FROM RECEIPT) -- IN 연산자를 사용하면 조건에 걸리는 데이터 모든 행을 가져옵니다.

SELECT MENU1, MENU2, MENU3 FROM KOSMENU 
WHERE MENUDATE < ANY (SELECT MENUDATE FROM RECEIPT WHERE PRICE > 10000) -- 서브쿼리 안의 조건을 대상을 범위로 스캔하여 다중 행을 가져옵니다.

SELECT MENU1, MENU2, MENU3 FROM KOSMENU
WHERE MENUDATE > ALL (SELECT MENUDATE FROM RECEIPT WHERE CUSNAME LIKE 'BB') -- 하위 조건의 범위를 대상으로 하여 다중 행을 가져오지만,
                                                                            -- 반환할 값이 조건 범위의 행 보다 많으면 나머지 값들은 NULL을 반환합니다. 
-- >> ANY 랑 ALL은 조금 다릅니다.


-- 3)다중 열, 컬럼 서브쿼리
SELECT * FROM KOSMENU
WHERE (COLUMN1, COLUMN2) IN (SELECT COLUM1, COLUMN2 FROM TABLENAME); --의 형태로 서브쿼리를 짤 수 있습니다.
                                                                     --비교할 컬럼이 조건과 반환할 메인쿼리 모두 여러개임을 의미하는 서브쿼리입니다.
                                                     
-- 4)연관 서브쿼리
SELECT ENAME, DEPTNO, SAL
    FROM EMP A
    WHERE SAL > (SELECT AVG(SAL) FROM EMP B
                    WHERE B.DEPTNO = A.DEPTNO)
    ORDER BY DEPTNO;    -- 조건절의 서브쿼리에 메인쿼리로 지정한 타입.컬럼이 들어가 있으므로 연관 서브쿼리가 됩니다.
                        -- 위에서 기술한대로 메인쿼리의 컬럼을 서브쿼리 안에서 사용하기 때문입니다.

-- 5) 스칼라 서브쿼리
SELECT ENAME, (SELECT DNAME FROM DEPT D
                WHERE D.DEPTNO = E.DEPTNO) AS NAME, JOB -- 서브쿼리 결과값이 NAME 이라는 컬럼으로 반환.
    FROM EMP E
    WHERE JOB = 'MANAGER';  -- 스칼라 서브쿼리는 서브쿼리 자체를 컬럼 조회로 가져오는 형태가 됩니다.
                            -- SQL 문에서 단일값을 스칼라 값이라고 하는데, 스칼라 쿼리는 1행만 결과로 줍니다.
                            
-- 6) 인라인 뷰
SELECT ROWNUM, ENAME, SAL FROM EMP WHERE ROWNUM > 5;
-->실행하면 아무런 레코드도 검색되지 않습니다. 이유는 어떤 조건이 와도 시작 ROWNUM은 1부터 시작이 되어야 하기 때문입니다.
--> 따라서 위와 같은 한계를 극복하기 위해서 몇 가지 방법이 있는데 그중 하나인 인라인 뷰라는 것을 이용하여 해결이 가능합니다.

SELECT ROWNUM, EMPNO, ENAME, HIREDATE FROM
(SELECT * FROM EMP ORDER BY HIREDATE DESC)
WHERE ROWNUM <= 5;
--> 인라인 뷰를 통해 새로운 테이블을 일시적으로 생성하고, 인라인 뷰에 대해서 ROWNUM을 이용하여 쿼리문을 실행하면 원하는 결과를 검색이 가능합니다.
--> (인라인 뷰에 의해 ROWNUM이 1부터 새로 부여됨)

SELECT ROWNUM, RNUM, ENAME, SAL FROM
(SELECT ROWNUM RNUM, ENAME, SAL FROM -- FROM 절의 INLINE VIEW ROWNUM에 대해 별칭 부여
(SELECT * FROM EMP ORDER BY SAL DESC))
WHERE RNUM BETWEEN 6 AND 10;
--> 인라인 뷰 안에 조건을 또 달고싶으면 별칭을 부여합니다.
--> 인라인 뷰 안의 인라인 뷰를 작성하고 만드는 것이 규칙이며, 별칭부여의 이유는 컬럼 구분자를 위함입니다.
--> ROWNUM 2번 생성시 어느 것이 한 번 더 고려하는 조건의 컬럼인지 구분하기 위한 별칭 RNUM 부여입니다.

--7)HAVING 절
SELECT column1, SUM(column2)
    FROM table1
    GROUP BY column1
    HAVING SUM(column2) > (SELECT SUM(column2)
                            FROM table1
                            WHERE column1="KOREA");
-- : column1, column2를 column1로 묶어 출력하되, column1이 KOREA일 때의 column2 보다 큰 경우만 출력하는 쿼리입니다.                           
-- * 그룹함수의 조건은 HAVING절 사용합니다.