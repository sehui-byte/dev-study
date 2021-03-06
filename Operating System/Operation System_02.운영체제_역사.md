# 02. 운영체제의 역사

> 운영체제의 발전은 컴퓨터의 발전과 같이 발전하게 되었다. 운영체제의 역사를 이해하면서 각 시스템의 특징을 이해하도록 하자.

---

## 1) 초기 컴퓨터

- 초기 컴퓨터는 크게 3 가지로 구성
  - 카드 리더 : 입력기. 종이에 입력할 코드에 맞는 구멍을 뚫음
  - 프로세서 : 현대와 비슷. 계산기 역할
  - 프린터 : 입력한 결과를 종이에 출력
- 이 시기에는 운영체제가 존재하지 않았으며, 오퍼레이터(Operator)라는 컴퓨터 사용 직업이 따로 존재
- 컴퓨터의 크기가 커서 건물 전체를 사용할 필요가 있음

## 2) Batch Processing System(일괄 처리 시스템)

- 초기 컴퓨터에서는 프로그램 수행시 **컴파일 → 링크 → 로딩** 순서를 오퍼레이터가 직접 입력
- 이러한 과정을 **자동화**한 것이 일괄 처리 시스템
  - **컴파일 → 링크 → 로딩**을 하나의 프로그램으로 작성, 프로세서의 메모리에 할당
    - 이러한 프로그램을 Resident Monitor라고 함(항상 프로세서 안에서 거주한다는 의미)
    - 최초의 OS
- 일괄 처리 시스템의 문제
  - 컴퓨터 응답 시간이 오래 걸림
    - 시간을 많이 필요로 하는 사용자 프로그램이 실행될 경우 → 하나만 보고 있음
  - 실행 시간도 오래 걸릴 수 있음
    - CPU가 필요 없이 I/O만 필요한 작업임에도 CPU를 점유하고 있음

## 3) Multi Programming System(다중 프로그래밍 시스템)

- 초창기 메모리의 상태는 Resident Monitor를 제외하고 단 하나의 사용자 프로그램만 할당하여 사용 → 매우 비효율적인 방법
  - 따라서, 매우 비싼 장비인 컴퓨터를 동시에 사용할 수 있는 방법이 필요
- 프로그램을 수행하는 도중에는 CPU와 I/O 장치가 **교대로 수행**
- CPU에 비해 I/O 장치는 속도가 느리기 때문에 I/O 장치가 수행하는 동안에 CPU는 아무 일도 하지 않음
  - CPU가 아무 일도 하지 않는 상태를 **cpu** **idle 상태**라고 함
- 이러한 idle 상태를 해결하기 위해 Memory에 여러 사용자 프로그램을 올리는 시스템을 구안
  - **Multi Programming System**
- 다중 프로그래밍 시스템의 절차

  1. CPU가 user1 프로그램을 수행
  2. user1 프로그램 수행 도중 I/O 장치를 수행하는 코드를 읽음
  3. I/O 장치를 수행할 동안, CPU는 user2 프로그램을 수행

  **→ idle 상태의 시간을 최대한 줄이고자 노력**

- 다중 프로그래밍 시스템의 문제
  - user1, user2, user3 ... → CPU가 어느 프로그램을 먼저 수행해야 하는지 선택의 문제
    - **CPU 스케줄링**
  - 새로 들어온 프로그램 → CPU가 어느 메모리 공간에 할당해야 하는지의 문제

## 4) Time-sharing System(TSS, 시분할 시스템)

- 모니터, 키보드의 사용 → 사용자와 컴퓨터가 대화 형식(interactive) 가능
- 여전히 비싼 컴퓨터의 비용
- 하나의 단말기에 여러개의 모니터&키보드를 붙여서 사용

  - 다중 사용자 지원

  ![OS__](https://user-images.githubusercontent.com/60249222/120916406-6ee0c700-c6e4-11eb-9beb-9d21eaba6734.png)

- 문제

  - CPU가 하나인 상태에서 User1의 프로그램이 수행할 경우
    - User2, 3, ... N의 사용자는 CPU가 User1의 프로그램이 완료되거나, 혹은 I/O 작업을 할 때까지 마냥 대기하는 수밖에 없음

  → 이러한 문제를 해결하기 위해 **시분할 시스템(TSS)**가 등장

- TSS는 **CPU가 하나의 프로그램을 수행하는 시간에 제한**을 둠

  - CPU가 User1의 프로그램을 일정 시간 수행
  - CPU가 다른 프로그램(User2, User3, etc...)으로 스위칭
  - 다른 프로그램을 일정 기간동안 수행 후 다시 스위칭

  → 매우 빠른 속도(ms)로 CPU가 하나인 환경에서도 사용자가 동시에 작업하는 듯한 효과

- 여전히 스케줄링 & 메모리에 대한 문제가 제기
  - 이를 위해 동기화, 가상 메모리 기술 등이 이후에 등장

## 5) Interrupt Base System(인터럽트 기반 시스템)

- 현대의 운영체제 시스템
- 인터럽트
  - 중단, 방해, 새치기 등등
  - CPU가 특정 기능을 수행하는 도중, 급하게 다른 일을 처리하고자 할 때 사용하는 기능
  - 마우스를 동작하는 경우
    1. 마우스에서 인터럽트 전기 신호 발생
    2. 인터럽트 전기 신호를 CPU에 전송
    3. CPU는 이를 감지, 자신이 하던 일을 중단
    4. 인터럽트 신호를 처리하기 위해 운영체제 내부에 있는 인터럽트 서비스 루틴(Interrupt Service Routine, ISR)을 수행
- 인터럽트의 종류
  - 하드웨어 인터럽트
    - 마우스, 키보드 등등 하드웨어에서 발생하는 인터럽트
  - 소프트웨어 인터럽트
    - 명령어를 통해서 전기 신호를 CPU에 전송
    - 어셈블리어의 swi, int 와 같은 명령어를 수행
  - 내부 인터럽트
    - 프로그램을 수행하는 도중에 발생하는 예외 상황 처리
    - DividedByZero, 0으로 나누는 경우
      - ISR 이동, 에러 메시지 발생, 해당 프로그램 종료
- 복합적인 인터럽트 절차
  1. 마우스로 워드 프로그램 실행 → 하드웨어 인터럽트 수행
  2. 워드 프로그램에서 특정 파일을 읽기 실행 → 소프트웨어 인터럽트 수행
     - 하드웨어 인터럽트와 동일, 운영체제 내부에 ISR이 존재, 해당 루틴 수행
     - 루틴 수행 후 다시 사용자 프로그램을 바라봄

![OS_](https://user-images.githubusercontent.com/60249222/120916417-81f39700-c6e4-11eb-88cf-e25a65b0b2f1.png)

> CPU는 사용자 프로그램과 운영체제 내부를 **교대로 작업 수행**

> 운영체제는 평소에 아무 일도 하지 않는 **대기 상태**에 있다가 **인터럽트가 발생**하는 순간 작업을 수행한다. 

---

_참고문헌_

- 운영체제 KOCW 양희재 교수님 강의(youtube) <[https://www.youtube.com/channel/UCvC5uyGB_oVtTZU15SFkqWg](https://www.youtube.com/channel/UCvC5uyGB_oVtTZU15SFkqWg)>
- codemcd님의 정리 자료 <[https://velog.io/@codemcd/운영체제OS-2.-운영체제-역사](https://velog.io/@codemcd/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-2.-%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EC%97%AD%EC%82%AC)>
