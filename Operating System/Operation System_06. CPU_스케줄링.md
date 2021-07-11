# 06. CPU 스케줄링

> CPU는 프로세스를 작업하며, 기본적으로 1개밖에 수행하지 못한다. 따라서 1개의 프로세스가 끝나면 다음 프로세스를 작업하며, 이 때 다음 프로세스를 선택하는 알고리즘을 CPU 스케줄링이라고 한다. 최적의 CPU 스케줄링은 상황에 따라 다르기 때문에 그에 맞게 여러가지 방법으로 나뉘어진다.

---

# 1. Preemptive(선점형) VS Non-preemptive(비선점형)

## 1.1. Preemptive(선점형)

- 선점형은 강제적으로 CPU를 점유할 수 있는 알고리즘
  - CPU가 특정 프로세스를 수행중이든 아니든 상관 없다.
  - 병원에서 응급 환자가 온 경우 → 의사는 대기 환자들보다 응급 환자를 먼저 진료한다.

## 1.2. Non-preemptive(비선점형)

- 비선점형은 선점형의 반대로 CPU가 프로세스를 점유한 경우에는 다른 프로세스가 CPU를 점유할 수 없다.
  - I/O 혹은 프로세스 종료로 CPU가 유휴상태일때 다른 프로세스가 점유 가능

---

# 2. Scheduling criteria

- Scheduling criteria(스케줄링 척도)는 스케줄링의 효율을 분석하는 기준
  - CPU Utiliztion(이용률) : CPU가 수행되는 비율
  - Throughput(처리율) : 단위시간당 처리하는 작업의 수
  - Turnaround time(반환시간) : 단일 프로세스의 처음 → 끝까지 걸리는 시간
  - Waiting time(대기시간) : CPU를 점유하기 위해 ready queue에서 기다린 시간
    - 다른 queue는 제외
  - Response time(응답 시간) : 대화형 시스템에서 입력에 대한 반응 시간

---

# 3. CPU Scheduling Algorithms

CPU가 프로세스를 선택하는 알고리즘

## 3.1. First-Come, First-Served(FCFS/FIFO - First-in, First-Out)

