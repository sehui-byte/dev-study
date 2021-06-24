###### 6월 3주차

###### 금주의 주제 : 내장함수 



/*
오라클의 내장함수1 - : 문자형 함수1

문자형 함수란?
: 특정 데이터 입력을 받아 문자 데이터 값을 반환하는 함수입니다.
: 문자형 함수의 경우 특히나 많이 사용되기 때문에 잘 기억해두는 것이 중요합니다.
: 그럼 종류에 대해 알아보겠습니다.
  각 함수들 괄호 안에 들어가는 수식은 함수를 쓸 때 사용하는 형식입니다.

1)INITCAP(데이터), LOWER(데이터), UPPER(데이터)
: 이 세 함수는 알파벳과 관련된 함수들입니다.
  INITCAP은 데이터의 첫글자를 대문자로 변환하고 나머지는 소문자로 변환합니다.
  LOWER은 데이터의 모든 알파벳을 소문자로,
  UPPER은 데이터의 모든 알파벳을 대문자로 바꿔줍니다,

2)CONCAT(데이터,데이터)
: 데이터로 입력되는 두 값을 연결하는 함수입니다. 데이터 - 데이터 간의 연결을 위한 함수입니다.

3)SUBSTR(데이터,커서위치,길이)
: CONCATE 함수와 반대로 문자열을 잘라내는 함수입니다.
  SUBSTR은 커서의 위치로부터 길이만큼 문자를 분리하는 함수로써 사용되고,
  SUBSTRB는 커서 위치로부터 길이만큼 바이트를 분리하는 함수입니다.

4)LPAD(데이터,문자열 길이,채워주는 문자), RPAD(데이터,문자열 길이, 채워주는 문자)
: LPAD와 RPAD 함수는 비슷한 기능을 가진 서로 정반대의 함수입니다.
  L은 입력한 데이터를 문자열 길이만큼 늘려서 반환하고, 만약 문자열 길이보다 짧으면
  채워주는 문자를 통해 채워주는 함수입니다.

: RPAD는 동잃한 기능이고 오른쪽에서 시작합니다.

5)LTRIM(데이터, 자를 문자열), RTRIM(데이터, 자를 문자열)
: LTRIM와 RTRIM 함수는 각각 왼쪽 시작점, 오른쪽 시작점에서 자를 문자열을 입력받아
  자른 후 반환하는 함수입니다.

6)REPLACE(데이터, 변경하고 싶은 문자열, 변경할 문자열)
: 입력한 데이터 중 일부의 문자열을 다른 문자열로 변경하여 반환하는 함수입니다.

7)TRANSLATE(데이터, 변경하고 싶은 문자열, 변경할 문자열)
: TRANSLATE 함수는 REPLACE 함수와 비슷하지만 다른 부분이 있습니다.

: REPLACE 함수처럼 데이터에서 특정 문자열을 변경하지만, 변경하고 싶은 문자열과 변경할 문자열
  에서 대응되는 대로 변경이 됩니다.
*/

ALTER TABLE EMP ADD ROLE VARCHAR(100) DEFAULT '-' NOT NULL;


SELECT * FROM EMP;
-- 실습 1번)
SELECT INITCAP('SALESMAN'),
        LOWER('SALESMAN'),
        UPPER('SALESMAN')
    FROM EMP;

--실습 2번)
SELECT CONCAT('DEPTNO','ENAME') AS DEPNAME
FROM EMP;
    
-- 실습 3번
SELECT  SUBSTR('PERSIDENT' ,4,2),
                SUBSTR('EMPTEST',3,2),
                SUBSTR('EMPTEST',3, 1),
                SUBSTRB('SALESMERSON',4,1)
FROM EMP;
        
-- 실습 4번)
SELECT LPAD('SALESPERSON',7,'B'),
        RPAD('SALESPERSON',5,'A')
FROM EMP;

SELECT * FROM EMP;

-- 실습 5번)
SELECT      LTRIM('SALESPERSON','SALES'),
            LTRIM('ENAME','E'),
            LTRIM('SALESPERSON','PERSON'),
            LTRIM('HIREDATE','HIRE')
FROM EMP;

SELECT * FROM EMP;

