구글 시트 함수 이용해서 클래스 만들기
------------------------------------------------------

# 구글시트를 SQL DB 대용으로 사용해보기

## 1.함수 연습

```python

        testSheet = doc.worksheet('사업보고서 예제')
        print(testSheet.col_values(1)) # 첫번째 열 가져오기
        
        print(testSheet.col_values(1,'UNFORMATTED_VALUE')) #값을 가져올 때 서식의 결과값을 가져옴 ex) A1=김주형 =find("주",A1) 출력 : 2 출력된 '2'값을 가져온다.
        
        print(testSheet.col_values(1, 'FORMATTED_VALUE')) #값을 가져올 때 텍스트 포맷형식을 따라 가져온다 ex) "1234"=문자열 1234=숫자 = 텍스트 포맷 설정값
        
        print(testSheet.col_values(1,'FORMULA')) #값을 가져올 때 서식 값을 가져온다. ex) "=find("주",A1)" 값을 그대로 가져온다.
        
        print(testSheet.row_values(1)) #첫번째 행 가져오기
        

        print(testSheet.row_values(1).index('reprt_code')) # 해당하는 문자열이 위치한 인덱스를 행 기준으로 가져온다.
        
        print(testSheet.find('sj_div').address) #특정 데이터값의 주소값 ex)A1, B1
        
        print(testSheet.find('sj_div').col) #특정 데이터값의 col 인덱스값 (가로의 몇번째인지)
        
        print(testSheet.find('sj_div').row) #특정 데이터값의 row 인덱스값 (세로에서 몇번째인지)
        
        print(testSheet.cell(3,2)) # 셀 값을 기준으로 가져온다. .cell(row, col
        

        print(testSheet.title) #시트 제목 가져오기
        
        print(testSheet.get_all_records()) #전체 시트 데이터 가져오기
        

        print( type(testSheet.cell(1, 1)) ) #tpye = gspread.models.Cell
        
        print( type(testSheet.range(1, 1)) ) #type = list
        

        print(testSheet.cell(1,2).value) #값만꺼내기
        
        print(testSheet.cell(1,2).col) #열주소
        
        print(testSheet.cell(1,2).row) #행주소
        
        print(testSheet.cell(1,2).address) #값주소 = (ex) A1)
        
```

## 2. 함수 소개

- 1, save_worksheets
> 워크시트들을 넣을 맵 객체 생성 함수      
   

- 2, save_dataFrame_to_sheet
> 데이터프레임을 시트에 넣어줄 함수   
   

- 3, get_dataFrame
> rest api를 dataFrame으로 바꾸는 함수
   
   
- 4, check_sheet_is_dataFrame   
> 시트 안의 데이터가 데이터프레임과 일치하는지 검사하는 함수    
> 구글 시트에 저장한 후 성공 여부를 리턴하지 않아서 저장 성공 여부를 체크할 수 있도록 만들었다.   
   
   
- 5, replace_api_key_value
> rest api의 쿼리스트링 값을 내가 원하는 값으로 치환하는 함수   
   
   
- 미완성 함수   
> get_find_list_col / get_find_list_row = 값을 찾아서 인덱스를 반환해주는 함수   
> get_find_list_col_name / get_find_list_row_name 첫번째 행, 열을 가져오는 함수 위 값을 찾아 인덱스를 반환해주는 함수와 상호작용하게 다시 만들 예정   
> save_list_join_sheet = 특정 시트의 키값을 반환하여 다른 시트와 조인하여 데이터를 저장시키는 함수   


## 3. 클래스 만들기

- 객체를 선언하자마자 구글 시트와 연동하고 모든 시트의 데이터를 전부 맵에 넣게 한다.
- 구글 시트로 주고받는 리퀘스트 리스폰이 느리기 때문에 가급적이면 반복 호출을 자제하고 한번에 핸들링하도록 노력한다.
- 모든 시트 데이터는 시트 이름으로 맵에서 꺼내오도록 한다.
```python
import gspread
from oauth2client.service_account import ServiceAccountCredentials
from gspread_dataframe import set_with_dataframe # 데이터프레임-구글시트 저장해주는 패키지
import pandas as pd
import re
import requests

class google_sheets_handler:
    def __init__(self):
        import testApi_stock_code_xml_parser as parser
        scope = [
        'https://spreadsheets.google.com/feeds',
        'https://www.googleapis.com/auth/drive',
        ]
        self.parser = parser.create() # 이전에 만들었던 xml parser
        self.parser.find_file('y1994-278010-5ba607106a0f.json')
        json_file_name = self.parser.getFile_name()
        credentials = ServiceAccountCredentials.from_json_keyfile_name(json_file_name, scope)
        gc = gspread.authorize(credentials)
        spreadsheet_url = 'https://docs.google.com/spreadsheets/d/1lDSci8Kh44ucDGT3EZjJwNcPIfeM2UCTuGY1Mv-RxeU/edit#gid=1474400587'
        doc = gc.open_by_url(spreadsheet_url)
        self.all_worksheets_list = doc.worksheets() # 모든 워크시트를 list에 집어넣는다.
        self.sheets_map = self.save_worksheets(self.all_worksheets_list) # 필요한 워크시트를 꺼내기 용이하게 맵에 집어넣는다.
```

## 4. save_worksheets   

