## API TEST TOOL

api를 테스트 할수 있는 서비스이다.



## POSTMAN

* postman : https://www.postman.com/

* postman : api를 테스트 하는 툴 중 하나. 사용방법이 간단하다.

* 웹으로도 테스트가 가능하고 앱을 다운로드받아 사용할 수 도 있다.

  

* 사용방법 : get, post, put 등 전송방법을 선택하고 url로 요청하여 응답 결과를 쉽게 볼 수 있다.



* Params 탭 : 보통 get방식으로 많이 사용
  + key와 value를 입력하면 자동으로 쿼리스트링을 작성해준다.

* Body 탭 : 보통 post 방식으로 많이 사용
  + form-data : form태그로 묶어서 데이터를 보내는 방법과 마찬가지로 key와 value를 작성하여 데이터를 전송할 수 있다. file 전송도 테스트가 가능하다.
  + x-www-form-unlencoded : form-data 와 비슷한 방법이지만 영문자를 제외한 글자는 모두 인코딩한다. 파일은 전송 불가능하다.
  + raw  : 직접 코드 작성을 통해 데이터를 보낼 수 있다. JSON/XML 등 의 형태로도 작성이 가능하다.
  + binary : 파일을 전송 할 수 있다.

