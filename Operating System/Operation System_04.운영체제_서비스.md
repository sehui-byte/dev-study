# 04. 운영체제 서비스

> 운영체제의 주요 서비스는 하드웨어 자원을 각 사용자 프로그램에 적절히 분배하는 것 → 이를 위해 운영체제에서는 각 하드웨어를 관리하는 기능이 존재한다.

---

- 운영체제 서비스

  ```bash
  ** 운영체제 내에서 각 하드웨어를 관리하는 기능
  1. 프로세스 관리
  2. 주기억장치 관리
  3. 파일 관리
  4. 보조기억장치 관리
  5. 입출력 장치 관리
  6. 네트우킹
  7. 보호
  8. 기타 ...
  ```

## 1. 프로세스 관리

- 프로세스 : 실제 메인 메모리에서 실행 중인 프로그램
- 프로그램 : 하드디스크와 같은 보조기억장치에서 아무런 동작도 하지 않는 상태
- 주요 기능
  - 프로세스의 생성, 소멸
  - 프로세스 활동 일시 중지, 활동 재개
  - 프로세스간 통신(IPC 통신)
  - 프로세스간 동기화
  - 교착상태 처리(deadlock handling)
- 프로세스의 생애 주기(라이프 사이클)

  ![프로세스_라이프사이클_2](https://user-images.githubusercontent.com/60249222/122664437-58566780-d1dc-11eb-95d2-0d68bb36c289.png)

  - new → ready(admitted) : 생성 → 준비상태, 프로세스가 처리되기 위한 메모리와 필요 주변 장치 체크
  - ready → running(scheduler dispatch) : 준비 → 수행상태, CPU 스케줄링 알고리즘에 따라 프로세스가 선택, 처리됨
  - running → ready(interrupt) : 수행 → 준비상태, 현재 처리되고 있는 프로세스의 타임 퀀텀이 다 되었을 경우
  - running → waiting : 수행 → 대기상태, 입/출력 요청에 의해서 CPU 서비스를 기다리는 상태
  - waiting → ready : 대기 → 준비상태, 입/출력이 끝나 다시 CPU의 서비스를 받기 위한 상태
  - running → terminated : 수행 → 종료상태, 작업이 성공적으로 완료되었거나 에러가 발생해 작업이 종료된 상태

## 2. 주기억장치 관리

- 메인 메모리는 프로그램이 실행되기 위한 공간
- CPU만 메인 메모리에 있는 프로세스하고 소통 가능
- 주기억장치 관리는 메인 메모리를 효율적으로 사용하도록 관리
- 주요기능
  - 프로세스에게 **메모리 공간 할당**
  - 메모리의 어느 부분이 어느 프로세스에게 할당되었는지 추적
  - 프로세스 종료 시 메모리 회수
  - 메모리의 효과적인 사용
  - 가상 메모리 : 물리적 실제 메모리보다 큰 용량을 갖도록

## 3. 파일 관리

- 여기서 파일이라는 논리적 관점에서 데이터를 바라보고 관리하는 것
- 파일 : 하드디스크에 저장되어 있는 것을 사용자가 편리하게 사용할 수 있도록 논리적인 형태로 운영체제에서 관리하여 보여주는 것
- 주요기능
  - 파일의 생성과 삭제
  - 디렉토리의 생성과 삭제
  - 기본 동작 지원 : open, close, read, write, create, delete
  - Track/sector(디크스의 물리적인 구성) - file 간의 매핑
  - 백업

## 4. 보조기억장치 관리

- 주로 하드 디스크, 플래시 메모리
- 하드 디스크에서 아무 것도 저장되어 있지 않는 공간 → block
  - 보조기억장치는 이를 관리하는 것
- 주요기능
  - 빈 공간 관리
  - 저장공간 할당
  - 디스크 스케줄링
    - 헤드를 적게 움직이면서 파일을 찾는 방법

## 5. 입출력 장치 관리

- 키보드, 마우스, 프린터, 스피커 등등
- 주요기능
  - 장치 드라이브
  - 입출력 장치의 성능 향상

## 6. 시스템 콜

- 프로세스에서 운영체제 서비스를 필요로 할 때 이를 받기 위해 사용하는 호출

  ![시스템_콜](https://user-images.githubusercontent.com/60249222/122664446-699f7400-d1dc-11eb-91b1-9e6650642ed7.png)

  프로세스 관리에 필요한 시스템 콜 요청 → 운영체제 안의 해당 코드로 진입

- 주요 시스템 콜
  - Process : end, abort, load, execute, create, terminate...
  - Memory : allocate, free
  - File : create, delete, open, read, write...
  - Device : request, release, read, write
  - Information: get/set time, get/set system data
  - Communication : socket, send, receive

---

_참고문헌_

- 운영체제 KOCW 양희재 교수님 강의(youtube) <[https://www.youtube.com/channel/UCvC5uyGB_oVtTZU15SFkqWg](https://www.youtube.com/channel/UCvC5uyGB_oVtTZU15SFkqWg)>
- codemcd님의 정리 자료 <[https://velog.io/@codemcd/운영체제OS-3.-이중모드와-보호](https://velog.io/@codemcd/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-3.-%EC%9D%B4%EC%A4%91%EB%AA%A8%EB%93%9C%EC%99%80-%EB%B3%B4%ED%98%B8)>
- L2MEN Research Blog님의 정리 자료 <[https://l2men.tistory.com/9](https://l2men.tistory.com/9)>