- 워크시트를 담는 맵 객체 생성 함수   
```python
    def save_worksheets(self, worksheets):
        sheet = {}
        for worksheet in worksheets:
            sheet[worksheet.title] = worksheet
        return sheet
```

## 5. get_dataFrame

- restApi를 데이터프레임으로 바꾼다.
- 원래는 정규식을 이용해서 맛깔나게 만들고 싶었다.
```python
    def get_dataFrame(self, option, data_name):
        if re.match('.xml?', str(data_name)) != None: # 해당하는 문자열이 포함되었는지 확인
            root = self.parser.get_root(data_name) # 이전에 만들었던 xml parser 클래스를 이용한다.
            df = self.parser.get_xml_dataFrame(root, self.parser.find_http(data_name))
        if re.match('.json?', str(data_name)) != None:
            response = requests.get(data_name)
            result = response.json()
            df = pd.read_json(data_name, lines=False)
        return df
```

## 6. save_dataFrame_to_sheet

- from gspread_dataframe import set_with_dataframe 패키지를 이용하여 구글 시트에 데이터 프레임을 저장한다.
- gspread_dataframe의 경우 뜯어보니 기능이 꽤 많아서 성공여부를 리턴하는 게 분명 있을텐데 시간 날 때 봐야할 것 같다.
```python
    def save_dataFrame_to_sheet(self, sheet_name, url, request_log):
        df = self.get_dataFrame(url) #데이터프레임 가져오기
        test = set_with_dataframe(self.sheets_map[sheet_name], df, row=1, col=1, include_index=False, include_column_header=True, 
                               resize=False, allow_formulas=True) #시트에 데이터 프레임 저장하기
        print(test) # 출력 : None / 무조건 None가 출력, 성공여부 리턴 X
        
        if request_log :
            return self.check_sheet_is_dataFrame(sheet_name, df)
```

## 7. check_sheet_is_dataFrame
- 시트에 있는 데이터와 데이터프레임의 데이터를 비교하는 함수
``` python
    def check_sheet_is_dataFrame(self, sheet_name, origin_df):

        get_sheets_data = self.sheets_map[sheet_name].get_all_values() # 시트의 전체 데이터를 가져온다.
        columns_name_list = get_sheets_data[0] # 시트의 전체 데이터 중 컬럼값만 추출하여 컬럼 이름 리스트에 넣는다.
        del get_sheets_data[0] # 시트의 전체 데이터에서 컬럼값만 제거

        sheet_df = pd.DataFrame(get_sheets_data, columns=columns_name_list) #시트 -> 데이터프레임 변환

        compare = pd.concat([origin_df, sheet_df])  # 중복확인 체크 시작
        compare = compare.reset_index(drop=True)  # 인덱스 초기화

        compare_grp = compare.groupby(origin_df.columns.tolist())  # 전체 열 비교
        compare_di = compare_grp.groups  # 딕셔너리로 만들기

        idx = [x[0] for x in compare_di.values() if len(x) == 1]  # 인덱스 검토  # Same as df.reindex(idx)
        compare = compare.reindex(idx)  # 중복체크 확인 종료
                                        # 두 데이터프레임에서 중복되는 값을 감가했을 때
                                        # 하나의 데이터라도 남는다면
                                        # 두 데이터프레임은 완전히 동일한 객체가 아니다.

        if compare.count().sum() == 0:#모든 행렬에서 값 체크, 하나의 데이터라도 있을 시 False
            return True
        else:
            return print_df(compare) # 중복되지 않은 값들을 리턴
```

## 8.replace_api_key_value
- 추후에 rest API url을 핸들링할 목적으로 만든 함수
```python
    def replace_api_key_value(self, url, join_key, replace_value):
        start_str = url.find(join_key) #쿼리스트링의 키값이 몇번째 글자에 있는지 확인
        end_str = url[start_str:len(url)] #키값을 시작으로 키값부터 끝까지 문자열 반환
        replace_data = str(join_key) + '=' + str(replace_value)
        str_data = url.replace(end_str[0:len(replace_data)], replace_data ) #원하는 키의 value를 새로운 값으로 치환
        return str_data
        
        # 출력값 : join_key = 'corp_code' replace_value = {변경할 값}
        """
        https://opendart.fss.or.kr/api/fnlttSinglAcntAll.xml?crtfc_key={비밀}&corp_code=00353735&bsns_year=2020&reprt_code=11013&fs_div=OFS
        https://opendart.fss.or.kr/api/fnlttSinglAcntAll.xml?crtfc_key={비밀}&corp_code=00186540&bsns_year=2020&reprt_code=11013&fs_div=OFS
        https://opendart.fss.or.kr/api/fnlttSinglAcntAll.xml?crtfc_key={비밀}&corp_code=00128759&bsns_year=2020&reprt_code=11013&fs_div=OFS
        https://opendart.fss.or.kr/api/fnlttSinglAcntAll.xml?crtfc_key={비밀}&corp_code=00535588&bsns_year=2020&reprt_code=11013&fs_div=OFS
        https://opendart.fss.or.kr/api/fnlttSinglAcntAll.xml?crtfc_key={비밀}&corp_code=00264981&bsns_year=2020&reprt_code=11013&fs_div=OFS
        Process finished with exit code -1
        쿼리스트링 중 corp_code의 value만 바꾼다.
        """
```
