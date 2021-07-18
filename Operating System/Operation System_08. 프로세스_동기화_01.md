# 08. 프로세스 동기화 - 1

운영체제에서 가장 중요한 부분을 뽑으면 **CPU 스케줄링과 프로세스 동기화**이다. 강의에서 프로세스 동기화는 예제 코드를 중심으로 설명하므로, 강의를 수강하면서 해당 예제를 실습하는 것을 권장한다.

# 1. 개요

- 현대 컴퓨터의 메모리에는 여러 프로세스가 동시에 존재한다. 이러한 프로세스들이 하나의 공유 메모리, 혹은 다른 프로세스에 접근하는 것은 매우 섬세한 작업이다.
  - 다른 프로세스 혹은 스레드의 자원에 접근하여 데이터를 변경하는 경우, **사용자가 원하는 결과를 도출할 수 없기 때문**이다.
- 프로세스가 다른 프로세스에 영향을 주는 여부를 가지고 프로세스를 분류할 수 있다.
  - Cooperating Process : 한 프로세스가 다른 프로세스와 영향을 주고 받는 프로세스
  - Independent Process : 아무런 영향도 미치지 않는 독립적인 프로세스
- 현대의 컴퓨터 환경에는 Cooperating Process가 많이 존재한다.
  - 서로 영향을 주기 때문에 수행 흐름에 대한 **동기화**가 매우 중요하다.
    - 프로세스 동기화 : 프로세스 사이의 동기화
    - 스레드 동기화 : 스레드 사이의 동기화
      - 현재는 대부분 스레드를 단위로 스위칭을 하기 때문에 스레드 동기화가 중요하다.
- 결론적으로, 프로세스 단위, 혹은 스레드 단위로 스위칭을 할 때, 공유 자원에 접근하는 경우, 공유 자원의 일관성을 유지하기 위해서는 **동기화**를 고려해야 한다.

# 2. 은행 계좌 문제

- 동기화 문제 중 가장 대표적이며, 많이 사용되는 예시는 은행 계좌 문제가 있다.
- 은행에서는 하나의 계좌에 입금과 출금이 동시다발적으로 일어날 수 있다. 입금과 출금은 각각 프로세스(혹은 스레드)로 볼 수 있으며, 이를 구현한 자바 코드는 아래와 같다.

  - 입금 > deposit
  - 출금 > withdraw

  ```java
  // Test.java
  class Test {
  	public static void main(String[] args) throws InterruptedException {
  		BankAccount b = new BankAccount();
  		Parent p = new Parent(b);
  		Child c = new Child(b);
  		p.start();   // start(): 쓰레드를 실행하는 메서드
  		c.start();
  		p.join();    // join(): 쓰레드가 끝나기를 기다리는 메서드
  		c.join();
  		System.out.println("balance = " + b.getBalance());
  	}
  }

  // 계좌
  class BankAccount {
  	int balance;
  	void deposit(int amount) {
  		balance = balance + amount;
  	}
  	void withdraw(int amount) {
  		balance = balance - amount;
  	}
  	int getBalance() {
  		return balance;
  	}
  }

  // 입금 프로세스
  class Parent extends Thread {
  	BankAccount b;
  	Parent(BankAccount b) {
  		this.b = b;
  	}
  	public void run() {   // run(): 쓰레드가 실제로 동작하는 부분(치환)
  		for (int i = 0; i < 100; i++)
  		  b.deposit(1000);
  	}
  }

  // 출금 프로세스
  class Child extends Thread {
  	BankAccount b;
  	Child(BankAccount b) {
  		this.b = b;
  	}
  	public void run() {
  		for (int i = 0; i < 100; i++)
  		  b.withdraw(1000);
  	}
  }
  ```

