신경망과 LSTM
====================
-------------------------


신경세포설에서 뉴런의 구조
-----------------
<img src="http://img.hani.co.kr/imgdb/resize/2016/0809/00503285_20160809.JPG?raw=true" width="25%" height="30%" title="px(픽셀) 크기 설정" alt="RubberDuck"></img>

- 인간의 몸에는 1천억개의 뉴런이 있다.
- 수상돌기(가지돌기)는 다른 뉴런으로부터 정보를 받는 통로이다.
- 세포체에서 정보 신호를 처리하고
- 축색돌기로 다른 뉴런의 수상돌기로 정보를 전달한다.
- 세포체와 수상돌기는 다른 뉴런들과 약 1000 개의 연접을 가진다.
- 뉴런은 전기신호, 신경전달물질을 다른 뉴런으로 보내면서 서로간에 상호작용한다.
- 각 뉴런은 신호를 받아들여서 처리한 후 다른 신호로 가공하여 다른 뉴런에 전달한다.

LSTM에서 LSTM cell 구조
--------------------------------
<img src="https://github.com/joohyoungkim19940805/imgRepository/blob/91d7065813b8b34fd8b9d3e2e5fb6e8c49b5c8cb/2222.JPG?raw=true" width="50%" height="40%" title="px(픽셀) 크기 설정" alt="RubberDuck"></img>

- LSTM의 단위적 구조이다
- h는 단기상태(short-term state) c는 장기상태(long-term state)이다.
- c는 셀의 왼쪽에서 오른쪽으로 이동하며 다른 LSTM cell로 상태값을 전달한다.
- 이때 c가 연결통로를 지나면서 기억할 부분, 삭제할 부분, 읽어들이는 부분을 연산하여 통로를 지난다.
- 사진의 forget gate 를 지나면서 일부 정보를 잃고 input gate에서 새로운 정보를 기억한다.
- c는 각 스텝을 지나면서 cell을 지날 때마다 기억을 삭제하고 추가하는 과정을 거쳐 output gate의 tanh 함수로 전달되어
- 단기 상태인 h와 셀의 출력값인 y를 만든다.

> Forget gate : f에 의해 c의 어느 부분을 삭제할지 결정한다.   
> Input gate : i에 의해 제어 되며 g의 어느 부분이 c에 더해져야 하는지 결정한다.   
> Output gate : c의 어느 부분을 읽어서 h와 y로 출력해야 할지 결정한다.   


### 감각정보의 신경학적 정보 처리 프로세스

<img src="https://github.com/joohyoungkim19940805/imgRepository/blob/92a4815313338d98ed9285c139f1f76532cbd6e6/%EC%BA%A1%EC%B2%98.JPG?raw=true?raw=true" width="25%" height="30%" title="px(픽셀) 크기 설정" alt="RubberDuck"></img>


### LSTM의 기억 정보 처리 프로세스

<img src="https://github.com/joohyoungkim19940805/imgRepository/blob/e974fef1859382f53a53a69040c213811baa847e/1111.JPG?raw=true" width="35%" height="30%" title="px(픽셀) 크기 설정" alt="RubberDuck"></img>

<img src="https://github.com/joohyoungkim19940805/imgRepository/blob/85d696cf1da5409e7d5e09c0328eec7481d07be7/12345.JPG?raw=true" width="40%" height="35%" title="px(픽셀) 크기 설정" alt="RubberDuck"></img>
