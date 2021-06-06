###### 4주차 주제는 슬로우 쿼리와 쿼리 사용시 cpu 사용량 분석입니다.



###### 목차는 다음과 같습니다.

###### 1.슬로우쿼리

###### 2.쿼리사용시 cpu 사용량 분석

###### 3.3~4주차 내용의  V&SQL 목적과  현업에서의 사용





- 1.슬로우 쿼리

  : 슬로우 쿼리란, 데이터베이스 운용에 있어 부하가 걸리는 쿼리를 일컫습니다. 이런 문제의 대부분의 원인은 방대해진 row 데이터들이 원인입니다. 해당 문제를 분석하기 위해선  V&SQL 이란 방법을 사용합니다.

  - v&SQL을 통해서 누적된 SQL 수행횟수로 나눈 평균값, SQL 한번 수행당 얼만큼의 일량과 시간을 소비하는지 계산해서 통계로 보여주는 분석 명령어입니다.

  - SELECT parsing_schema_name,
        **Count**(*)                      sql_cnt,
        **Count**(DISTINCT **Substr**(sql_text, 1, 100))      sql_cnt2,
        **SUM**(executions)                  executions,
        **Round**(**Avg**(buffer_gets / executions))        buffer_gets,
        **Round**(**Avg**(disk_reads / executions))        disk_reads,
        **Round**(**Avg**(rows_processed / executions))      rows_processed,
        **Round**(**Avg**(elapsed_time / executions / 1000000), 2) "ELAPSED_TIME(AVG)",
        **Count**(

    ​	CASE
    ​        		WHEN elapsed_time / executions / 1000000 >= 10 THEN 1
    ​    END)  "BAD SQL",
    ​    **Round**(**Max**(elapsed_time / executions / 1000000), 2) "ELAPSED_TIME(MAX)"
    FROM  v$sql
    WHERE last_active_time >= SYSDATE - 7
    AND executions > 0
    GROUP BY parsing_schema_name  

    -- 스키마별 쿼리 수행 통계 SQL문

  - SELECT sql_id,
        child_number,
        sql_text,
        sql_fulltext,
        parsing_schema_name,
        sharable_mem,
        persistent_mem,
        runtime_mem,
        loads,
        invalidations,
        parse_calls,
        executions,
        fetches,
        rows_processed,
        cpu_time,
        elapsed_time,
        buffer_gets,
        disk_reads,
        sorts,
        application_wait_time,
        concurrency_wait_time,
        cluster_wait_time,
        user_io_wait_time,
        first_load_time,
        last_active_time 
    FROM  v$sql; 

    -- > 개별쿼리별 통계 조회 SQL문

    이상 2가지 형태의 SQL문을 이용하여 다음과 같은 세부사항을 분석할 수 있습니다.

    

  - 1)라이브러리 캐시에 적재된 SQL 커서 정보

    2)SQL 커서에 의해 사용되는 메모리

    3)하드파싱 및 무효화 issue 쿼리횟수, Parse, Execute, Fetch Call 발생 횟수, Execute 혹은   	Fetch Call 시점에 처리한 row 개수 etc

    - 커서(cursor) : select 문을 통해 값들을 저장해두는 메모리 공간으로 3주차 프로시저의 내용에 보면 실행부 파트에 나와있습니다. 해당 커서를 통해 다량의 inser, select t문을 빠르게 수행하는데 유용한 기능을 합니다.

      - 커서의 종류에는 두가지가 존재합니다.

        1.묵시적 커서 : 오라클에서 자동으로 선언해주는 SQL 커서(사용자는 알지 못함) 

        2.명시적 커서 : 주로 프로시저에서 자주 쓰이는 사용자 정의 SQL 커서. 대부분 여러						   개의 행을 처리하고자 사용함.

      - 커서의 속성에는 다음과 같은 내용이 있습니다.

        1. %Found** : 가져올 레코드가 있는 경우 true를 반환

        2. %isOpen** : 커서가 오픈 상태일 경우 true를 반환

        3. %NotFound** : 더이상 참조할 레코드가 없을 때 true를 반환

        4. %RowCount** : 카운터 역할. 처음 오픈시 0, 패치 발생할 때마다 1씩 증가

    - 패치 콜(fetch call) : SELECT문에서 실제 레코드를 읽어 사용자가 요구한 결과집합을 반환하는 과정에 대한 통계를 보여줍니다. (INSERT, UPDATE, DELETE문에서는 결과에 대한 Fetch Call만 리턴함)

    - Parse Call : 커서를 파싱하는 과정에 대한 통계로서, 실행계획을 생성하거나 찾는 과정에 관한 정보를 포함 합니다.

    - Execute Call : 커서를 실행하는 단계에 대한 통계를 보여줍니다.

      

    4)SQL 문을 수행하면서 사용된  CPU time 과 소요시간(ms 단위)

    5) SQL을 수행하면서 발생한 논리적 블록 읽기와 디스크 읽기, 그리고 소트 발생 횟수

    6) SQL 수행 도중 대기 이벤트 때문에 지연이 발생한 시간(microsecond) 

    7) 커서가 라이브러리 캐시에 처음 적재된 시점, 가장 마지막에 수행된 시점	

    

  - 위의 내용들은 주로 DB의 성능을 고도화 시키는데  사용되는 기능입니다. 실제로 실행했던 쿼리문들의 내역과 분석 내용을 보려면 SELECT * FROM V&SQL 을 조회해주면 됩니다.

  - 최종적으로 부하의 원인이 되는 실행이 느린 슬로우 쿼리를 조회하는 방법은 다음과 같습니다.

    SELECT parsing_schema_name 사용자이름,
        **To_char**(elapsed_time / ( 1000000 * **Decode**(executions, NULL, 1,
                                  																			 0, 1,
                                   																			executions) ),
        999999.99)
                  								  평균실행시간,
        executions     					실행횟수,
        sql_text      						 쿼리,
        sql_fulltext    					  전체쿼리
    FROM  v$sql *--실행한 쿼리를 담고있는 테이블*
    WHERE last_active_time > SYSDATE - ( 1 / 24 * 2 )
       	 	  AND elapsed_time >= 1 * 1000000 * **Decode**(executions, NULL, 1,
                                  																					  0, 1,
    																					                              executions)
        		  AND executions > 10
    ORDER BY 평균실행시간 DESC,
         			   실행횟수 DESC 

    --> 10초 이상 걸리는 쿼리문을 조회하여 분석 내용을 보는 쿼리문입니다.

    --> 오라클 11g 버전부터는 대소문자 구분을 주의하여 작성해야 합니다.



