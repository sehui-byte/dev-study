# 05. 프로세스 관리

> **프로세스**는 메인 메모리에서 **실행중인 프로그램**을 의미하며, **프로그램**은 디스크에 저장되어 **아무 일도 하지 않는 상태**이다. 프로세스는 실행에 따라서 stack pointer, data, text, register 등이 끊임 없이 변한다. 운영체제는 이러한 프로세스를 **관리하는 기능**을 수행한다.

---

## 1. 프로세스

### 1.1 프로세스의 상태

> 프로세스의 상태는 프로세스의 라이프 사이클을 설명하면서 언급했던 부분이다.

![__2](https://user-images.githubusercontent.com/60249222/123507223-40cb2300-d6a3-11eb-9b97-2fbe3de10f03.png)

1. new : 프로그램이 메인 메모리에 할당된 상태
2. Ready : 할당된 프로그램이 실행되기 위한 모든 준비를 마친 상태
3. Running : CPU에 의해 수행되고 있는 상태
4. Waiting : 프로세스가 미종결된 상태에서 I/O 및 타임퀀텀/타임 슬라이스(CPU 실행의 최소 단위)에 의해 대기중인 상태 → 미종결이기 때문에 Ready에서 다시 Running으로 CPU의 수행을 받아야 함
5. Terminated : 프로세스가 종결된 상태

### 1.2 PCB(Process Control Block)

- PCB는 **프로세스에 대한 모든 정보**가 있는 블록
  - PID(프로세스 ID), PC(프로세스 counter), register, MMU, CPU 점유 시간 등이 포함
- CPU는 스케줄링에 의해 여러 프로세스를 변경하면서 작업을 수행
- A 프로세스에서 B 프로세스로 컨텍스트 스위칭이 일어날 경우, A 프로세스의 상태값을 저장 필요
  - 이러한 정보를 PCB에 저장

![20210626-164221](https://user-images.githubusercontent.com/60249222/123507249-648e6900-d6a3-11eb-9aef-774537b0db4b.png)

PCB는 프로세스의 여러가지 상태값을 저장하고 있다.

### 1.3 프로세스 큐(Queue)

- 프로세스는 수행하면서 상태가 여러번 변경
  - CPU의 수행를 받는 상태
  - I/O의 수행를 받는 상태
  - Disk에 저장된 상태
- 프로세스는 여러개가 한번에 수행됨 → 이에 따른 **순서**가 필요
  - 이러한 순서를 대기하는 곳이 **큐(Queue)**

![20210626-164216](https://user-images.githubusercontent.com/60249222/123507234-4b85b800-d6a3-11eb-8551-325f589f9156.png)

- Job Queue : Disk에 있는 프로그램이 메인 메모리의 할당을 기다리는 큐
- Ready Queue : 메인 메모리에서 CPU의 점유 순서를 기다리는 큐
- Device Queue : I/O 장치의 수행을 기다리는 큐
- 각 큐에는 스케줄링이 동작하기 때문에 PCB가 존재
- Job Queue → Job Scheduler(Long-term scheduler)
  - Disk에서 메인 메모리로 올라가는 스케줄러는 시간이 오래 걸림(오래 걸려도 상관 없음)
- Ready Queue - CPU Scheduler(Short-term scheduler)
  - 메모리에서 CPU로 할당되는 스케줄러는 시간이 매우 빠름(현대 컴퓨터가 멀티프로그래밍처럼 보여지는 이유)
- Device Queue - Device Scheduler

## 2. 멀티 프로그래밍(Multi-Programming)

> **단일 CPU**에서 **여러 개의 프로세스가 동시 실행**되는 것을 말하나, 실제 동시에 실행되지는 않고 그렇게 보이는 것을 말한다. 아래는 멀티 프로그래밍을 이해하기 위한 용어에 대한 정리이다.

### 2.1 Degree of Multi-Programming

- 현재 메모리에 할당되어 있는 프로세스의 개수

### 2.2 I/O-bound VS CPU-bound process

- 프로세스는 아래와 같이 나뉜다.
  - I/O-bound : I/O 작업의 비중이 높은 프로세스
  - CPU-bound process : CPU 작업의 비중이 높은 프로세스
- 때문에 Job schedule는 프로세스가 어떤 종류의 프로세스인지를 파악하여 메모리를 할당

### 2.3 Medium-term scheduler

- short-term보다는 덜 발생하지만, long-term보다는 자주 발생하는 scheduler
- 메인 메모리 → Disk로 옮기는 작업을 수행
  - 장기적으로 사용하지 않는 프로세스가 대상
- 이를 **Swapping**이라고 한다.
  - Swapping out : 메인 메모리에서 장기간 사용하지 않는 프로세스를 Disk로 옮김
  - Swapping in : 해당 프로세스를 다시 사용하려고 할 경우 메인 메모리에 할당
  - Swapping in으로 메인 메모리 할당시, **이전의 메모리 주소로 할당되는 것을 보장하지는 않음**

### 2.4 Context Switching(컨텍스트 스위칭, 문맥 전환)

- CPU가 스케줄링에 의해 A 프로세스에서 B 프로세스로 옮겨가는 것
- Scheduler : 여기서는 CPU 스케줄러를 의미
- DIspatcher : CPU 내부의 데이터를 A 프로세스에서 B 프로세스로 변경
  - 프로세스의 현재 상태를 저장 및 복원
- Context switching overhead : 컨텍스트 스위칭이 발생할 때마다 dispatcher가 매번 프로세스의 상태를 저장 및 복원
  - 컨텍스트 스위칭시에 컴퓨터에 생기는 부담

---

_참고문헌_

- 운영체제 KOCW 양희재 교수님 강의(youtube) <[https://www.youtube.com/channel/UCvC5uyGB_oVtTZU15SFkqWg](https://www.youtube.com/channel/UCvC5uyGB_oVtTZU15SFkqWg)>
- codemcd님의 정리 자료 <[https://velog.io/@codemcd/운영체제OS-5.-프로세스-관리](https://velog.io/@codemcd/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-5.-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EA%B4%80%EB%A6%AC)>
