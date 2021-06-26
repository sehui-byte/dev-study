### 운영체제란?

- Operation System으로 컴퓨터 하드웨어를 잘 관리하여 성능을 높이고(Performance), 사용자에게 편의성을 제공(Convenience)하며, 컴퓨터 하드웨어를 관리하는 프로그램(Control program for computer)
- 시스템 자원(System Resource, 컴퓨터 하드웨어)을 관리
  - CPU(중앙처리장치) : 각 프로그램이 얼마나 CPU를 사용해야 하는지 스스로 결정하지 못함
  - Memory : 각 프로그램이 어느 주소에 저장되어야 하는지, 어느 정도의 메모리 공간을 확보해야 하는지 결정 못함
  - HDD/SSD : 어떻게, 어디에 저장하는지 결정 못함

> 이러한 문제를 해결하기 위해서는 운영체제가 필요

### 운영체제의 역할

- 시스템 자원(System Resource) 관리자
  - CPU, Memory, Disk, I/O Drive etc...
- 사용자와 컴퓨터간의 커뮤니케이션 지원
  - 사용자가 컴퓨터 프로그램을 원활하게 동작할 수 있도록
- 컴퓨터 하드웨어와 응용 프로그램을 제어
  - 관리자 모드/사용자 모드 존재 → 권한

### 운영체제의 부팅

- 전원을 누르면 Memory 내부에 존재한 ROM에 있는 내용을 읽음

  - ROM : 비휘발성 메모리로 메모리에서 극히 일부를 차지함(수 KB)
  - ROM 내부에는 POST(Power-On Self-Test)와 부트 로더(Boot Loader)가 저장됨
    - POST : 전원이 켜지면 가장 처음으로 실행되는 프로그램. 현재 컴퓨터 상태 검사
    - 부트 로더 : HDD에 저장되어 있는 운영체제를 찾아서 메인 메모리(RAM)에 올림

  ![OS_](https://user-images.githubusercontent.com/60249222/120916293-c03c8680-c6e3-11eb-962a-6fda5caa3a0a.png)

  부팅 과정을 도식화

### 운영체제의 구조

- 운영체제는 크게 커널(Kernel)과 명령어 해석기(Command interpreter, Shell)로 나뉜다.
  - 커널 : 운영체제의 핵심. 운영체제가 수행하는 모든 것이 저장되어 있음
  - 쉘(명령어 해석기) : 커널에 요청하는 명령어를 해석하여 커널에 요청, 결과를 출력

### 운영체제의 위치

- 사용자 프로그램(Application)은 특정 운영체제에 종속적이다. 따라서 하나의 사용자 프로그램은 서로 다른 운영체제에서 수행할 수 없다.
  - Windows용 프로그램은 Linux에서 사용 불가

![OS_ 1](https://user-images.githubusercontent.com/60249222/120916266-a1d68b00-c6e3-11eb-8082-ea5779d3dda2.png)

사용자 프로그램은 운영체제가 제공하는 자원만 사용 가능하다. 

---

_참고문헌_

- 운영체제 KOCW 양희재 교수님 강의(youtube) <[https://www.youtube.com/channel/UCvC5uyGB_oVtTZU15SFkqWg](https://www.youtube.com/channel/UCvC5uyGB_oVtTZU15SFkqWg)>
- codemcd님의 정리 자료 <[https://velog.io/@codemcd/운영체제OS-1.-운영체제란](https://velog.io/@codemcd/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-1.-%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C%EB%9E%80)>
