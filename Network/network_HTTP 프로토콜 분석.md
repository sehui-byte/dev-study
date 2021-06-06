- NAT
    - Network Address Translation : IP 패킷의 TCP/UDP 포트 숫자와 소스 및 목적지의 IP 주소 등을 재기록하면서 라우터를 통해 네트워크 트래픽을 주고 받는 기술
        - 엄밀히 말하면 NAT 와 PAT로 구분
            - NAT : IP를 변경, 가정의 경우 사설 IP와 공인 IP를 변경
                - 사설 IP 호스트들이 동일한 목적지를 가질 경우 구분 X
            - PAT : NAT 테이블에 출발지 port 까지 기재
                - 사설 IP 호스트들이 동일한 목적지를 가지고 있다고 해도, port로 구분 가능
            - 일반적인 홈 네트워크는 PAT를 사용
    - 사설 IP 와 공인 IP를 변경하는데 사용
        - IP 주소를 절약
        - 보안 강화
    - NAT 절차(사설 IP에서 외부 IP와 송수신할 경우)
        1. 패킷 헤더에 출발지와 목적지의 주소를 기재, 출발지는 자신의 사설망 IP 주소를 기록
            - PC(호스트)에서 출발
                - 출발지 IP 주소 : 10.0.0.1
                - 목적지 IP 주소 : 200.100.10.1
        2. 기본 게이트웨이(공유기 등)에서 외부로 나가는 패킷을 인식할 경우, 출발지 IP 주소를 게이트웨이 자신의 공인 IP 주소로 변경. 이때 별도의 NAT 테이블을 보관
            - 기본 게이트웨이에서 다시 출발
                - 출발지 IP 주소 : 10.0.0.1 → 150.150.0.1 (재기록하여 변경)
                - 목적지 IP 주소 : 200.100.10.1
                - NAT 테이블에 아래 기록 추가

                    ![NAT_!](https://user-images.githubusercontent.com/60249222/120068054-ab476e00-c0b9-11eb-9fcc-1758558153aa.png)


        3. 웹 서버에서 수신한 데이터 처리 후, 응답 패킷에 출발지, 목적지 IP 기재
            - 웹 서버(200.100.10.1)에서 출발
                - 출발지 IP 주소 : 200.100.10.1(웹 서버)
                - 목적지 IP 주소 : 150.150.0.1(호스트의 게이트웨이 주소))
        4. 호스트의 게이트웨이에서 패킷 수신, 기록한 NAT 테이블 참조하여 최종 목적지인 호스트의 사설 IP 주소로 변경하여 해당 호스트로 패킷 전송
            - 기본 게이트웨이 패킷 수신 후 IP 변경 & 출발
                - 출발지 IP 주소 : 200.100.10.1(웹 서버)
                - 목적지 IP 주소 : 150.150.0.1(게이트웨이) → 10.0.0.1(호스트, 사설 IP 주소)
- 포트포워딩
    - 특정 IP 주소와 포트 번호의 통신 요청을 특정 다른 IP와 포트 번호로 넘겨주는 기술
    - 공인 IP 의 PORT로 보내면 해당 공유기가 다른 특정 IP의 특정 포트로 전송
