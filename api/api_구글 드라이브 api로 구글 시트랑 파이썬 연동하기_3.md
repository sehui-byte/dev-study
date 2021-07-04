# 코드 다듬기
-----------------------------------------------

### xml parser 코드 추가
```python
    #xml parser 코드 추가
    def get_root(self, xml_name): # urp의 https://값을 이용하여 파일 or 도메인 분기처리
        if self.find_http(xml_name):
            root = self.create_element_tree(xml_name)
        else:
            root = self.create_element_tree_url(xml_name)
        return root

    #html 찾기
    def find_http(self, xml_name):#없으면 True 있으면 False
        return re.match('https://|http://',str(xml_name)) == None

    #xml을 dataFrame으로 변환할 때 xml에서 최하위 루트값이 없을 경우
    #최하위 루트값을 그대로 반환하였는데
    #이를 처리하는 로직이 없었기에 추가함.
    def get_xml_dataFrame(self, root, option=False):
        prev = []
        lowest = []
        mapData = self.find_lowest_tag(root)
        if type(mapData) == dict:
            prev = mapData['prev']
            lowest = mapData['lowest']
        else :
            prev.append(mapData)
        df = self.create_stock_dataFrame(prev, lowest, option)
        return df
```

### 구글 시트 핸들러 코드 추가

```python
    def save_dataFrame_to_sheet(self, sheet_name, df=None, xml_name=None, request_log=True):
        # 데이터프레임이 없을 경우 데이터 프레임을 생성
        # 디폴트 벨류를 이용하여 오버로딩과 비슷한 형식 구현
        # 파이썬은 오버로딩이 없다.
        if df.empty == True and xml_name != None and xml_name != '':
            df = self.get_dataFrame(xml_name)
        #데이터프레임이 있을 경우 데이터프레임을 시트에 저장
        test = set_with_dataframe(self.sheets_map[sheet_name], df, row=1, col=1, include_index=False, include_column_header=True,
                               resize=False, allow_formulas=True)

        if request_log :
            return self.check_sheet_is_dataFrame(sheet_name, df)
        else:
            print("")
```

# 파이썬 리플렉션 이용하여 함수 구현하기

### 구글 시트 핸들러 코드 추가

- 파이썬은 case 문이 없다.
- 리플렉션을 이용하여 case 문과 유사하게 동작시킬 수 있다.
- 자바에도 리플렉션이 있으며
- 리플렉션은 다양하게 이용할 수 있다.
- 리플렉션 = 동적인함수