- FCFS는 먼저 온 프로세스가 CPU를 점유하는 방식
- 매우 단순하고 많이 사용함
- 반드시 효율적인 알고리즘은 아니다.
- 3개의 프로세스를 실행한 경우를 비교

  - P1 → P2 → P3가 들어왔을 경우
    - AWT(Average Waiting Time)은 평균 대기시간으로 위를 계산하면 17msec가 나온다.

  ![CPU_스케줄링_1](https://user-images.githubusercontent.com/60249222/124295256-974ccb80-db93-11eb-9ec5-53267dc8dfce.png)

  - 만약, 프로세스가 들어온 순서가 P3 → P2 → P1이면 그림은 아래와 같다.

  ![CPU_스케줄링_2](https://user-images.githubusercontent.com/60249222/124295288-a3388d80-db93-11eb-896d-a771fa8b364c.png)

  - 이러한 경우 AWT는 3msec가 나온다.

- FCFS는 Non-preemptive라 CPU에서 수행중인 프로세스가 종료하기 전까지는 끼어들 수없다.

## 3.2. Shortest-Job-First(SJF)

- 가장 짧게 수행되는 프로세스가 가장 먼저 수행되는 방식
- 4개의 프로세스를 간트 차트로 비교하면 SJF가 AWT가 가장 짧다.

  ![CPU_스케줄링_3](https://user-images.githubusercontent.com/60249222/124295321-ad5a8c00-db93-11eb-96ea-06a812e0f2cd.png)

  - AWT = 7mesc
  - FCFS로 비교할 경우에는 AWT가 10.25msec가 나온다.

- 비현실적인 방법 → 어느 프로세스가 언제 끝날지 알 수 없다.
- SJF는 Preemptive & Non-preemptive 둘 다 적용 가능하다

## 3.3. Priority

- 우선순위가 높은 프로세스를 선택하는 방식
- 5개의 프로세스를 우선순위로 수행

  ![CPU_스케줄링_4](https://user-images.githubusercontent.com/60249222/124295361-b77c8a80-db93-11eb-8897-5ddffb595dc6.png)

  - 우선순위별로 수행시 AWT = 8.2msec

- 우선 순위를 정하는 방법
  - 내부적인 요소(internal) → time limit, memory requirement, I/O to CPU burst(I/O작업은 길고, CPU 작업은 짧은 프로세스 우선) 등
  - 외부적인 요소 (external)→ amount of funds being paid, political factors 등
- Priority는 Preemptive & Non-preemptive 둘 다 적용 가능하다
- Priority의 문제는 Starvation(기아)이다.

  - 우선순위가 낮을 경우에 계속해서 CPU를 점유하지 못하는 현상이 발생할 수 있음.
  - 실제 환경에서는 지속적으로 ready queue에서 프로세스가 들어오는데, 특정 프로세스의 우선순위가 너무나 낮을 경우에는 평생 대기만 할 수밖에 없다.

  → 이를 해결하기 위해서 aging이라는 방법이 등장했다.

  - ready queue에서 기다리는 시간이 많아지면 우선순위를 일정량 높여주어 CPU의 서비스를 받을 수 있도록 하는 것

## 3.4. Round-Robin

- 일정 시간(Time Quantum, Time Slice)을 두어서 그 시간만 프로세스를 수행함
- 일정 시간이 끝나면 프로세스의 완료 여부와 상관 없이 다른 프로세스로 스위칭
- 모든 프로세스를 순환하면서 작업 수행
- 기본적으로 preemptive
- 3개의 프로세스로 Round-Robin

  ![CPU_스케줄링_5](https://user-images.githubusercontent.com/60249222/124295395-c06d5c00-db93-11eb-857b-91882deecf31.png)

  - 4초씩만 프로세스가 CPU를 점유할 수 있기 때문에 P1은 4초 후에 끝나고 10초에 다시 받는다.
  - AWT = 5.66msec

- Time Quantum에 매우 의존적
  - Time Quantum이 너무 클 경우에는 FCFS와 동일
  - Time Quantum이 너무 작을 경우는 switching overhead가 증가하여 비효율적
  - 10 ~ 100msec가 가장 적당하다

## 3.5. Multilevel Queue

- 프로세스는 기준에 따라 여러 그룹으로 나누어짐
  - System processes : 운영체제 커널 수준의 프로세스
  - Interactive processes : 유저 수준의 대화형 프로세스
  - Interactive editing processes
  - Batch processes: 대화형 프로세스의 반대인 것으로 일정량을 한 번에 처리하는 프로세스(Ex, 컴파일러)
  - Student processes
- 이러한 프로세스 그룹을 단일 queue에서 사용하는 것은 비효율적 → 각 그룹에 맞게 queue를 두어 사용하는 것이 Multilevel Queue 방식

  ![CPU_스케줄링_6](https://user-images.githubusercontent.com/60249222/124295421-c82d0080-db93-11eb-9860-9c2d93300e00.png)

  queue를 그룹에 맞게 나눈다.

  - queue마다 우선 순위를 지정해줄 수 있다
  - queue마다 여러 기준을 둘 수 있다.
    - CPU 시간을 다르게
    - 다른 스케줄링이 수행되도록

## 3.6. Multilevel Feedback Queue

- 여러개의 queue를 사용하는 점에서 Multilevel Queue와 유사

  ![CPU_스케줄링_7](https://user-images.githubusercontent.com/60249222/124295443-cf540e80-db93-11eb-94af-c529773c2293.png)

- 해당 queue에서 특정 프로세스의 대기 시간이 너무 연장되면 아래의 queue로 프로세스를 옮긴다.
- 각 queue 마다 다른 스케줄링, 다른 우선순위 등을 사용할 수 있다.
- 대부분의 상용 OS는 여러 개의 queue를 사용하며 저마다 다른 방식의 스케줄링을 사용

---

_참고문헌_

- 운영체제 KOCW 양희재 교수님 강의(youtube) <[https://www.youtube.com/channel/UCvC5uyGB_oVtTZU15SFkqWg](https://www.youtube.com/channel/UCvC5uyGB_oVtTZU15SFkqWg)>
- codemcd님의 정리 자료 <[https://velog.io/@codemcd/운영체제OS-6.-CPU-스케줄링](https://velog.io/@codemcd/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-6.-CPU-%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81)>