- 7계층
    - HTTP 프로토콜
        - 웹을 만드는 다양한 기술이 존재
            - 필수 요소
                - HTTP(HTTPS → SSL/TLS)
                    - HTML, JavaScript, CSS 등, Client Side Script를 받아오는 역할
                - HTML
                - JavaScript
                - CSS
                - JSP
                    - Server Side Script, Server 쪽에서 동작하는 것
                    - 서버쪽에서 실행하고 그 결과만 받아옴
                - DB
            - 선택 요소
                - Python
                - Spring
                - Jquery
                - Ajax 등
        - HTTP 프로토콜의 특징
            - www에서 쓰이는 핵심 프로토콜. 문서의 전송에 쓰임. 음성, 화상 등 MIME으로 정의하여 전송 가능
            - Request/Response 동작에 기반하여 서비스 제공
            - HTTP 1.0 특징
                - 연결 수립, 동작, 연결 해제의 단순함
                    - 하나의 URL은 하나의 TCP 연결만 가능
                    - HTML 문서를 전송 받은 뒤 연결을 끊고 다시 연결하여 데이터를 전송
                    - 단순 동작(연결 수립, 동작, 해제)이 반복, 통신의 부하 발생
            - HTTP 1.1 특징
                - 3 Way Handshake로 연결이 수립되었으면 HTTP 프로토콜을 통해 지속적으로 데이터를 주고 받을 수 있음
                - 통신시에 연결 수립과 해제가 1회만 이루어짐
        - HTTP 요청 프로토콜

            ![HTTP_요청_1](https://user-images.githubusercontent.com/60249222/120068131-1b55f400-c0ba-11eb-8b1a-08f5e5cc2a42.png)


            HTTP 요청 프로토콜 도식화

            ![HTTP_요청_2](https://user-images.githubusercontent.com/60249222/120068141-2a3ca680-c0ba-11eb-8c7f-da1ad780ef85.png)

            가장 간단한 GET 방식의 HTTP 요청 프로토콜 첫번째부터 Request line, Headers, 공백, Body. 현재 Body 부분은 비어있다.

            - Request Line

                ![HTTP_요청_3](https://user-images.githubusercontent.com/60249222/120068148-3163b480-c0ba-11eb-8ea8-400030af92dd.png)

                - 요청타입

                    ![HTTP_요청_4](https://user-images.githubusercontent.com/60249222/120068155-39235900-c0ba-11eb-92ff-28bf8765dca8.png)

                    - 사실상 GET, POST만 사용
                        - GET, POST 둘 다 요청, 데이터 전송 가능
                            - GET
                                - 데이터를 서버로 보낼때 URI에 포함해서 보냄
                            - POST
                                - 데이터를 서버로 보낼때 BODY에 포함시켜 보냄
                    - 나머지 부분들은 보안상 문제로 보통 막아놓음
                - URI(Uniform Resource Identifier)
                    - 인터넷 상에서 특정 자원(파일)을 나타내는 유일한 주소
                    - URI 의 구조
                        - scheme :// host[:port][/path][?query]
                            - scheme : 요청 형식 지정
                                - ex) ftp, http 요청 형식 지정
                            - host : IP 주소 기입
                                - DNS 서버가 도메인주소를 입력하면 IP 주소로 변환
                            - port : port 번호 기입
                                - 웹브라우저에서 URI 입력시, port 번호를 기재하지 않을 경우 80번 포트로 자동 기입
                            - path : 해당 자원이 위치한 폴더와 파일의 이름
                            - ?query : 전송할 데이터 값 key & value 형식
            - HTTP 요청 프로토콜 실습
                - telnet으로 진행

                    ![HTTP_요청_6](https://user-images.githubusercontent.com/60249222/120068173-4c362900-c0ba-11eb-8ab2-1f2cc9a16448.png)

                    1. telnet으로 www.google.com 80 으로 연결
                    2. HTTP 요청 프로토콜 작성
                        - GET / HTTP/1.1
                    3. 응답결과 확인
        - HTTP 응답 프로토콜
            - HTTP 응답 프로토콜 구조

                ![HTTP_응답_1](https://user-images.githubusercontent.com/60249222/120068176-52c4a080-c0ba-11eb-95a1-d72dd14d694f.png)

                ![HTTP_응답_2](https://user-images.githubusercontent.com/60249222/120068182-59531800-c0ba-11eb-8b6d-8c2986e7545f.png)

                실제 응답 프로토콜 구조. HTTP/1.1 200 OK 가 Statuc Line에 해당

                - Status Line

                    ![HTTP_응답_3](https://user-images.githubusercontent.com/60249222/120068196-63751680-c0ba-11eb-8b76-e1c742b80f3e.png)

                    - 상태코드

                        ![HTTP_응답_4](https://user-images.githubusercontent.com/60249222/120068208-6a9c2480-c0ba-11eb-9ed8-ce1a380ec9c6.png)

                        - 200 ~ : 성공
                        - 400 ~ : 에러, 클라이언트 문제
                            - 403, Forbidden : Client가 권한이 없는 페이지를 요청할 경우
                            - 404, Not Found : Server에 없는 페이지를 요청할 경우
                        - 500 ~ : 에러, 서버 문제
                            - 500, Internal Server Error : Server의 일부가 멈췄거나 설정 오류
                            - 503, Service Unavailable : 최대 Session 수를 초과했을 경우
        - HTTP 헤더 포맷
            - HTTP 헤더는 공통으로 사용하는 일반 헤더, 요청 프로토콜에서 사용하는 요청 헤더, 응답 프로토콜에서 사용하는 응답 헤더로 분류
            - 일반 헤더

                ![HTTP_헤더포맷_1](https://user-images.githubusercontent.com/60249222/120068223-7556b980-c0ba-11eb-89d3-3bd1226e1353.png)

            - 요청 헤더

                ![HTTP_헤더포맷_2](https://user-images.githubusercontent.com/60249222/120068235-7b4c9a80-c0ba-11eb-8e7c-9b2a5fd57520.png)

                - User-Agent : Client의 프로그램 정보를 Server에게 전달
                    - 접속 플랫폼이 PC인지, Mobile인지 확인 가능
            - 응답 헤더

                ![HTTP_헤더포맷_3](https://user-images.githubusercontent.com/60249222/120068241-830c3f00-c0ba-11eb-837b-52636f90c0ab.png)
                
                
<hr/>

_참고문헌_

따라하면서 배우는 IT 네트워크 기초(개정판) : <https://www.youtube.com/channel/UCl9zTDOvOxdCfUt1HqVwwdg>

STEVEN J. LEE님의 블로그 : <https://www.stevenjlee.net/2020/07/11/%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-nat-network-address-translation-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%A3%BC%EC%86%8C-%EB%B3%80%ED%99%98/>