```python
    #리플렉션 이용하여 분기처리시키기
    #구글 시트로부터 col, row, all로 원하는 데이터를 가져올 수 있게 한다.
    def get_find_matrix(self, sheet_name, find_taget, matrix = 'all', option = 'list'):
        self.matrix_taget = "get_find_"+matrix # 함수 이름 정의하기
        self.method = getattr(self, self.matrix_taget, lambda : "default") # 문자열로 된 함수를
                                                                           # 함수로서 기능케 하기 위해 셋팅
                                                                           # lambda : "default"는 어떤 케이스도 아닐 경우 실행 시킬 함수를 정의
                                                                           # lambda : "default" == get_find_matrix_dafault() 함수 실행
        return self.method(sheet_name, find_taget, option = 'list')
        
    #col 가져오기
    #@param sheet_name : 시트의 이름
    #@param find_taget : 찾고자하는 데이터로서 int={찾고자하는 index}, str={찾고자하는 문자열}값만 가져오게끔 제한시킨다.
                         # 어차피 데이터는 index 기준으로 찾는지, str 기준으로 찾는지 둘 중 하나밖에 없다.
                         # 시트의 모든 데이터는 str 기준 (예외 : 날짜 >>> 추후 적용 예정) 
    #@param option : 어떤 형태로 반환받을지 선택, list / index
    def get_find_col(self, sheet_name, find_taget, option = 'list'):
        if type(find_taget) != int and type(find_taget) != str:
            return print("valueError") # type 안 맞으면 튕구기
        if type(find_taget) == str : #찾고자 하는 값의 기준이 str : 일 경우 해당 문자열의 인덱스값 가져오기
            index = self.sheets_map[sheet_name].find(find_taget).col
        else : # int : 찾고자하는 값의 기준이 int : 일 경우 해당 인덱스 기준으로 값 가져오기
            index = find_taget
        if type(find_taget) == int and find_taget == 0: # 구글 시트의 인덱스 초기값은 0이 아닌 1이다.
                                                        # 인덱스 값으로 0을 줄 경우 error
            return print("0은 안되요")
        else:
            data = self.sheets_map[sheet_name].col_values(index) # 해당 문자열의 인덱스값 찾아서 list 반환
            
        try:
            if option == 'list': # list 반환을 원할 경우 list 반환
                return data
            if option == 'index': #찾고자 하는 값의 index값을 원할 경우 index 반환
                return (data.index(find_taget) + 1)
        except:
            return -1 # 찾고자 하는 값이 없을 경우 -1 반환
            
    # row 가져오기
    def get_find_row(self, sheet_name, find_taget, option='list'):
        if type(find_taget) != int or type(find_taget) != str:
            return print("valueError") # type 안 맞으면 튕구기
        if type(find_taget) == str: #str 일 경우 해당 문자열의 인덱스값 가져오기
            index = self.sheets_map[sheet_name].find(find_taget).row
        else:
            index = find_taget
        if type(find_taget) == int and find_taget == 0:
            return print("0은 안되요")
        else:
            data = self.sheets_map[sheet_name].row_values(index)
        try:
            if option == 'list':
                return data
            if option == 'index':
                return (data.index(find_taget) + 1)
        except:
            return -1
    #어떠한 케이스도 아닐 경우 실행 시킬 함수 셋팅
    def get_find_default(self, sheet_name, find_taget, option='list'):
        return self.sheets_map[sheet_name].get_all_records()
    #모든 데이터를 가져오는 함수 get_find_all = default와 임시로 동일하게 만든다.
    def get_find_all(self, sheet_name, find_taget, option='list'):
        return self.sheets_map[sheet_name].get_all_records()
        
    
    

        
    def save_restAPI_join_sheet(self,sheet_name, url, join_sheet_name, join_key): # 데이터를 조인하여 가져오는 함수
                                                                               # key가 될 컬럼값을 추출하여
                                                                               # 해당 key와 조인 되는 rest API를 호출하여 데이터를 가져온다.
        key_list = self.get_find_matrix(join_sheet_name, join_key, matrix='col', option='list')[1:] #컬럼이름 제외
                                                                                                    #key가 되는 컬럼 list 전부 추출
        i=0
        data_list = []
        for replace_value in key_list: #키값 리스트를 iter 대입
            taget_url = self.replace_api_key_value(url, join_key, replace_value) #restAPI의 URL의 쿼리스트링 값 중 자신이 원하는 키값의 value값을 치환하는 함수 실행
            root = self.parser.get_root(taget_url) # xml_parser로 root 가져오기
            data = self.parser.get_xml_dataFrame(root, self.parser.find_http(taget_url)) # http|https 검사
            data_list.append(data) # 데이터 추가
            i = i + 1
            print(i,'/',len(key_list)) #몇번째인지 체크

        return {'col_key' : self.parser.column_name_list, 'data' : data_list, 'key_list' : key_list, 'join_key' : join_key}
        # 조인하여 얻어낸 값들의 정보를 dict형식으로 저장하여 리턴한다.
   
    #위 함수에서 조인하여 얻어낸 값들의 정보를 구글 시트로 저장한다.
    def join_to_save_sheet(self, map_info):
        import type_of_selection as tos # 데이트 변환을 위해 내가 만든 클래스 불러오기
        
        col = map_info['col_key']  # 컬럼 키값 리스트
        taget_index = col.index(map_info['join_key'])  # 조인 할 키의 인덱스값
        col = col[taget_index:len(col)]  # 컬럼 키 가공 (상태값 제거 : 제거 안 할 시 상태값 = 000 성공여부 = 성공 값이 섞여들어감)
        data = [x[taget_index:len(x)] for x in map_info['data']] # 데이터 가공하기 (상태값 제거 : 제거 안 할 시 상태값 = 000 성공여부 = 성공 값이 섞여들어감)
        data = [tos.transform().transform_type(x, "list", "str") for x in data] # xml_parser에서 str을 list로 바꿔서 넘겨줬기 때문에 
                                                                                # list의 요소 값들의 type를 list -> str로 변환해줘야한다.
                                                                                # type_of_selection 패키지의 fransform 클래스를 이용한다.
        taget = pd.DataFrame(columns=col) # 컬럼값만 존재하는 데이터프레임 생성
        
        for i in range(0, len(data), 1):#키 값에 해당하는 list를 데이터 프레임에 집어넣는다.
            taget.loc[i] = data[i]
            
        taget = tos.save().save_type(type='df', taget=taget, data=data, row=map_info['key_list'], size=len(data)) # 데이터프레임 저장을 위해
                                                                                                                  # type_of_selection 패키지의 save 클래스를 이용한다.
        result = self.save_dataFrame_to_sheet(sheet_name='기업개황정보', df=taget) # 데이터프레임을 구글 시트로 저장시켜주는 함수를 실행한다.
        
        return result # 저장 결과 리턴
        
        
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
# 모든 분기처리를 해결할 클래스 만들기
- 실제 돈으로 주식 자동매매를 할 것이기 때문에 type error가 발생해선 안된다.
- 이와 별개로 함수가 많아지면서 함수 자체를 고치든, if문으로 분기처리를 시키든, 보강해야 할 구간이 많아졌다.
- if문으로 떡칠을 할 시 눈과 정신이 아파올 테니 type 핸들링과 type, 매개변수 등을 이용한 분기처리를 한 번에 진행할 수 있도록 
- 리플렉션을 응용하여 클래스를 만든다. 
- save :    
>>> 데이터를 다른 객체로 저장하는 클래스   
   
- transform:    
>>> 동일한 데이터를 다른 type으로 치환해주는 클래스   
   
- get :    
>>> 데이터 중 일부분을 꺼내오는 클래스   
   
- find :    
>>> 데이터의 type를 찾아오는 클래스   
   
- 현재 미완성   
   
```python
# file name : type_of_selection