- 해당 코드의 `balance = 0`이다.
- 다만, 위의 코드는 매우 단순한 구조로 동기화 문제가 발생할 확률은 매우 낮다. 실제 현실의 문제를 해결하기 위한 코드는 보다 복잡할 것이다. 여기서 `System.out.println()`을 이용하 화면에 출력하는 로직을 구현해 일부러 **시간 지연**을 만들면 조금이나마 현실에 근접한 코드가 될 것이다.

  ```java
  // 계좌
  class BankAccount {
  	int balance;
  	void deposit(int amount) {
  		int temp = balance + amount;
  		**System.out.print("+"); // 추가**
  		balance = temp;
  	}
  	void withdraw(int amount) {
  		int temp = balance - amount;
  		**System.out.print("-"); // 추가**
  		balance = temp;
  	}
  	int getBalance() {
  		return balance;
  	}
  }
  ```

  - 프린트 함수를 추가하였다. 이는 매우 간단하게 보일지 몰라도, 해당 코드는 모니터에 출력을 하는 I/O 작업이 추가되었기 때문에 컴퓨터 입장에서는 보이는 것처럼 간단한 동작은 아니다.
  - 또한, 입출금 횟수를 100 → 1000으로 증가하였다. 해당 코드를 실행할 경우 결과는 다음과 같다.

    ```java
    ++++++++++++++++++++++++++++++++++----------------------------------------------
    --------------------------------------------------------------------------++++++
    +++----------------------------------------------+++++++++++++++++++++++++++++++
    +----+++++++-+++++----+++-------------------------------------------------------
    -+++++++-++++-+++++++++-------++++++++++++++++++++++++++++++++++++++++++++++++++
    ++++++---------------+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    +-++++++++++++-------------------++++++++++++++++++++-++++++++++++++++++++++++++
    ++++++-+------------------------------------------------------------------------
    -+++++++++++-+++++++----------------------------------------+-------+-----------
    -+------+-----------------------------------------------------------------------
    -+------------------------------------------------------------------------------
    -+------------------------------------------------------------------------------
    -------------------+-------+----------------------------------------------------
    ------------------------------+-------------------------------------------------
    ------------------------------------------------------+-------------------------
    -+------------------------------------------------------------------------------
    -++---------------------------------------++++++++++++++++++++++++++++++++++++++
    ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

    balance = 1000000
    ```

  - balance의 값이 0이 아니다. 시간 지연으로 인해서 동기화 문제가 발생한 것이다.
  - 이는 `System.out.prinln()`함수로 인해서 코드 수행에 시간 지연이 발생하였으며, 시간 지연이 발생하여 입금 혹은 출금 코드를 완료하지 못한 상태로, 컨텍스트 스위칭이 일어나서 다른 프로세스(혹은 스레드)로 스위칭이 일어났기 때문이다.

    이 내용은 당연히 혼동스러울 수 있다. 이 설명이 정확한지 확신은 없지만 이해를 돕기 위해 내용을 덧붙인다.

    - 상기 코드는 최종적으로 연산한 값인 temp를 balance에 초기화하여 은행 계좌의 잔액을 표현한다.
    - 만약, temp의 값을 balance에 초기화 하기 전에 입금이든 출금이든 컨텍스트 스위칭이 일어나 다른 프로세스(혹은 스레드)를 수행한 후, 다시 컨텍스트 스위칭하여 해당 환경을 인계받고 temp에 balance값을 초기화한다고 생각해보자. 이 경우 그 전에 무수히 많은 작업을 하였던 무용지물이다. 최종적으로는 이전 temp에 속한 값을 balance에 대입할 테니까.
    - 이 경우 temp는 전역 변수가 아니기 때문에 자원을 공유하지 않는다. 따라서 컨텍스트 스위칭으로 인해서 balance처럼 값이 변경되지 않는다.
    - 하이 레벨 언어에서도 이러한 문제가 발생하는 것으로 본다면, 로우 레벨 언어까지 생각한다면 무척 혼란스러운 동기화 문제가 발생할 것 같다.

  - 이를 해결하기 위해서는 공통 변수에 접근하는 프로세스나 스레드는 하나만 존재하도록 관리해야 한다.
  - 이러한 공통변수 구역을 **임계구역(Critical Section)**이라 한다.

# 3. 임계구역(Critical Section) 문제

- 임계구역 : 각 스레드들이 공유하는 데이터(변수, 테이블, 파일 등)를 변경하는 코드 영역
- 상기 코드에서 임계구역은 다음과 같다.

  ```java
  void deposit(int amount) {
    balance = balance + amount;
  }
  void withdraw(int amount) {
    balance = balance - amount;
  }
  ```