SELECT      RTRIM('ROLE','LE'),
            LTRIM('ENAME','ME'),
            LTRIM('SALESPERSON','PERSON'),
            LTRIM('HIREDATE','DATE')
FROM EMP;

-- 실습 6번)
SELECT REPLACE('SALESPERNON','SALES','MAN') AS JOB FROM EMP;

-- 실습 7번)
SELECT TRANSLATE('SALESPERSON','SALES','ELSPERSON') AS DEPARTMENT FROM EMP;

-- <다음의 내용>--
-- TRUNCATE : 내용만 삭제
-- REPLACE : 문자열 치환
-- INSTR : 검색

-- TRIM : 공백 제거





/*
오라클 내장함수 - 문자열 함수2

-- **모든 문자열의 치환 함수 사용의 SELECT 는 데이터 추출의 진행시 가공처리에 주로 쓰입니다.**
내용
-- TRUNCATE : 일반적인 DROP TABLE은 테이블 존재까지 지우는 것이지만, TRUNCATE 는 ROW값만 지웁니다.
-- REPLACE : 문자열 치환 : 해당 필드 값을 원하는 값으로 바꾸어줍니다.
-- INSTR : 찾고자 하는 문자의 위치를 반환합니다.
-- TRIM : 제거

*/

/*
TRUNCATE 사용법
TRUNCATE TABLE TABLENAME;
*/
TRUNCATE TABLE EMP; -- ROW 값 다 날리기. 테이블 자체를 지우지는 않음.
SELECT * FROM EMP; -- TRUNCATE 사용 후 값들이 날아갈 것임.




/*
REPLACE
-사용법(문자열, 바꿀문자열, 바뀔문자열)
REPLACE(STR, TARGET_STR, REPLACE_STR)
*/
SELECT REPLACE('700', '700', '1001') AS BONUS FROM SALGRADE;  -- SELECT 할 때 데이터 추출의 경우
/*
UPDATE TABLENAME SET COLUMN-NAME = REPLACE(COLUMN, '바꿀문자열', '바뀔문자열');
*/
UPDATE KOSMENU SET MENU1 = REPLACE('메뉴1', '1', ''); -- 조회가 아닌 바꾼 상태로 테이블 저장할 경우




/*
INSTR 사용법
INSTR(문자열, 검색할 문자, 시작지점, N번째 검색단어) 형태
특징
: 1)찾는 문자가 없으면 0을 반환합니다.
: 2)찾는 단어 앞글자의 인덱스를 반환합니다.
: 3)기본으로 왼쪽부터 시작합니다.
: 4)시작지점에 음수를 쓸 경우 기본방향과 반대로 시작하게 됩니다.
*/

SELECT * FROM KOSMENU;

-- 특징1 의 예시)
SELECT INSTR('MENUDATE', 'OK') AS A FROM KOSMENU;   -- 코스메뉴 메뉴날짜에 OK 없으면 리턴이 0임
-- 특징2 의 예시)
SELECT INSTR('MENUDATE', 'DA') AS B FROM KOSMENU;   -- DA 앞글자의 인덱스인 5가 나올것임.
-- 특징3 의 예시)
SELECT INSTR('CSRCOMPLETE', 'CS',1) AS C FROM KOSMENU;  -- CSRCOMPLETE 에서 CS의 위치를 찾기. -> 1
-- 특징4 의 예시)
SELECT INSTR('LOADMASTER','LO','-2') AS D FROM KOSMENU; -- ?? 차이가 없어보임.


/*
TRIM 사용법
SELECT MENUDATE FROM KOSMENU
*/

SELECT * FROM KOSMENU;  -- 조회할 테이블.

WITH MENULIST AS (
    SELECT '    20112   5' MENUDATE, '  햄버거  ' MENU9 FROM KOSMENU
)
SELECT MENUDATE, TRIM(MENUDATE), MENU9, TRIM(MENU9)
FROM KOSMENU       -- ' 햄버거 ' 의 공백 양옆을 지우고 '햄버거' 형태로 출력할 TRIM 기능이다.

-- WITH AS 기능
-- : 오라클에서는 WITH절에 정의된 SQL문장으로 오라클 공유메모리에 임시테이블을 생성하여 반복 재사용이 가능하도록 할수있습니다.
-- 그렇게 하면 동일 테이블 접근을 최소화하여 퀴리 수행시 필요한 속도나 SQL 복잡도 혹은 스캔 라인을 간소화하는
-- 쿼리 튜닝 측면의 활용도 기능이 있습니다. 해당 기능은 다른 DBMS 에서도 대부분 지원합니다.