- 2.쿼리 사용시 CPU 사용량 분석 및 대처

  : 쿼리를 사용하다 보면 DB의 운용에 문제가 생길 때가 있습니다. 대부분은 앞서 언급했던 바와 같이 방대한 row 데이터들이 원인입니다. 기타의 이유로는 튜닝의 최적화가 덜 된 것들이 마구잡이로 실행되거나 하는 부분이 있겠지만, 이는 실제로 큰 비중을 차지하지는 않는다고 합니다. 경우에 따라서는 특정쿼리가 CPU의 자원을 과하게 사용하는 것이 원인이 되기도 합니다. 따라서 쿼리를 실행할 때 어느 쿼리가 메모리와 CPU를 잡아먹는지는 로그 분석을 통해 알 수 있습니다. 또한 SPID 라는 값을 통해 CPU 사용의 부하에 대한 대처도 할 수 있습니다.

  소개는 

  1.서버의 메모리 보는 방법

  2.V&PROSESSION을 통한 CPU 사용 정보 조회 방법

  3.특정 데이터베이스의 SID 값을 딴 대처방법

  으로 설명하고자 합니다.

  

  - 먼저 오라클의 서버 메모리 조회 방법입니다.

    - select pool, sum(bytes) "SIZE"
      from v$sgastat
      where pool = 'shared pool'
      group by pool;

    - 위와 같은 쿼리를 실행하게 되면 풀의 형태와 SIZE 값이 

      ​		POOL / SIZE

      - shared pool / 452989642 (여기서 단위는 bytes)

        등의 형태로 조회가 됩니다.  pool 이란 SQL문을 처리하고 커서를 공유하는데 사용되는 메모리 공간입니다. 해당 pool의 정보가 관계형 DB에서 메모리 사용량에 핵심이 되는 부분입니다.

      - pool에는 다음과 같은 유형이 있습니다.

        1.Streams Pool : 오라클 스트림이 독점적으로 사용하고 있는 메모리. 오라클 10g에서 처음 출시 되었으며 온라인으로도 크기 조정이 가능함.

        2.Java Pool : 데이터베이스에서 JVM이 실행되는 동안 할당하는 고정된 메모리 공간을 뜻함.

        3.Shared Pool : 공유 커서, 공유 프로시저, 고정 오브젝트, 딕셔너리 캐시와 수십개의 많은 데이터들을 포함하고 있고, 온라인 상태에서도 크기를 조정할 수 있음.

        4.SGA(System Global Area) - 모든 사용자가 공유 가능하여 사용.

        ​	PGA(Program Global Area) - 사용자마다 공유하지 않고 개별적으로 사용.

      

    - 위의 내용을 프로세서(CPU) 측면과 연관지어서 하드웨어 개념으로써 바라본다면, 메모리의 과부하가 생기면 프로세스 처리에 장애가 발생하게 됩니다. 가상 메모리를 사용한다고 가정했을 경우엔 모르겠지만, 일반적으로는 둘의 관계는 톱니바퀴로 굴러가게 됩니다.

      

  - 다음은 V&PROCESS 를 거쳐 CPU 사용 정보를 조회하는 방법입니다. (오라클 10g 이상의 상위버전은 기본적으로 view를 관리자계정에 제공한다고 합니다. )

    - SELECT c.sql_text,
          b.sid,
          b.serial#,
          b.machine,
          b.osuser,
          b.logon_time
      FROM  v$process a,
          v$session b,
          v$sqltext c
      WHERE a.addr = b.paddr
          AND b.sql_hash_value = c.hash_value
          AND a.spid = '26235'
      ORDER BY c.piece; 

      -- 해당 쿼리는 CPU를 많이 사용하는 프로세스를 확인하는 쿼리입니다. 내용에서 컬럼별로 SID는 DB 시드, serial은 시리얼번호, machine은 머신 정보, osuser는 os 사용자 정보, logon_time은 로그인 시각 등을 나타냅니다. 	  

      해당 방법은 V&SESSION(오라클 세션 접속 정보)을 통해서 조회를 하는 방법입니다.

      흔히 DB 서버 모니터링 이라고 하는 작업으로 구분됩니다. 

      

  - 다음은 특정 데이터베이스의 SID 값을 딴 CPU 사용 부하시 대처방법 입니다.

    - 먼저 데이터베이스 정보를 출력해줄 테이블을 만들어줍니다.

      CREATE TABLE sp_who2
       (
         spid    *INT*,
         status   *VARCHAR*(255),
         login    *VARCHAR*(255),
         hostname  *VARCHAR*(255),
         blkby    *VARCHAR*(255),
         dbname   *VARCHAR*(255),
         command   *VARCHAR*(255),
         cputime   *INT*,
         diskio   *INT*,
         lastbatch  *VARCHAR*(255),
         programname *VARCHAR*(255),
         spid2    *INT*,
         requestid  *INT*
       ); 

    - 그 다음 데이터 조회를 위해 쿼리문을 다음과 같이 작성합니다.

      SELECT *
      FROM  #sp_who2
      WHERE status = 'RUNNABLE'
      ORDER BY cputime ASC 

      --> 출력시 CPUTIME 과 SPID 값이 행 데이터로 반환됩니다. 가장 높은 사용 정보를 뱉은 SPID 값을 따서 쿼리가 어떤것이 있는지 체크합니다.

    - SELECT 문을 이용하는 쿼리문이므로 CPU를 유발합니다. PID 값 죽이기 명령어를 CMD 에서 실행하게 되면 해당 PID는 죽게되고 CPU 사용량 부하는 줄어들게 됩니다.

      - KILL -9 PID 를 입력하면 PID가 속한 프로세스는 죽게됩니다. 해당 내용은 좀비프로세스의 대처에도 도움이 됩니다.(제대로 제거되지 않은 프로세스 잔해물)

        