# 모든 데이터 저장을 처리할 save 클래스
# 파이썬에는 **사용자가 정의할 수 있는** 오버로딩이 없다.
# 리플렉션과 매개변수의 디폴트 밸류를 이용하여 오버로딩 클래스를 구현한다.

import re # 정규식 관련
import pandas as pd # 판다스

class save:
    # 현재는 데이터프레임을 기준으로 작성한다. 
    # 추후 리스트, 딕셔너리, 제이슨 , xml 등도 추가한ㄷ,
    # 오버로딩을 모델로 구현하였기 때문에
    # 디폴트밸류가 None이며, 때문에 매개변수의 이름에 휘둘릴 필요 없이
    # 마음대로 타입을 정의하고 이를 처리하는 함수로 작성하면 된다.
    def save_type(self, type, data=None, taget=None, col=None, row=None, size=None):
        self.save_type_name = 'save_' + str(type) #함수 이름 설정
        self.method = getattr(self, self.save_type_name, lambda : "default") # method 셋팅
        return self.method(data, taget, col, row, size) #동적 함수 실행
        
    #데이터프레임으로 저장하는 동적 함수
    def save_df(self, data, taget, col, row, size): #row에 키 data= data
        if find().find_type(taget) != "DataFrame" : # 데이터프레임이 지정되지 않았으면...
                                                    # 해당 값은 디폴트가 None인데 데이터프레임은 None를 사용할 시 에러가 나기 때문에 (None가 없기에..)
                                                    # None인지 DataFram인지 둘 다를 비교하기 위해서는 type으로 비교해야 한다.
                                                    # 에러 방지를 위해 문자열로 반환받은 타입으로 비교한다.
            taget = pd.DataFrame(columns=None) # 데이터 프레임을 생성
        if row != None: # 행을 기준으로 데이터프레임에 값 추가
            for i in range(0, size, 1):
                taget.loc[i] = data[i]
        if col != None: # 열을 기준으로 데이터프레임에 값 추가
            for i in range(0, size, 1):
                taget.loc[:, col[i]] = data[i]
        return taget
    def save_default(self, data, taget, col, row, size): # type가 정해지지 않았을 경우 
                                                         # data값을 그대로 던져 준다. (data = 다차원 list 값)
                                                         # xml parser에서 무조건 데이터프레임을 리턴하게끔 만들었는데
                                                         # dataFram형식의 다차원 list를 반환받아야 하는 로직이 필요했다.
                                                         # 이럴 때 마다 매번 if문을 쓰기 보단
                                                         # 분기처리 클래스를 이용하여 재사용이 가능토록 만든다.
        return data
        
        
        