/*
오라클 내장함수 3 : miscellaneous single row function

1.COALESCE 함수
기능 : 처음으로 NULL이 아닌 필드값을 만나면 그 값을 리턴합니다.
용도 : 두 개 컬럼중 존재하는 값으로 합치고 싶을 때, 컬럼 병합하기, 여러 열 NULL 아닌 것 찾기.
문법 : SELECT COALESCE (COLUMN1, COLUMN2, COLUMN3, COLUMN4 . . . COLUMN N)
        FROM TABLENAME
        >> 순서대로 컬럼1이 NULL이 아니면 컬럼1 리턴, 컬럼2가 NULL이 아니면 컬럼2 리턴 등.
        >> 일종의 NVL 함수처럼 보일 수 있지만 COALESCE 함수는 컬럼을 여러개 받아서 처리할 수 있으므로
        >> NVL 함수의 일반화된 함수의 기능을 가지는 것이라고 할 수 있습니다.

2.DECODE 함수
기능 : CASE IF 문과 같은 기능으로써 조건에 맞으면 TRUE 아니면 FALSE를 하는 기능입니다.
용도 : CASE IF 와 같은 기능이지만 쿼리의 간결화를 위해 사용됩니다. 이 경우는 동등문(=) 조건일 경우만 해당합니다.
문법 : DECODE(컬럼, 조건, TRUE 결과값, FALSE 결과값)
       >>  NVL함수와 마찬가지로 오라클에서만 존재하는 함수입니다.


3.NVL2 함수
기능 : NVL2 는 NVL과 비교했을 때 IF의 분기가 하나 더 추가된 것이라고 볼 수 있습니다. 
용도 : 대상이 NULL인 경우 지정값 반환, NULL이 아닌 경우도 지정된 값으로 반환합니다.
문법 : NVL2(대상, NULL 아닌경우 값, NULL인 경우 값)
        >> NVL(대상, NULL인 경우 값)의 문법에 비해서 NULL인 경우의 지정값도 반환합니다.
        >> DECODE 보다는 간편한 쿼리를 짤 수 있습니다.
        
4.PATH 함수


*/

--1.COALESCE
INSERT INTO KOSMENU
(MENUDATE, MENU1, MENU2, MENU3, MENU4, MENU5, MENU6, MENU7, MENU8, MENU9)
 VALUES('210523', '', '', '','','','','','돼지고기', '김치찌개');      -- 비교를 위해서 ROW 별로 데이터 중간에 하나씩 필드값 교차로 넣음.

SELECT COALESCE (MENU1, MENU2, MENU3, MENU4, MENU5, MENU6, MENU7, MENU8, MENU9) FROM KOSMENU; -- 중간에 널이 아닌것을 찾아서 각 ROW 별로 값을 가져올 것임.

-- 사실은 COALESCE 함수는 내부적으로 CASE WHEN THEN 을 구현한 것과 같다고 합니다.
-- 예시)
-- CASE WHEN COLUMN1 IS NOT NULL THEN A, 
-- CASE WHEN COLUMN2 IS NOT NULL THEN B  END
    --> COALESCE(A, B, C, D, E, F, . . .)는 A가 NULL이 아니면 A를 반환하는 개념이라고 하였으니 위와 같이 동작합니다.
    
--2.DECODE
SELECT * FROM KOSMENU;
SELECT DECODE(SUBSTR(MENU9,0,3),'돈||햄','good','bad') AS FAVORITE FROM KOSMENU;  -- MENU9가 돈 또는 햄으로 시작하면 GOOD, 아니면 BAD 출력하기.

--3.NVL2
SELECT * FROM KOSMENU;
SELECT NVL2(MENU4, 'SALE', 'SOLD OUT') AS MENU4 FROM KOSMENU;   -- NVL2를 통해 MENU4 라는 컬럼값이 NULL이 아니면 SALE, NULL이면 SOLD OUT 이라는 것.
                                                                -- 다만 ALIAS를 안 붙이면 함수이름으로 나오므로 AS 표현을 해준다.
