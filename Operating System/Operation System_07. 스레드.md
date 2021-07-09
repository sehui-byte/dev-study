# 07 스레드(Thread)

> 프로세스와 스레드는 비슷하지만 다르다. 헷갈릴 수 있는 개념이기 때문에 여기서 명확하게 정리했으면 좋겠다.

---

# 1. 프로세스의 생성과 종료

- 프로세스는 프로세스에 의해 만들어짐
  - 최초의 프로세스는 운영체제마다 다르게 부르나 통상적으로 Init이라고 부른다.
  - 프로세스는 마치 조직도처럼 트리 모양을 나타낸다.
    - 계층과 관계에 따라서 부모, 자식, 형제 프로세스라고 한다.
- 프로세스는 각각 고유한 번호를 가진다. 이를 PID(Process Identifirer)라고 한다.

  - PID는 일반적으로 정수형으로 표현된다.
  - PPID는 부모의 프로세스를 나타낸다.

    ![스레드_1](https://user-images.githubusercontent.com/60249222/125082026-a5ee3200-e101-11eb-8198-007ce6e3ad18.png)

    PID 7 을 부모로 하는 프로세스가 4개 존재하고 있음을 ps -ef 명령어를 통해 확인할 수 있다.

## 1.1 프로세스 생성

- 새로운 프로세스를 만드는 시스템 콜이 존재 → `fock()` 사용
- 만들어진 프로세스에서 특정 파일을 실행하는 시스템 콜 → `exec()` 사용

## 1.2 프로세스 종료

- 프로세스를 종료하는 시스템 콜 → `exit()`
- 프로세스 종료시 프로세스가 점유하고 있는 자원(메모리, 파일, I/O 등)을 회수
  - 회수한 자원과 권한은 운영체제로 회귀

# 2. 스레드(Thread)

- 스레드는 프로그램 내부의 흐름(맥)이다.
- 프로세스보다 작은 실행 흐름의 최소의 단위이다.

```c
int main(void)
{
  int n = 0;
  int m = 10;
  printf("%d\n", n * m);
  while(n < m)
    n++;
  printf("END\n");
}
```

## 2.1 다중 스레드(Multi Threads)

- 하나의 프로그램에 스레드가 2개 이상 존재하는 경우를 다중 스레드라 한다.
- 스레드 단위의 스위칭이 매우 빠르기 때문에 사용자는 여러 스레드가 동시에 실행되는 것처럼 보인다.
  - Concurrent : 동시에 수행되는 효과, 사실은 하나의 일만 처리하고 있다.
  - Simultaneous : CPU가 여러개라 실제로 동시에 수행한다.
- 다중 스레드의 대표적인 예는 웹 서버가 있다.
  - 화면 출력과 데이터 I/O를 하는 스레드가 다른 일을 하고 있다.
  - 현대의 대부분의 프로그램은 모두 다중 스레드로 동작한다
- 현대의 운영체제는 프로세스보다 작은 스레드 단위의 컨텍스트 스위칭을 지원한다.

## 2.2 스레드 VS 프로세스

- 하나의 프로세스에는 운영체제로부터 할당받은 메모리 공간이 존재

  ![스레드_2](https://user-images.githubusercontent.com/60249222/125082056-aedf0380-e101-11eb-99c9-1f66f87b7e4c.png)

  - code : 코드 영역, 실행할 프로그램의 코드가 위치한다.
  - data : 데이터 영역, 전역 변수, 정적 변수가 위치한다.
  - heap : 힙 영역, 사용자가 동적으로 할당할 수 있는 공간이다.
  - stack : 스택 영역, 지역 변수, 매개변수가 위치한다.

- code, data 메모리 공간은 하나의 프로세스에 속한 여러 스레드가 자원을 공유해서 사용한다.
- 이외에 프로세스의 자원인 file, I/O 등도 여러 스레드가 공유한다.
- 반면, PC(Program Count), SP(Stack Pointer), register, stack 등은 각 스레드가 고유하게 가지고 있다.
  - PC(Program Count) : 다음에 CPU가 실행해야 할 명령어의 주소
  - SP(Stack Pointer) : 스택에서 데이터가 추가되거나 제거되어야 할 위치(TOP Pointer)
  - register : 레지스터
  - stack : 스택, 스위칭시 작업해야 할 리턴 어드레스를 stack에 저장

---

_참고문헌_

- 운영체제 KOCW 양희재 교수님 강의(youtube) <[https://www.youtube.com/channel/UCvC5uyGB_oVtTZU15SFkqWg](https://www.youtube.com/channel/UCvC5uyGB_oVtTZU15SFkqWg)>
- codemcd님의 정리 자료 <[https://velog.io/@codemcd/운영체제OS-7.-쓰레드Thread](https://velog.io/@codemcd/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-7.-%EC%93%B0%EB%A0%88%EB%93%9CThread)>
- TCP SCHOOL.com <[http://tcpschool.com/c/c_memory_structure](http://tcpschool.com/c/c_memory_structure)>
