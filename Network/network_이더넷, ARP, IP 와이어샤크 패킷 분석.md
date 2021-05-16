## 학습목표

- [ ]  데이터 통신에 사용되는 패킷과 PDU의 개념을 알 수 있다.
- [ ]  2계층 프로토콜인 이더넷 프로토콜을 분석할 수 있다.
- [ ]  3계층 프로토콜 중 하나인 ARP 프로토콜을 분석할 수 있다.

---

## 패킷

- 패킷
    - 네트워크 상에서 전달되는 데이터를 통칭하는 말. 네트워크에서 전달되는 데이터의 형식화된 블록
    - 패킷은 또한 3계층 PDU의 명칭으로 부르기도 함. 그러나 일반적으로 패킷은 네트워크 상에 전달되는 데이터를 아우르는 용어로 사용됨
    - PDU : 프로토콜 데이터 단위, 데이터를 부르는 명칭
    - 패킷은 제어 정보와 사용자 데이터로 이루어지며 사용자 데이터는 페이로드라고 한다.
        - 여러가지 프로토콜의 조합으로 이루어짐
        - 헤더 + 페이로드 + 풋터로 이루어짐
            - ex) Ethernet + IPv4 + TCP + HTTP
            - 이더넷(헤더) + 페이로드(IPv4 + TCP + HTTP) ⇒ PDU : 프레임
            - IPv4(헤더) + 페이로드( TCP + HTTP) ⇒ PDU : 패킷
            - TCP(헤더) + 페이로드(HTTP) ⇒ PDU : 세그먼트
                - 페이로드에 헤더를 붙이는 과정 → 캡슐화, 데이터를 보냄
                - 패킷을 까는 과정을 데이터를 받음

## 2계층 

