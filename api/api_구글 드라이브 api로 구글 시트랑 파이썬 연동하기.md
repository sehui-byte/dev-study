구글 드라이브 api로 구글 시트랑 파이썬 연동하기
---------------------------------------------------

##준비물   

### 1.구글 드라이브 api 신청하기   

https://console.cloud.google.com/apis/library   

- 접속 후 리스트 중간에 있는 Google Drive API 선택   
    <br/>  
<img src="https://lh3.googleusercontent.com/f4sDhGNGFnO38pNp-LVDXlpQmlr3kDE3RNciaImc_lUDpFysmw_9JI_KI1tiv6v0RYX0MErO0wxFuNJ0aawx5w?raw=true" width="20%" height="25%" title="px(픽셀) 크기 설정" alt="RubberDuck"></img>   
   
> 리스트에서 이렇게 생긴 걸 선택하면 된다.   
<br/>  

- 사용 클릭  
    <br/>  
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FIGVuQ%2FbtqOle328Cs%2FJGwb5QlYRv7pJ7nYky5htk%2Fimg.png?raw=true" width="50%" height="40%" title="px(픽셀) 크기 설정" alt="RubberDuck"></img>   
 <br/>    
     <br/>  
- 사용자 인증 정보 만들기 클릭   
    <br/>  
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F9VxdN%2FbtqOiByKIZs%2FQ8fSj8XARuEgT6bDhCX1LK%2Fimg.png?raw=true" width="50%" height="40%" title="px(픽셀) 크기 설정" alt="RubberDuck"></img>   
   
> 구글 드라이브 api 선택 그 후 적당히 아무렇게나 그럴싸하게 입력하면 된다.   
<br/>  
- 1.사용자 인증 정보 탭으로 들어간다.   
- 2.상단에 +사용자인증정보만들기 버튼을 클릭한 후 서비스 계정 선택   
   <br/>  
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmIO0W%2FbtqOpqJdwQu%2FsShICK70kLvDINGjJ1rUs0%2Fimg.png?raw=true" width="50%" height="40%" title="px(픽셀) 크기 설정" alt="RubberDuck"></img>   
<br/>  
   
- 엑세스 권한은 소유자, 뷰어, 탐색자, 편집자를 선택한다.   
- 사실 소유자만 선택해도 무방해보이지만 다시 하기 귀찮으니까 다 선택해준다. 
     <br/>  
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbuNIo7%2FbtqOnZ6oMtT%2FUmkswfpK0QeBVuCBRKAfNK%2Fimg.png?raw=true" width="50%" height="40%" title="px(픽셀) 크기 설정" alt="RubberDuck"></img>   
<br/>     

- 이름을 입력하면 자동으로 이메일을 생성해준다. 혹시 모르니까 어디다 저장해놓으면 좋다.   
      <br/>  
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FuxwzO%2FbtqOiAT6SXe%2FAluhfjkkOoABcfNwlVhOyk%2Fimg.png?raw=true" width="50%" height="40%" title="px(픽셀) 크기 설정" alt="RubberDuck"></img>   
<br/>  
   
- 구글 시트에서 공유 버튼을 누른 후   
- 위에서 자동으로 생성된 이메일을 input box에 넣어준다.   
- 완료 버튼을 누르면 된다.   
   <br/>  
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbkKuAm%2FbtqOiAzNTx2%2FUFYGb0CFv0sqaTAftMBJn1%2Fimg.png?raw=true" width="50%" height="40%" title="px(픽셀) 크기 설정" alt="RubberDuck"></img>   
   <br/>  
   

- 서비스 계정 생성을 완료하면 해당 화면에서 서비스 계정이 추가된 것이 보인다.   
- 생성된 자신의 서비스 계정을 누른다.   
       <br/>  
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmIO0W%2FbtqOpqJdwQu%2FsShICK70kLvDINGjJ1rUs0%2Fimg.png?raw=true" width="50%" height="40%" title="px(픽셀) 크기 설정" alt="RubberDuck"></img>   
> 현재 이미지에서는 서비스 계정이 공란이지만 계정 생성 후 본인 계정이 나오는 게 맞다.   
<br/>  
   
- 상단 탭의 키 버튼을 누른다.   
- 키 생성을 누른다.   
- json으로 키 생성을 누른다.   
   <br/>  
<img src="https://github.com/joohyoungkim19940805/imgRepository/blob/bbd2aa02458aa736916fc2e61d317e3ab18cc4ff/%EC%BA%A1%EC%B2%98222.JPG?raw=true" width="50%" height="40%" title="px(픽셀) 크기 설정" alt="RubberDuck"></img>   
<br/>  
   