- 3.V&SQL의 사용 목적과 현업에서의 사용

  :  3주차 부터 설명했던 쿼리 튜닝을 할 때의 효율적인 방법은 상위 N% 범위를 정하고, 해당 범위의 SQL을 튜닝하는 것입니다. 전체 SQL을 튜닝할 수는 없기 때문에 시스템 부하가 높은 쿼리나 자주 수행되는 쿼리 등을 지목하는 전략을 세우는게 일반적입니다. 그러기 위해선 위와 같은 작업이 필요한 것이 됩니다.

  

  - V&SQL의 사용 목적

    : V&SQL은 위의 집중 튜닝이 필요한 대상 SQL을 선정하는 데 활용하는 도구입니다. 더 나아가서는 관리의 목적으로 튜닝 성능 전과 후의 향상도를 비교할 목적으로 통계를 내는 데도 활용됩니다.

  

  ​		: 이런 개념들은 DB의 성능 고도화를 목표로 이루어집니다. 고도화의 작업도 다음과 같은 절		   차를 따라 진행되는 것이 정석입니다.

  ​			**모니터링 자료 수집 -> 분석 진단 -> 튜닝 -> 평가**

  

  ​		:  DB 성능 튜닝의 3대 핵심은 다음과 같습니다.

  ​			1.라이브러리 캐시 최적화(위의 1번 항목 내용 참고)

  ​			2.데이터베이스 CALL 최적화(위의 1번 항목 내용 참고)

  ​			3.I/O 효율화 및 버퍼캐시 최적화

  ​				- I/O 효율화 및 버퍼캐시 최적화 개념은 인덱스와 조인 원리, 옵티마이저 원리를 기반으				  로 한 SQL이 포함됩니다.

  

  - V&SQL의 현업에서의 사용

    : 원초적인 V&SQL의 사용 목적은 DB의 고도화라고 말씀드렸습니다. 이것을 토대로 실무 데이터를 운용하는 현업에서는 BACK 단의 성능 고도화 및 최적화가 데이터를 실제로 저장되어 프론트 단에서 보여질 수 있게 하는 데 있어서 기반이 됩니다. 

    : 최근의 IT 기업의 흐름은 대부분 빠르게 변화하는 툴의 흐름과 신기술을 반영하는 차세대 개발이 유행한다고 합니다. 기존의 시스템을 배제하는 방법을 선택할 때는 이와 같은 프론트 - 백엔드 각자의 고도화 작업이 필요한 것이며, 기존의 시스템 SM 차원에서 놓고 봤을 때, 운용서비스 측면에서도 필수적인 작업입니다.



###### 별첨 : 오라클 공식 홈페이지의 V&SQL 안내 >> https://docs.oracle.com/search/?q=V%26SQL

