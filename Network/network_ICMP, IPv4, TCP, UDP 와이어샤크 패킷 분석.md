# ICMP, IPv4, TCP, UDP 패킷 분석

날짜: 2021년 5월 17일 → 2021년 5월 23일

- 3계층
    - IPv4 프로토콜
        - 서로 다른 네트워크 상에서 데이터를 교환하기 위한 프로토콜
        - 데이터를 멀리 보내는 용도
        - **데이터가 정확하게 전달되는 것을 보장하지 않음**
        - IPv4 프로토콜의 구조

            ![IP_4](https://user-images.githubusercontent.com/60249222/119250057-66629980-bbd8-11eb-8471-ce0def60e12f.png)


            IP Option은 붙을 수도, 붙지 않을 수도 있음, 최대 10개까지 결합 가능

            - 20 Byte
            - Version : IP 프로토콜의 버전
                - IPv4 → "4"
            - IHL(Header Length) : 헤더의 길이
                - 4 bit, 2진수 4개로 헤더의 길이를 표현 → 15까지만 표현 가능
                - 헤더의 길이는 최소 20 ~ 최대 60
                    - 따라서 IHL에는 4로 나눠서 헤더의 길이를 기재한다.

                    $$IHL = (헤더의 길이)/4$$

                    - 길이가 20인 경우 → 5를 기재(2진수로)
            - TOS(Type of Service) : 현재 사용하지 않음, "00"으로 기재
            - Total Length : 페이로드까지 합친 전체 길이
            - IP 조각화에 사용되는 정보
                - 데이터 전송에는 보낼 수 있는 최대 크기가 존재 → MTU(최대전송단위)
                - 송신측에서 패킷을 조각화하고 수신측에서 결합하여 데이터를 송수신
                    - Identification : 네트워크 패킷의 ID 값. 수신 측에서 패킷을 결합할 때 동일한 패킷인지 인지하기 위해 사용
                    - IP Flags : 3 bit → xDM으로 구성, 비트의 자리마다 역할이 다름
                        - x : 현재 사용하지 않음, 고정값 0
                        - D : 송신측에서 패킷을 조각화하지 않고 보낼때 사용하는 플래그, 사용시 "1" 세팅
                        - M : 패킷 조각화가 일어났음을 알려줌, 사용시 "1" 세팅
                    - Fragment Offset : 13 bit
                        - 조각화가 일어난 패킷의 상대적 위치(기준으로부터 떨어진 정도)를 알려줌
                        - 시작부분으로부터 얼마나 떨어져 있는지를 표시

                        $$Offset = (나누어진 패킷 데이터의 크기)/8$$

            - TTL(Time To Live) : 패킷이 사용되는 기간을 지정
                - 특정 숫자값을 지정하고 장비를 지날 때마다 -1
                - 0이 되어도 도착을 안하면 패킷을 버림
            - Protocal : 현재 패킷의 페이로드에 담긴 프로토콜 타입을 알려줌
                - ICMP : "1"
                - TCP : "6"
                - UDP : "17"
            - Header Checksum : 헤더의 오류유무를 확인
                - 송신측과 수신측의 체크섬을 확인하여 정상적으로 송수신 되었는지 확인
            - Source Address : 출발지 IP 주소
            - Destination Address : 목적지 IP 주소
        - IPv4 프로토콜 실습
            - ping 8.8.8.8

                ![ICMP_3 1](https://user-images.githubusercontent.com/60249222/119250072-87c38580-bbd8-11eb-95c2-a228a7ccfc7d.png)


                ICMP 프로토콜 캡쳐시 IPv4도 같이 캡쳐되므로 동일한 패킷을 사용

            - IPv4 상세 패킷

                ![IPv4_1](https://user-images.githubusercontent.com/60249222/119250081-9742ce80-bbd8-11eb-81ad-f99ced48bf7d.png)

                - Version : "0100"(4,  2진수 표기)
                - IHL : "5"(20 byte를 4로 나눈 값, 2진수 표기)
                - TOS → Differentiated Services Field로 변경됨, 사용하지 않음
                - Total Length : 페이로드까지 포함하여 84 byte
                - Identification : 패킷의 ID 값
                - Flags : 현재 패킷의 조각화 여부를 명시

                    ![IPv4_2](https://user-images.githubusercontent.com/60249222/119250085-9f027300-bbd8-11eb-953a-46423c32e2f9.png)

                    "4"는 조각화되어 있지 않음

                - Fragment offset : "0", 조각화 되어 있지 않음
                - TTL : "63"
                - Protocal : ICMP(1)
                - Header Checksum : wireShark 최신 버전에선 IPv4의 체크섬 부분을 확인하는 기능을 삭제
                - Source Address : 송신자 IP 주소
                - Destination Address : 수신자 IP 주소
        - IPv4의 조각화
            - 큰 IP 패킷들이 작은 MTU(Maximum Transmission Unit)를 갖는 링크를 통해 전송되려면 작은 패킷으로 조각화 되어 전송되어야 함
            - 즉, 각 라우터마다 전송에 적합한 크기로 변경되어야 함
            - IPv4의 헤더는 20 Byte 이므로 MTU - 20 Byte만큼으로 쪼개서 보내야 함
                - MTU가 1500이며, 전송되어야 하는 IPv4의 패킷 크기가 3000인 경우
                    - IPv4 헤더의 크기 20 byte 이므로 최대 패킷 크기는 1480 byte
                    - 3개의 패킷으로 조각화하여 전송
                    - Identification : 동일한 값을 기재
                    - Fragment : "001"로 세팅 ???
                        - Fragment는 3 bit로 구성됨
                        - More Flagment는 2진수로 "001"이나 16진수 표현으로 "0010"이 변경되어 "2"가 세팅
                    - Offset : 1480 / 8 → "185"
                        - Offset은 8로 나눈 몫을 기재
                        - 패킷의 상대적 위치에 따라서 값이 달라짐
                            - 2번째 패킷은 185
                            - 3번째 패킷은 370
    - ICMP 프로토콜
        - 네트워크 컴퓨터 위에서 돌아가는 OS에서 오류 메시지를 전송받는데 쓰임
            - IP는 패킷을 목적지에 전달하는 용도로만 사용하므로 단선되었거나 호스트가 꺼져있는 비정상적인 상황에서 알려줄 프로토콜이 필요
        - 프로토콜 구조의 Type과 Code를 통해 오류 메시지를 전송
        - ICMP 프로토콜 구조

            ![_001](https://user-images.githubusercontent.com/60249222/119250089-a7f34480-bbd8-11eb-9f2e-396f78508386.png)

            - Type : 대분류, 메시지 타입

                ![ICMP_2](https://user-images.githubusercontent.com/60249222/119250094-b04b7f80-bbd8-11eb-88a2-400724a1383e.png)

                - 꼭 알아야 하는 타입
                    - 0 : 응답
                    - 3 : Destination Unreachable(목적지에 도달할 수 없음)
                        - 경로의 문제로 인해 목적지에 도착 X
                    - 8 : 요청
                    - 5 : 경로 변경, 최적의 경로가 탐색되었을 경우 패킷이 최적의 경로로 보내질 수 있도록 유도
                    - 11 :  Time Exceeded(요청 시간 만료)
                        - 목적지에 도착하였으나 응답을 못받음, 상대방이 문제 → 주로 방화벽
            - Code : 소분류
            - Checksum :  패킷 송수신 확인
        - ICMP 프로토콜 실습
            - ping 8.8.8.8

                ![ICMP_3 1](https://user-images.githubusercontent.com/60249222/119250101-b9d4e780-bbd8-11eb-870c-3b879ce79ff9.png)

            - ICMP 상세 패킷

                ![ICMP_4](https://user-images.githubusercontent.com/60249222/119250103-c0fbf580-bbd8-11eb-9789-e06e06c1d42b.png)

                뒤에 DATA가 48 Byte가 오는데 쓸모 없는 데이터

                ![ICMP_5](https://user-images.githubusercontent.com/60249222/119250109-c9ecc700-bbd8-11eb-983a-faecab82e52b.png)

                수신 패킷, Type이 "0"으로 정상적으로 응답을 받음

                - Type : "8" → 요청
                - Code : 0
                - Checksum → 패킷 송수신 확인 용도
    - **다른 네트워크와 통신 절차**
        - A 호스트에서 B 호스트로 통신하는 과정
        - A와 B는 다른 네트워크 대역
            1. A에서 ICMP 프로토콜 작성
                - A의 라우팅 테이블을 확인
                    - cmd에 netstat -r 을 통해서 확인 가능

                        ![ipv4_3](https://user-images.githubusercontent.com/60249222/119250116-d2450200-bbd8-11eb-9eb5-8bba5bac1d6b.png)

            2. A 호스트의 라우팅 테이블에 B의 정보가 없음을 확인
                - 다른 네트워크 대역이기 때문
                - 프로토콜을 "0.0.0.0"으로 송신
                    - "0.0.0.0" 은 컴퓨터가 기록하고 있는 라우팅 테이블 정보가 아닌 요청이 들어올 경우에 요청을 보냄 → 게이트웨이(라우터)
            3. A 라우터에서 IPv4 프로토콜을 확인
                - 다른 네트워크 대역이므로 해당 라우팅 테이블에 B 호스트의 IP 주소가 없음
                    - "0.0.0.0"으로 B 호스트의 라우터로 송신 필요
                - 이더넷은 LAN에서만 통신하므로 이더넷 프로토콜을 B 호스트의 라우터로 새로 작성
            4. B 라우터에서 해당 프로토콜을 수신
                - B 라우터의 라우팅 테이블에는 같은 네트워크 대역인 B 호스트의 MAC 주소 혹은 IP 주소를 알고 있음
                - MAC 주소를 알 경우 이더넷 프로토콜을 B 호스트의 MAC 주소로 새로 작성
                - MAC 주소를 모를 경우는 ARP 프로토콜을 사용하여 MAC 주소를 확인
                - 이더넷 프로토콜을 B 호스트에 전송
            5. B 호스트에서 ICMP 프로토콜 수신, ICMP 응답 프로토콜(Type : 0)로 변경하여 패킷 전송
            6. A 호스트까지 ICMP 응답 프로토콜이 도달하여 패킷 송수신이 이루어짐
- 4계층
    - 전송 계층
    - 송신자의 프로세스와 수신자의 프로세스를 연결
        - 메모리에 동작중인 프로그램을 연결
    - 특정 프로세스를 식별하기 위해 **포트 번호**를 사용
    - 포트 번호의 종류
        - Well Known 포트(잘 알려진 포트)
            - 0 ~ 1024번

            ![port_1](https://user-images.githubusercontent.com/60249222/119250120-dc670080-bbd8-11eb-8bd1-915fae05cf86.png)

        - Registered 포트(예약된 포트)

            ![port_2](https://user-images.githubusercontent.com/60249222/119250125-e1c44b00-bbd8-11eb-9c1a-e83174887590.png)

        - Dynamic 포트(일반 사용자들이 사용)
            - 49152번 ~ 65535번 포트
    - TCP 프로토콜
        - 전송 제어 프로토콜(Transmission Control Protocal, TCP)
        - 연결 지향형(안전한 연결을 지향하는) 프로토콜
            - 안정적, 순서대로, 에러 없이
        - TCP 프로토콜의 구조

            ![TCP_1](https://user-images.githubusercontent.com/60249222/119250128-e852c280-bbd8-11eb-8967-cd3ddd67842b.png)

            - 20 byte ~ 최대 60 byte
                - TCP Options에 의해 달라짐
            - Offset : 헤더의 길이, 4로 나눔

                $$Offset = (헤더길이)/4$$

            - Reserved : 예약된 필드, 사용하지 않음
            - TCP Flags : TCP의 주된 기능을 명시
                - C : Congestion Window Reduced, t사용 x
                - E : ECN-Echo, 사용 x
                - U : Urgent Flag, 긴급 비트
                    - 우선 순위가 높은 데이터가 포함되어 있음을 명시
                - A : Acknowledgment Flag, 승인 비트
                - P : Push Flag, 밀어넣기 비트
                    - TCP 버퍼가 일정 크기 만큼 쌓여야 전송하는데, 크기 상관 없이 데이터를 밀어넣을 때 사용
                - R : Reset Flag, 초기화 비트
                    - 연결된 상태에서 문제 발생 → 연결 관계를 리셋할 때 사용
                - S : Syn Flag, 동기화 비트
                    - 상대방과 연결 시작시 사용, S가 보내지고 데이터 전송
                - F : Fin Flag, 종료 비트
                    - 데이터를 전부 주고 받고 연결을 종료할 떄 사용
            - Window : 보내야 하는 TCP 버퍼 공간을 알려줌
            - Checksum : 체크
            - Urgent Pointer :  TCP Flags의 "U"와 같이 사용, 긴급 데이터의 위치 값을 명시
        - TCP 프로토콜의 연결 과정
            - 연결 수립 과정 : TCP를 이용한 데이터 통신을 할 때 프로세스와 프로세스를 연결하기 위해 가장 먼저 수행되는 과정
                1. 클라이언트가 서버에게 요청 패킷을 전송
                2. 서버가 클라이언트의 요청을 받아들이는 패킷 전송
                3. 클라이언트는 이를 최종 수락하는 패킷 전송

                ⇒ 위 3개의 과정을 **3 Way Handshake**라고 한다.

                ![TCP_2](https://user-images.githubusercontent.com/60249222/119250136-f30d5780-bbd8-11eb-8def-d99ade64719b.png)

                3 Way Handshake를 도식화

            - 데이터 송수신 과정
                - 연결 수립 과정이 종료된 후, 클라이언트에서 서버와 통신이 시작
                - 송신측은 SEQ, ACK 번호를 그대로 사용해서 통신
                - 수신측 SEQ 번호 = 받는 ACK 번호가 됨
                - 수신측 ACK 번호 = 받은 SEQ 번호 + 받은 데이터의 크기

                ![TCP_3](https://user-images.githubusercontent.com/60249222/119250142-f9033880-bbd8-11eb-87c3-e77a41be4b04.png)

        - TCP 연결 상태의 변화
            - TCP 상태전이도

                ![TCP_4](https://user-images.githubusercontent.com/60249222/119250147-ff91b000-bbd8-11eb-9d76-38cd3ce829bf.png)

                여러가지 상태가 존재하나 가장 중요한 상태는 LISTEN/ESTABLISDHED

            - LISTEN : 연결을 위해 포트를 열어둔 상태
            - ESTABLISHED : 연결이 수립된 상태, 통신 가능 상태

                ![TCP_5](https://user-images.githubusercontent.com/60249222/119250152-07515480-bbd9-11eb-92c3-7d0c7cfbece8.png)

            TCP 연결 수립 과정의 진척도에 따라 상태 전이

        - TCP 프로토콜 실습
            - Chrome 중 패킷 캡쳐

                ![TCP_6](https://user-images.githubusercontent.com/60249222/119250154-0ddfcc00-bbd9-11eb-85b0-cd1308783317.png)

                ![TCP_7](https://user-images.githubusercontent.com/60249222/119250158-146e4380-bbd9-11eb-9e42-d7b1cb140fb7.png)

                현재 TCP 패킷 송수신을 그래프로 시각화

            - 상위 3개 패킷은 3 Way Handshake
                - Flag가 [SYN], [SYN. ACK]. [ACK}
            - 이후에 상태가 ESTABLISDHED가 되면서 클라이언트쪽에서 다시 데이터 송수신을 진행

            ![TCP_9](https://user-images.githubusercontent.com/60249222/119250163-1932f780-bbd9-11eb-99f5-bb99b7284ee5.png)

            - Source Port : 4821
            - Destination Port : 80(web server)
            - Sequence Number : 이전 3 Way Handshake가 종료된 값을 가져옴
            - Acknowledgment Number : 이전 3 Way Handshake가 종료된 값을 가져옴

                → Wireshark가 임의의 값을 사용자가 보기 편하게 SEQ, ACK 값을 변경시킴

            - Flags : PSH, ACK
                - 12 bit 값에서 ACK, PSH인 0000 0001 1000(2)로 세팅 : 24
                - 24를 16진수로 변경하면 18(16)
            - TCP의 DATA인 TCP payload는 HTTP 프로토콜
    - UDP 프로토콜
        - 사용자 데이터그램 프로토콜(User Datagram Protocal, UDP)
        - 비연결지향형(안전한 연결을 지향하지 않는) 프로토콜
            - 전송 방식 단순
            - 신뢰성 낮음
        - UDP 프로토콜을 사용하는 프로그램
            - DNS 서버
                - 도메인 IP 확인
            - tftp 서버
                - 파일 전송
        - UDP 프로토콜의 구조

            ![UDP_1](https://user-images.githubusercontent.com/60249222/119250166-20f29c00-bbd9-11eb-89c6-592c78f4cd23.png)

            - Source Port : 출발지 포트
            - Destination Port : 목적지 포트
            - Length : 패킷의 길이
            - Checksum : 체크섬, 확인
        - UDP 프로토콜 실습
            - chrome 웹서핑 중 패킷 캡쳐

                ![UDP_2](https://user-images.githubusercontent.com/60249222/119250169-26e87d00-bbd9-11eb-8501-3e9225838c08.png)

                - Source Port : 443
                    - 출발지 포트 : https
                    - 59.18.30.83 → KT IP
                        - KT에서 패킷을 전송
                - Destination Port : 53554
                    - 목적지 포트 : 호스트에서 UDP 프로토콜을 보낼 때 사용했던 포트
                - Length : 1358
                    - 페이로드의 데이터 크기 + UDP 헤더 크기 → 1358 byte
