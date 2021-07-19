# 구글 시트 연동해서 파이썬 배치 프로그램 만들어 보기_1

- 특정 시간에 데이터를 업데이트 하는 배치 만들기

### 구조

- 1.프로퍼티로 등록한 url로 api 호출할 수 있도록 하기
- 2.배치하는 함수 만들기 (미완성)
- 3.플라스크 서버로 rest api 형태의 서비스로 만들기 (추후 )

### 0.기본 import 정보
```python
import testApi_stock_code_xml_parser as xml_parser #내가 만든 xml 파서
import testApi_save_data_to_sheets as save_sheets #내가 만든 구글 시트 핸들러
import type_of_selection as ts #내가 만든 타입 셀렉터

    parser = xml_parser.create()
    sheets_handler = save_sheets.google_sheets_handler()
#그외
import get_url_class as url_class # 배치를 위해 만든 api url 사용 관련 클래스

from urllib.request import urlopen 
from io import BytesIO
from zipfile import ZipFile
```

### 1.프로퍼티로 등록한 url로 api 호출 할 수 있도록 하기

- 프로퍼티 형태
```pytrhon
api_name     property_name	  property_value	                  property_attribute	        call_key	                                        call_vlaue	  
단일재무제표  opendart	      https://opendart.fss.or.kr/api/	  fnlttSinglAcntAll.xml	      crtfc_key,corp_code,bsns_year,reprt_code,fs_div     NoData	      
기업개황정보  opendart	      https://opendart.fss.or.kr/api/	  company.xml                 crtfc_key,corp_code                                 NoData	      
기업고유번호  opendart	      https://opendart.fss.or.kr/api/	  corpCode.xml	              crtfc_key                                           NoData	      
```

- url 저장하고 리턴하는 클래스 만들기
- 추후 api별로 url의 기본값이 달라질 수 있으므로 현재는 동일한 로직이지만 미리 나눠놓는다.
```python
class url:
    def set_key(self, key): #api 키값은 set만 하여 쓴다.
        self.key = key
    def set_sheets(self, sheets): #sheets는 오직 하나의 객체만 유지하도록 한다. (속도를 위해)
        self.sheets_handler = sheets
    def get_sheets(self):
        return self.sheets_handler

    #추후 api별로 url의 기본값이 달라질 수 있으므로 현재는 동일한 로직이지만 미리 나눠놓는다.
    def url_setter(self, param, url, prop_name):
        self.matrix_taget = "url_setter_" + prop_name
        self.method = getattr(self, self.matrix_taget, lambda: "default")
        return self.method(param, url)

    #프로퍼티에 있는 url에 키값을 집어넣는다.
    def url_setter_단일재무제표(self, param, url):
        # *param[0] = crtfc_key
        # *param[1] = corp_code
        # *param[2] = bsns_year
        # *param[3] = reprt_code
        # *param[4] = fs_div
        url = self.sheets_handler.replace_api_key_value(url, param[0], self.key) #url에 자신이 원하는 파라미터 값에 원하는 데이터를 삽입하는 함수 실행
        return url
    def url_setter_기업개황정보(self, param, url):
        # *param[0] = crtfc_key
        # *param[1] = corp_code
        url = self.sheets_handler.replace_api_key_value(url, param[0], self.key)
        return url
    def url_setter_기업고유번호(self, param, url):
        # *param[0] = crtfc_key
        url = self.sheets_handler.replace_api_key_value(url, param[0], self.key)
        return url
    def url_setter_dafault(self, param, url):
        return url
```

- 키값 셋팅하고 url 받아오는 함수들
```python
def get_key(file_name): #키값은 파일로 저장해놓는다.
    key = ""
    if parser.find_file(file_name) :
        key = open(parser.dir_root, 'r') #parser 클래스에서 해당하는 파일을 읽어온다.
    get_url.set_key(key.read()) #키값 저장하기

#api 프로퍼티 url 가져오기
def get_property_url(prop_name):
    return sheets_handler.get_property(prop_name) #시트핸들러 클래스에서 프로퍼티 값 가져오기

def url_handler(key, api_name):
    # 키 셋팅하기
    get_key(key) #키값 셋팅하는 함수 호출
    #url 셋팅하기
    url = get_property_url(api_name) # url 프로퍼티값 가져오는 함수 호출
    url = get_url.url_setter(sheets_handler.prop_attr_key_list, url, sheets_handler.prop_name) # 프로퍼티 값을 url 클래스에 던져주면 url을 연결해서 셋팅한다.
    #결과 = https://opendart.fss.or.kr/api/corpCode.xml?crtfc_key={myKey}
    if api_name == "기업고유번호" :
        #배치 함수호출
        return saveCorpXml(url)
    elif api_name == "기업개황정보" :
        #배치 함수호출
        saveCorpInfo(url)
    elif api_name == "단일재무제표":
        saveCorpReportInfo(url)
```

### 2.배치하는 함수 만들기 (미완성)
```python
def saveCorpXml(url):
    with urlopen(url) as zipresp: #해당 url로부터 약 8만개 기업의 고유번호가 있는 xml 파일을 다운로드 받도록 요청을 보낸다.
        with ZipFile(BytesIO(zipresp.read())) as zfile: # IO로 해당 파일을 읽는다.
            zfile.extractall('DataDir/CorpList') # 해당 경로에 기업 고유번호 xml을 다운로드 받는다.
    if parser.find_file(zfile.namelist()[0]) : # 설치 성공여부를 체크하기 위해 ZipFile 객체에 있는 namelist()를 이용한다.
                                               # namelist 안에는 저장한  파일들의 파일명이 리스트로 저장되어 있다.
                                               # parser 클래스의 find_file 함수로 해당 파일이 존재하는지 체크한다.
        df = parser.get_xml_dataFrame(parser.get_root(zfile.namelist()[0]), True, 'df') # parser 클래스의 함수를 이용하여 해당 xml 파일을 데이터프레임형태로 저장한다.
        return sheets_handler.save_dataFrame_to_sheet("상장기업_목록", df=df, request_log=True) # 저장된 데이터프레임을 시트에 저장하는 함수를 실행한다.
                                                                                               # 시트 저장에 성공할 경우 True를 반환한다.
    else :
        return sheets_handler.save_dataFrame_to_sheet("상장기업_목록",xml_name="CORPCODE.xml", request_log=True) #직접 파일 안에서 꺼내오기



def saveCorpInfo(url): #추후 비동기형태로 속도 향상할 예정
                       #api를 여러번 호출하기 때문에 속도가 느리다.
                       #상장기업의 업체정보를 가지고 있는 api를 호출한다.
    key_map_data = sheets_handler.save_restAPI_join_sheet(url, "상장기업_목록", sheets_handler.prop_attr_key_list[1]) # 키값 리스트와 조인할 테이블 명칭과 조인할 컬럼명칭을 보낼 경우
                                                                                                                     # 키값에 해당하는 restApi url로 변환하여
                                                                                                                     # 데이터를 받아온다. (약 3000개 기업의 업체 정보가 담긴 다차원 배열)
                                                                                                                     # 키값 리스트와 조인한 테이블 명칭, 컬럼명칭, 가져온 데이터를 맵에 넣어 반환하는 함수
    result = sheets_handler.join_to_save_sheet(key_map_data, "기업개황정보") # 조인한 데이터를 "기업개황정보" 테이블에 넣는다.
    return result #성공 여부 반환
    
    
def saveCorpReportInfo(url): #미완성
```