- 2계층
    - 하나의 네트워크 대역(LAN), 같은 네트워크 상에 존재하는 여러 장비들 중에서 어떤 장비가 어떤 장비에게 보내는 데이터를 전달
    - 추가적으로 오류제어, 흐름제어 수행
    - 다른 네트워크와 통신할 때에는 3계층의 도움이 필요
    - MAC : LAN에서 통신할 때 사용하는 주소, 물리적인 주소
        - OUI(IEEE에서 부여하는 제조회사 식별 ID) + 고유번호(제조사에서 구여한 고유 번호) ⇒ 16진수로 이루어짐, 6Byte
            - ex) 6C:29:95:04:EB:A1
    - 이더넷 프로토콜

        ![__1](https://user-images.githubusercontent.com/60249222/118391740-dfaa3b80-b670-11eb-9b10-6b1b81889f33.png)

        - 1줄은 4 바이트
        - Destination Address : 목적지 주소 ⇒ 6 Byte
            - 목적지 MAC 주소가 온다.
        - Source Address : 출발지 주소 ⇒ 6 Byte
            - 출발지 MAC 주소가 온다.
        - Ethernet Type : 이더넷 타입 ⇒ 4 Byte
            - 페이로드(DATA) 내부에 프로토콜 정보(3계층 프로토콜)를 알려줌
                - IPv4 or ARP
                    - IPv4 ⇒ 0800
                    - ARP ⇒ 0806
        - 실습하기
            1. ping 8.8.8.8 로 Google DNS 서버와 통신

                ![__2](https://user-images.githubusercontent.com/60249222/118391816-31eb5c80-b671-11eb-99c9-adb47a33f258.png)

                패킷 전송

            2. 와이어샤크 패킷 캡쳐

                ![__3](https://user-images.githubusercontent.com/60249222/118391827-3ca5f180-b671-11eb-8ebb-399a32c2fa32.png)

                - 172.30.1.49 ⇒ 8.8.8.8 로 패킷 전송
            3. 패킷 분석
                - 해당 패킷 리스트 중에서 Request 패킷 분석

                ![__4](https://user-images.githubusercontent.com/60249222/118391841-47f91d00-b671-11eb-9171-f28038ec71b7.png)

                Ethernet, IPv4, ICMP까지 포함한 패킷이 캡쳐됨

                - Ethernet 프로토콜을 클릭하면 Ethernet에 속하는 패킷들이 활성화된다.
                    - 목적지 MAC 주소 : 88 : 3c : 1c : 7a : 89 : 30
                    - 출발지 MAC 주소 : 24 : f5 : aa : 74 : 24 : d9 ⇒ 노트북

                        ![__5](https://user-images.githubusercontent.com/60249222/118391863-68c17280-b671-11eb-9aba-5bcb8fb96639.png)

                        cmd에 ipconfig /all을 입력하면 MAC 주소를 알 수 있다.

                    - 이더넷 타입 : 0800 ⇒ 이더넷 패킷의 페이로드는 IPv4임을 알 수 있다.

## 3계층

- 3계층
    - LAN 과 LAN, 서로 다른 네트워크 대역을 연결
    - 멀리 떨어진 곳에 존자해는 네트워크에 데이터를 전달
    - IP 주소 체계 사용
        - IPv4 : 32 bit
        - IP 주소 : 현재 PC에 할당된 IP 주소
        - 서브넷 마스크 : IP 주소에 대한 네트워크 대역 규정
        - 기본 게이트웨이 : 외부와 통신할 때 사용하는 네트워크의 출입구
        - IP 주소의 역사
            - ClassFull IP 주소 : 네트워크 대역 8 bit 단위로 나누고 클래스별로 지정
                - A, B, C, D, E 클래스 존재
                    - D, E 클래스는 멀티캐스트, 연구용 목적으로 사용
                - 사용 가능한 IP 범위가 제한적임
                    - A~C클래스를 네트워크 대역으로 설정하는 경우, 사용 가능한 IP는 0 ~ 255로 무조건 결정
                    - 사용 가능한 IP에 대한 탄력적인 조절이 불가능
            - ClassLess IP 주소 :  네트워크 대역을 클래스 단위가 아니라 bit 단위로 쪼개서 네트워크 대역과 IP 주소를 지정할 수 있음
                - 낭비되는 IP 주소를 줄일 수 있음
            - 사설 IP와 공인 IP
                - 공인 IP : ISP에서 제공하는 IP 주소
                    - 실제 외부와 통신하기 위해서 사용하는 IP 주소
                - 사설 IP : 라우터를 사용하여 공인 IP 하나에 새로운 네트워크 대역을 생성
                    - 라우터를 통해서 네트워크 대역을 생성하면 하나의 공인 IP 주소로 네트워크 대역을 생성하여 여러 호스트가 외부 네트워크와 통신할 수 있다.
                    - 사설 IP에서 외부 네트워크와 통신하기 위해 특정 IP를 특정 IP로 변환하는 것을 NAT라고 한다.
                    - 사설 IP가 외부 네트워크와 통신시에는 먼저, NAT Table에 해당 사설 IP를 기록하고, 외부 네트워크에 응답이 올 때 NAT Table 기록을 보고 해당 사설 IP로 데이터를 전달한다.

                    ![3](https://user-images.githubusercontent.com/60249222/118391881-7c6cd900-b671-11eb-985e-fc79ae2356ba.png)

    - ARP
        - ARP 프로토콜 : 같은 네트워크 대역에서 통신을 하기 위해 필요한 MAC 주소를 IP 주소를 이용해 찾아내는 프로토콜.

            ![ARP_1](https://user-images.githubusercontent.com/60249222/118391887-88589b00-b671-11eb-91f9-dfee1ba6a394.png)

        - 28 Byte
        - Hardware type : 2계층 프로토콜 타입
            - 거의 고정값
            - 대부분 이더넷 프로토콜이 옴
            - 0001로 이더넷 프로토콜임을 명시
        - Protocal type : 프로토콜 타입
            - 거의 고정값
            - 대부분 IPv4이 옴
            - 0008로 IPv4를 명시
        - Hardware Address Length : 하드웨어 주소 길이 ⇒ MAC 주소의 길이
            - 거의 고정값
            - MAC 주소의 길이, Byte 단위
            - 06로 명시
        - Protocal Address Length : 프로토콜 주소 길이 ⇒ IPv4 주소의 길이
            - 거의 고정값
            - IP 주소의 길이, Byte 단위
            - 04로 명시
        - Opcode : 오퍼레이션 코드
            - 요청인지, 응답인지 보여주는 코드
            - 0001 ⇒ 요청
            - 0002 ⇒ 응답
        - Source Hardware Address : 출발지 MAC 주소
        - Source Protocal Address : 출발지 IPv4 주소
            - 10진수인 IP주소를16진수로 표기
        - Destination Hardware Address : 목적지 MAC 주소
        - Destination Protocal Address : 목적지 IPv4 주소
            - 10진수인 IP주소를16진수로 표기

        - ARP 요청 프로토콜 절차
            1. ARP 요청 프로토콜 생성
                - 목적지 MAC 주소를 모르기 때문에 Destination Hardware Address에 00 00 00 00 00 00으로 세팅
                - 이더넷 프로토콜도 목적지 MAC 주소를 모르기 때문에 FF : FF : FF : FF : FF : FF로 작성해서 송신
                    - FF : FF : FF : FF : FF : FF 는 브로드캐스트 ⇒ 동일 네트워크 대역에 있는 모든 호스트에게 송신
            2. 스위치(2계층 장비)에 ARP 프로토콜 송신
            3. 스위치에서 목적지 MAC 주소가 브로드캐스트를 확인 후, 동일 네트쿼의 대역의 모든 호스트에게 송신
            4. 호스트에서 ARP 프로토콜을 수신 후 IP 주소를 확인하여 일치할 경우 ARP 응답 프로토콜을 작성해서 송신
                - Opcode ⇒ 0002
                - Source Hardware Address ⇒ 본인 MAC 주소
            5. 스위치를 통해 ARP 요청 프로토콜을 생성한 호스트로 ARP 응답 프로토콜 전달
            6. ARP 응답 프로토콜을 받는 호스트는 ARP 캐시 테이블에 IP와 MAC 주소를 저장

                ![arp_](https://user-images.githubusercontent.com/60249222/118391899-95758a00-b671-11eb-8faf-8b860b346c74.png)

                cmd > arp -a

            7. 이후 통신 시작
        - ARP 실습
            1. 와이어샤크 실행 & ARP 캡쳐

                ![ARP_2](https://user-images.githubusercontent.com/60249222/118391905-9d352e80-b671-11eb-8a02-7c35cf5ab816.png)

            2. ARP 요청 프로토콜 상세보기

                ![ARP_3](https://user-images.githubusercontent.com/60249222/118391909-a3c3a600-b671-11eb-9374-9efc5b27018e.png)

                - Hardware type : Ethernet
                    - 00 01
                - Protocal type : IPv4
                    - 08 00
                - Hardware size : 하드웨어 MAC 주소 길이
                    - 06
                - Protocal size : 프로토콜 주소 길이
                    - 04
                - Opcode : 오퍼레이션 코드
                    - 요청 ⇒ 00 01
                - Sender MAC address(출발지 MAC 주소)
                    - 88:3c:1c:7a:89:30
                - Sender IP address(출발지 IP 주소)
                    - 172.30.1.254
                - Target Mac address(목적지 MAC 주소)
                    - 00:00:00:00:00:00
                    - 목적지 MAC 주소를 모르기 때문에 비워둠
                - Target IP address(목적지 IP 주소)
                    - 172.30.1.49