```json
{
  "type": "{타입}",
  "project_id": "{내 프로젝트 아이디}",
  "private_key_id": "{키 아이디}",
  "private_key": "{키값}",
  "client_email": "{비밀}@{비밀}.iam.gserviceaccount.com",
  "client_id": "{비밀}",
  "auth_uri": "{비밀}",
  "token_uri": "{비밀}",
  "auth_provider_x509_cert_url": "{비밀}",
  "client_x509_cert_url": "{비밀}"
}
```
이런식으로 되어 있다.   

- 구글 시트로 이동한다.   
- 데이터를 적당히 입력한다.   
   <br/>  
<img src="https://github.com/joohyoungkim19940805/imgRepository/blob/7fc282534e6ced75f585107694bba1422c36a6dd/%EC%BA%A1%EC%B2%98.JPG?raw=true" width="50%" height="40%" title="px(픽셀) 크기 설정" alt="RubberDuck"></img>   
   

###pip install   
```python
pip install gspread
pip install --upgrade oauth2client
pip install --upgrade google-

or

pip install --upgrade google-api-python-client oauth2client
```

- 터미널로 가서 필요한 라이브러리를 다운받는다. 귀찮으면 다 받아도 무방하다.   

```python

import gspread
from oauth2client.service_account import ServiceAccountCredentials


scope = [ #구글 시트 auth인증
'https://spreadsheets.google.com/feeds',
'https://www.googleapis.com/auth/drive',
]
json_file_name = 'y1994-278010-5ba607106a0f.json' # 제이슨 파일 불러오기
credentials = ServiceAccountCredentials.from_json_keyfile_name(json_file_name, scope) # 구글 api에 제이슨 파일 넣어주기
gc = gspread.authorize(credentials) # api 인증이 완료되면 클라이언트 생성
spreadsheet_url = 'https://docs.google.com/spreadsheets/d/1lDSci8Kh44ucDGT3EZjJwNcPIfeM2UCTuGY1Mv-RxeU/edit#gid=1474400587' #자신의 구글 시트 URL 입력 
                                                                                                                            #자신의 구글 시트에 공유하기로 
                                                                                                                            #구글 드라이브 api 생성 시 만들어진 이메일하고 연동되어 있어야함
doc = gc.open_by_url(spreadsheet_url) #시트 정보가 들어간다.
worksheet = doc.worksheet('정기보고서_제출기한') #워크 시트 정보가 들어간다.
range_list = worksheet.range('A1:B1') # 해당 워크 시트 안에서 A1셀과 B1셀을 찾는다.
print(range_list)

#---------------------------------------------
#출력 : 
#[<Cell R1C1 'A'>, <Cell R1C2 '1'>]

```
   
이제 파이썬에서 구글 시트에 있는 값을 get 하거나 set할 수 있다.   
다른 방법을 이용하면 구글 시트에서 파이썬 서버에 쿼리스트링으로 값을 요청해서 restapi형식으로 구글 시트가 파이썬에 값을 요청할 수도 있다.   

   
### 장점   
   
- 데이터 양이 적으면 미니 DB 대용으로 쓰기 편리하다.    
> ex) 프로퍼티값을 디비에 저장할 필요 없이 구글 시트에 저장하면 이용이 간편하기 때문에 DB에 애매한 테이블을 만들 필요가 없다.   
> ex) 현업에서도 테이블로 만들기는 부담스럽지만 만들 필요가 있는 테이블은 csv 형태로 저장해서 서버 로컬에 넣는 경우가 있는데, 서버 로컬에 저장하는 것보다 리소스 적으로 가볍다고 생각한다.    
> ex) 데이터를 분산할 수 있어서 구글 시트에 중요한 값들을 백업해놓고 에러 발생시 해당 값을 참조하도록 하면,    
> > 어떤 서버 장애로 반드시 매핑되어야 할 프로퍼티값이 매핑되지 않을 때 대응책으로 좋지 않을까? 싶어서 서비스적으로 편리하고 보안에도 좋다는 생각이 들었다.     
- 데이터를 즉석에서 가공할 수 있다.    
> ex) 쿼리문으로는 DB에 있는 내용을 가공하는 데에 한계가 있기 때문에 DB의 내용을 가져와서 백단에서 핸들링하는 경우가 있다.   
> 구글 시트에는 엑셀 함수가 내장 되어 있고, 개발 언어로는 자바스크립트 언어가 내장되어 있기 때문에 DB의 내용을 가공할 때 리소스를 분산할 수 있다.   
      
### 단점   
   
- 무겁다. 엑셀보다 무겁다. 때문에 DB 대용으로 쓰기에 한계가 명확하게 크다. 무겁다는 단점이 매우 크다.   
- 데이터 양이 많을 수록 구글 시트를 많이 생성해야 한다.    

   