```

```python
class transform :
    #동적함수를 2단으로 구현한다.
    #taget_type에 따라 분기할 동적함수가 달라지며
    #함수 + taget_type : 에서 want_type에 따라 다시 분기할 동적함수가 달라진다. 
    def transform_type(self, data, taget_type, want_type):
        self.taget_type_name = 'transform_' + taget_type #함수 이름 설정
        self.method = getattr(self, self.taget_type_name, lambda : "default") #method 셋팅
        return self.method(data, taget_type, want_type) #동적 함수 실행

    def transform_default(self, data, taget_type, want_type):#default일 경우 데이터를 그대로 반환한다.
        return data 

    def transform_list(self, data, taget_type, want_type):
        self.taget_want_type = 'fransform_list_' + want_type
        self.method = getattr(self, self.taget_want_type, lambda : "default")
        return self.method(data, taget_type, want_type)

    def fransform_list_str(self, data, taget_type, want_type): #리스트 안의 요소들의 타입을 변경한다.
        return ["".join(x) for x in data] # [['김'],['주'], ['형']]을 ['김','주','형']으로 반환한다.
        
    def fransform_list_default(self, data, taget_type, want_type): #default일 경우 데이터를 그대로 반환한다.
        return data
    #추후 다른 변수들도 치환하는 함수를 작성할 예정
    


class get:#미완성<<<

    def get_type(self, type):
        return ''
```

```python
class find: #미완성
            # type에는 기본 변수의 타입인 str, int, bool, float등이 있다.
            # 클래스 변수 타입인 DataFrame, dict, list, sheet 등이 있다.
            # 클래스로 정의되지 않는 타입인 xml, json은 문자열로 찾을 수 있다.(클래스로 못찾나?)
            # 모든 타입은 문자열로 변환하여 리턴한다.
    """
    미완성함수
    def find_type_cast(self, taget):
        type_str = self.find_type(taget) # 해당 데이터가 어떤 타입인지 찾아주는 함수 실행
        if type_str == 'type' :
            return taget 
        if type_str == 'default':
            type_str = str(type(taget))
        self.find_type_name = 'find_'+ type_str
        self.method = getattr(self, self.find_type_name, lambda : "default")
        return self.method(type_str, taget)
    """

    def find_type(self, taget): # 타입 셋팅 함수
        pattern =r"sheet|list|DataFrame|dict|xml|json|int|float|NoneType|tuple|dict|bool|type" #정규식 패턴 생성
                                                                                               #str은 무시한다.
        #pattern =r"sheet|DataFrame||xml|json|list|dict"
        re_com = re.compile(pattern) #정규식 패턴을 컴파일하여 재사용 여지를 남긴다.
        taget_type = self.return_type(taget, re_com) # 해당 데이터가 어떤 타입인지 찾는 함수 실행
        return taget_type

    def return_type(self, taget, re_com): #해당 데이터가 어떤 타입인지 찾는 함수
        try:
            sharch_taget = re_com.search(str(type(taget))).group() # 해당 데이터가 타입을 찾아내고 이를 문자열로 변환한다.
                                                                   # 해당하는 타입을 문자열로 리턴받을 수 있도록 한다.
        except:
            try:
                sharch_taget = re_com.search(str(taget)).group() # 해당 데이터를 타입으로 바꾸지 않고 그대로 문자열로 변환한다.
                                                                 # 해당 데이터 안에 xml, json이라는 문자열이 있을 경우
                                                                 # "xml", "json" 리턴
            except:
                sharch_taget = "default" #어떠한 값도 아닐 경우 default 리턴
        return sharch_taget

    def find_sheet(self, type_str, taget):
        return ""
    def find_DataFrame(self, type_str, taget):
        return ""
```