- 임계구역을 해결하기 위해서는 3가지 조건이 충족되어야 한다.
  - 상호 배타(Mutual Exclustion) : 오직 하나의 스레드만 진입 가능. 다른 스레드는 대기
  - 진행(Progress) : 임계구역에 접근하는 스레드를 결정하는 것은 유한 시간 이내에 이루어져야 함
  - 유한 대기(Bounded waiting) : 임계구역에 진입하기 위해 대기하는 모든 스레드는 유한 시간 이내에 해당 임계구역에 진입해야 함.
- 동기화로 인해 해결하고자 하는 문제
  - 원하는 결과값을 도출할 수 있다.
  - 프로세스의 실행 순서를 제어할 수 있다.
  - Busy Wait 등과 같은 비효율성을 제거한다.

# 4. 세마포(Semaphore)

- 대표적인 동기화 도구
- 사전적 의미로는 역이나 군대에서 사용하는 수신호
- 세마포에는 두 가지 동작이 존재
  - 네덜란드에서 만들어졌기에 네덜란드 약자로 되어 있음
    - P(현재 : test) : `acquire()`로 사용
    - V(현재 : increment) : `release()`로 사용
- 자바 코드로 구현한 세마포 구조

  ```java
  class Semaphore {
    int value;      // number of permits
    Semaphore(int value) {
      // ...
    }
    void acquire() {
      value--;
      if (value < 0) {
        // add this process/thread to list
        // block
      }
    }
    void release() {
      value++;
      if (value <= 0) {
        // remove a process P from list
        // wakeup P
      }
    }
  }
  ```

  - `acquire()`는 value 값을 감소
    - value의 값이 0보다 작은 경우 → 해당 임계구역에 어느 프로세스가 존재함을 의미
      - 이런 경우 block을 걸어준다. block는 큐로 보내서 강제대기시키도록 한다.
  - `release()`는 value 값을 증가

    - value의 값이 0보다 같거나 큰 경우 → 임계구역에 진입하려고 대기중인 프로세스가 list에 남아 있음

      - 이런 경우 그 중 하나를 꺼내서 임계구역을 수행

        ![__1](https://user-images.githubusercontent.com/60249222/126031462-c908488e-96e1-40b7-9f4f-bff305382cca.png)

        세마포를 도식화한 그림.

  - `acquire()`에 의해 block되는 프로세스는 세마포 내부에 있는 큐에 삽입
  - 다른 프로세스가 임계구역을 나오면서 `release()`를 호출 세마포 큐에 있는 프로세스를 깨움
  - 세마포는 일반적으로 상호 배타(Mutual Exclusion)를 구현하기 위해 사용된다.

## 4.1 은행 계좌 문제 - 세마포를 적용

- 처음 살펴본 은행 계좌 문제에 세마포를 적용 → 상호 배타를 구현한 코드는 아래와 같다.

  ```java
  import java.util.concurrent.Semaphore;  // 세마포를 사용하기 위해 파일 가장 위에 추가해야 한다.

  class BankAccount {
  	int balance;

  	Semaphore sem;
  	BankAccount() {   // BankAccount 클래스의 생성자가 호출되면 세마포를 만든다.
  		sem = new Semaphore(1);  // value 값을 1로 초기화한다.
  	}

  	void deposit(int amount) {
  		try {
  			sem.acquire();   // 임계구역에 들어가기를 요청한다.
  		} catch (InterruptedException e) {}
  	    /* 임계 구역 */
  		int temp = balance + amount;
  		System.out.print("+");
  		balance = temp;

  		sem.release();   // 임계구역에서 나간다.
  	}
  	void withdraw(int amount) {
  		try {
  			sem.acquire();
  		} catch (InterruptedException e) {}
  	    /* 임계 구역 */
  		int temp = balance - amount;
  		System.out.print("-");
  		balance = temp;

  		sem.release();
  	}
  	int getBalance() {
  		return balance;
  	}
  }
  ```

  - 세마포를 적용하기 위해서는 `value`값을 정해야 한다.
  - `value`는 몇 개의 프로세스를 접근할 것인지 정하는 것이다.
  - 현재는 상호 배타를 위해서 1로 초기화한다.
    - 즉, 하나밖에 프로세스 및 스레드가 오지 못한다.
  - 위와 같이 코드를 진행했을 때, 문제 없이 `balance = 0`이 나오는 것을 알 수 있다.

## 4.2 Ordering - 실행 순서 제어

- 세마포는 프로세스의 실행 순서를 제어할 수 있다.
- 프로세스 P1, P2를 구현하고, P1 > P2 순서로 실행되기를 원한다고 할 경우 아래와 같이 설정한다.

  ![__2](https://user-images.githubusercontent.com/60249222/126031481-a239f50a-eda2-4310-bfc4-2f27424ecb10.png)

- 세마포로 감싼 구역에 진입할 수 있는 프로세스를 결정하는 `value`값을 0으로 설정
- P1이 먼저 실행한 경우
  1. Section 1 실행
  2. `sem.release()`를 수행, `value`를 1 증가
  3. 세마포 큐에 있는 프로세스를 깨워줌
     - 프로세스가 존재하지 않기 때문에 아무 동작도 하지 않음
  4. P2 실행
  5. `sem.acquire()`를 수행, `value`의 값이 1이기에 감소해 0이 됨
  6. value가 0인 경우 block을 수행하지 않음, Section 2 수행
- P2가 먼저 실행한 경우
  1. Section 2 이전에 `sem.acquire()`가 있으므로 이를 수행
     - `value`값은 1인데 감소하여 0이 됨. `value`가 0이면 block
  2. P1 실행
  3. Section 1 수행
  4. `sem.release()`를 수행, value 값을 1 증가 → 세마포 큐에 있는 P2 프로세스를 깨워줌
     - value는 0으로 변경됨
  5. P2의 Section 2 수행
- 어떤 프로세스가 먼저 실행하든 P1이 먼저 실행하는 것을 알 수 있음

## 4.3 은행 계좌 문제 - Ordering 적용

- 은행 계좌 문제에 Ordering 적용할 경우

  - 프로세스는 입금 → 출금으로 수행한다 가정

  ```java
  class BankAccount {
  	int balance;

  	Semaphore sem, semOrder;
  	BankAccount() {
  		sem = new Semaphore(1);
  		semOrder = new Semaphore(0);   // Ordeing을 위한 세마포
  	}

  	void deposit(int amount) {
  		try {
  			sem.acquire();
  		} catch (InterruptedException e) {}
  		int temp = balance + amount;
  		System.out.print("+");
  		balance = temp;
  		sem.release();
  		semOrder.release();   // block된 출금 프로세스가 있다면 깨워준다.
  	}
  	void withdraw(int amount) {
  		try {
  			semOrder.acquire();   // 출금을 먼저하려고 하면 block한다.
  			sem.acquire();
  		} catch (InterruptedException e) {}
  		int temp = balance - amount;
  		System.out.print("-");
  		balance = temp;
  		sem.release();
  	}
  	int getBalance() {
  		return balance;
  	}
  }
  ```

- 입금, 출금 순으로 교차하여 진행하고자 할 경우

  ```java
  Semaphore sem, semDeposit, semWithraw;
  BankAccount() {
  	sem = new Semaphore(1);
  	semDeposit = new Semaphore(0);
  	semWithraw = new Semaphore(0);
  }

  void deposit(int amount) {
  	try {
  		sem.acquire();
  		int temp = balance + amount;
  		System.out.print("+");
  		balance = temp;
  		sem.release();
  		semWithraw.release();
  		semDeposit.acquire();   // 입금후에는 반드시 출금을 해야 하므로 자신을 block한다.
  	} catch (InterruptedException e) {}
  }
  void withdraw(int amount) {
  	try {
  		semWithraw.acquire();  // 입금보다 먼저 수행하는 것을 막는다.
  		sem.acquire();
  	} catch (InterruptedException e) {}
  	int temp = balance - amount;
  	System.out.print("-");
  	balance = temp;
  	sem.release();
  	semDeposit.release();  // 출금 수행이 완료되면 block되었던 입금 프로세스를 깨워준다.
  }
  int getBalance() {
  	return balance;
  }
  ```

---

# _참고문헌_

- 운영체제 KOCW 양희재 교수님 강의(youtube) <[https://www.youtube.com/channel/UCvC5uyGB_oVtTZU15SFkqWg](https://www.youtube.com/channel/UCvC5uyGB_oVtTZU15SFkqWg)>
- codemcd님의 정리 자료 <[https://velog.io/@codemcd/운영체제OS-8.-프로세스-동기화-1](https://velog.io/@codemcd/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-8.-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EB%8F%99%EA%B8%B0%ED%99%94-1)>
