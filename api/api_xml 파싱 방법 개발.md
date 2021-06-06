XML / DataFream 작업에 다형성을 추가하여 작업 효율 높이기
============================

대상이 되는 XML 파일
----------------
- 한국금융감독원에 등록된 기업체의 고유번호, 업체이름, 주식상장코드, 업데이트날짜로 이루어져있다.   
- 전자공시 opendart restapi를 이용하기 위해서는 기업체의 고유번호를 key값으로 이용해야 하기 때문에 해당 xml 데이터가 필요하다.   
- 단 작성자는 주식시장에 상장된 기업체만을 대상으로 할 예정이기 때문에 <stock_code>가 빈값인 기업체의 데이터는 필요 없다.   
> (ex : 다코)
```xml
<result>
    <list>
        <corp_code>00434003</corp_code>
        <corp_name>다코</corp_name>
        <stock_code> </stock_code>
        <modify_date>20170630</modify_date>
    </list>
    <list>
        <corp_code>00434456</corp_code>
        <corp_name>일산약품</corp_name>
        <stock_code> </stock_code>
        <modify_date>20170630</modify_date>
    </list>
    <list>
        <corp_code>00126380</corp_code>
        <corp_name>삼성전자</corp_name>
        <stock_code>005930</stock_code>
        <modify_date>20201209</modify_date>
    </list>
</result>
```

기존 코드의 문제점
---------------------------
- 파일 경로를 반드시 알아야 파싱할 수 있었다.
> ex : (C:xml\\XMLFILE.xml)
- 노드의 tag 이름을 반드시 알아야 파싱할 수 있었다.
> ex : (<list> <corp_code>, <corp_name>, <stock_code>, <modify_date> </list>)
- 데이터 프레임에서 {'컬럼 이름' : 데이터}를 지정해줄 때 '컬럼 이름'을 무조건 하드코딩으로 해야 해서 손가락이 아팠다.
```python
if __name__ == '__main__':
    tree = ET.parse('xmlDir/CORPCODE.xml')
    root = tree.getroot()
    dataList = [[],[],[],[]]
    for content in root.iter('list'):
        if content.findtext('stock_code') != ' ' and len(content.findtext('stock_code')) > 1:
            dataList[0].append(content.findtext('corp_code')) # 또는 content[i].text로 꺼내올 수 있다.
            dataList[1].append(content.findtext('corp_name'))
            dataList[2].append(content.findtext('stock_code'))
            dataList[3].append(content.findtext('modify_date'))

    df = pd.DataFrame(
        {
        'corp_code' : dataList[0],
        'corp_name' : dataList[1],
        'stock_code' : dataList[2],
        'modify_date' : dataList[3]
        })
    print_df(df)
```
   
해결방안
----------------------------------
- 1. 파일 이름만으로 파일경로까지 찾아서 해당 파일을 매핑할 수 있어야 한다.   
- 2. 노드의 이름을 알지 못해도 xml 데이터를 파싱할 수 있어야 한다.    
- 3. 데이터 프레임을 만들 때 별도의 하드코딩을 안 해도 된다.   
 
    
0.import info
--------------------------------------
    
```python
import xml.etree.ElementTree as et # XML 파싱을 위한 패키지
import os as os                    # 파일 이름으로 파일 경로 찾기를 위한 os 패키지
import pandas as pd                # 판다스
import time as time                # 함수 실행 속도 비교를 위한 time 패키지
from tabulate import tabulate      # 데이터프레임을 예쁘게 출력할 때 쓸 패키지
#from multiprocessing import Pool  # 멀티프로세스로 특정 함수를 자바 쓰레드처럼 돌릴 때 쓰는 패키지 쓰레드와는 개념이 다르다. 쓰려다가 말았다.
#import sys, os                    # 에러로그 해결책 중 하나, 하지만 안 쓴다.
    
def print_df(data):                #데이터 프레임을 예쁘게 출력해주는 함수
print(tabulate(data, headers='keys', tablefmt='psql'))


class create: #사용하게 될 클래스
    def __init__(self):
        self.dir_root=''
```
- 함수 목록
> create_element_tree()    : 최상위에 있는 노드를 가져오는 함수

> find_file()              : 파일 이름으로 해당 파일이 있는 경로까지 매핑해줄 함수

> find_lowest_tag()        : 최상위 노드에서 최하위 노드까지 내려가면서 최하위 노드와 그 부모 노드를 같이 리턴해주는 함수 

> check_validation()       : 최하위 노드와 그 부모 노드를 매개변수로 받으면 노드의 갯수가 일치하는지 검증하고, 최하위 노드의 갯수만큼 차원 배열을 가지는 빈 리스트와 최하위 노드들의 이름을 리스트로 set해주고, 최하위 노드의 갯수를 리턴해주는 함수 / 추후에 기능을 분리할 예정 /

> create_stock_dataFrame() : 최하위 노드와 그 부모 노드를 매개변수로 받으면 최하위 노드의 갯수만큼 차원을 가지는 빈 리스트에 데이터를 집어넣고, 노드에 알맞는 데이터 프레임을 생성시켜주는 함수 / 추후에 기능을 분리할 예정 /
    
> create_list() : 숫자를 매개변수로 받으면 그 숫자만큼 차원을 가지는 빈 리스트를 생성하는 함수
    
> getColumn_name_list() : 컬럼 이름을 리스트로 리턴해주는 함수, check_validation()을 먼저 호출해야 사용 가능한 함수
    
> getStock_list() : xml 데이터를 리스트 형태로 리턴해주는 함수
    

## 1.파일 이름만으로 파일경로까지 찾아서 해당 파일을 매핑하기
-----------------------------------------------------------------------------
    
```python
    def find_file(self, file_name):
        result = False
        for path, dirs, files in os.walk(str(os.getcwd()) + '\\'): # os.getcwd으로 현재 디렉토리 위치를 설정 / os.walk로 현재 디렉토리에서부터 모든 파일을 검색한다.
            for file in files: # os.walk로 하위 경로에 있는 모든 파일 중 file_name과 일치하는 파일을 찾는다.
                if file == file_name:
                    result=True #file_name과 일치할 경우
                    self.dir_root=path+'\\'+file #해당 파일의 경로를 저장한다.
                    break
            if result : # 일치하는 파일을 찾았으면 탐색 중지
                break
        return result
```

## 2.노드의 이름을 알지 못해도 xml 데이터를 파싱할 수 있어야 한다.
-------------------------------------------------------------------------------
    
```python
    def create_element_tree(self, file_name):
        if self.find_file(file_name): # 해당 파일이 진짜루다가 있는지 확인
            tree = et.parse(self.dir_root) #XML tree 생성
            root = tree.getroot() # 최상위 노드 정보 넣기
        else:
            root = '에러 로그를 어떻게 처리할지?'
        return root
```
    
```python
    def find_lowest_tag(self, root):
        first = list(root) # 최상위 노드의 바로 하위 노드 정보 가져오기
                           # root.getChildren()으로도 가져올 수 있으나, 추후 삭제될 예정인 함수로 list(root) 사용 권장
                           # 해당 변수는 계속 하위 노드 정보를 내려받게 된다.
        prev = first # 최하위 노드의 부모 노드가 들어가게 될 변수
        lowest = [] # 최하위 노드가 들어가게 될 변수
        while not first == False: # 하위 노드가 있으면...
            prev = lowest # 현재 노드의 부모 노드를 집어넣는다.
            lowest = first # 현재 노드를 집어넣는다.
            first = list(first[0]) #현재 노드의 하위 노드를 집어넣는다.
            if not first : # 하위 노드가 없으면 break
                break
        return {'prev' : prev ,'lowest' : lowest}
```
    
```python
    def check_validation(self, prev, lowest):
        check_test=False
        for i in range(0,len(prev),1): # 부모 노드로 거슬러 올라가면서 최하위 노드의 갯수가 전부 일치하는지 확인
            if len(lowest) != len(prev[i]):
                check_test=False
                break
            else:
                check_test=True
        if check_test :
            itemCount = len(lowest)
    
            # 최하위 노드의 데이터를 가지는 <list></list> element 정보를 집어넣는다. 
            # 이제 '<list>'라는 태그 이름을 몰라도 element_list[i][j] 또는 element_list[i].findtext['태그이름'] 형태 등 다양한 방법으로 데이터를 꺼내올 수 있다.
            self.element_list = prev 
    
            self.stock_list = self.create_list(itemCount) # 데이터프레임에 집어넣을 itemCount갯수만큼의 차원을 가진 빈 리스트 생성
            self.column_name_list = self.create_list(itemCount) 
            for i in range(0,len(self.column_name_list),1):
                self.column_name_list[i]=lowest[i].tag # 태그 이름을 몰라도 데이터를 꺼내올 수 있도록 최하위 태그들의 이름을 list형태로 저장한다.
        return itemCount
```
    
```python
    def create_list(self, itemCount):
        item = []
        stock_list = []

        for i in range(0,itemCount,1): # 매개변수 itemCount 갯수만큼의 차원을 가지는 리스트를 생성한다.
            stock_list.append(item)
        return stock_list
```
    
```python
    def getColumn_name_list(self): # check_validation() 함수에서 생성된 최하위 태그 이름들의 리스트
        return self.column_name_list
```
    
    
## 3.데이터 프레임을 만들 때 별도의 하드코딩을 안 해도 된다.
-------------------------------------------------------------
    
```python
    def create_stock_dataFrame(self, prev, lowest):
        break_point = False
        itemCount = self.check_validation(prev, lowest)
        dataFrame = pd.DataFrame(columns=self.column_name_list) # 최하위 태그이름 리스트를 컬럼값으로 지정하여 빈 데이터 프레임을 생성한다. 
        data=[[],[],[],[]]
        for row in self.element_list: # <list>노드로부터 실제 데이터를 가지고 있는 하위 노드들 정보를 row에 집어넣기
            for item in row: # row에 들어간 최하위 노드들의 리스트를 item에 iter 형식으로 집어넣기
                if len(item.text) < 2 and item.text == ' ': # 만약 해당 item에 데이터가 없으면 건너뛰기 해당 행을 완전히 건너뛰기
                                                            # 빈 데이터가 있을 경우 완전히 한 행을 제외해버린다.
                    break_point = True
                    break
                else:
                    break_point=False
            if not break_point:
                for i in range(0, itemCount, 1): # 사실상 동일한 for문이 두 번 돌게 된다. <<<문제점
                    data[i].append(row[i].text)
        self.stock_list=data
        for i in range(0,itemCount,1):
            dataFrame.loc[:,self.column_name_list[i]] = self.stock_list[i] # 컬럼이름, 태그이름을 몰라도 데이터프레임을 생성할 수 있게 됐다.
        return dataFrame
```

속도 비교하기
------------------------------------------------------------
    
### 해방안 적용 전
```python
| 3322 |    00365387 | AJ네트웍스                                        |       095570 |      20210513 |
| 3323 |    00302078 | 한국코퍼레이션                                    |       050540 |      20210513 |
| 3324 |    00120216 | KB손해보험                                        |       002550 |      20210513 |
| 3325 |    00753643 | 컨버즈                                            |       109070 |      20210513 |
| 3326 |    00304401 | 텔라움                                            |       047730 |      20210506 |
| 3327 |    01437186 | ESR켄달스퀘어리츠                                 |       365550 |      20210602 |
| 3328 |    00521390 | 뉴파워프라즈마                                    |       144960 |      20210602 |
| 3329 |    00657002 | 에이디테크놀로지                                  |       200710 |      20210602 |
+------+-------------+---------------------------------------------------+--------------+---------------+
WorkingTime: 0.915524959564209 sec

Process finished with exit code 0
```
    
    
### 해결방안 적용 후
```python
| 3322 |    00365387 | AJ네트웍스                                        |       095570 |      20210513 |
| 3323 |    00302078 | 한국코퍼레이션                                    |       050540 |      20210513 |
| 3324 |    00120216 | KB손해보험                                        |       002550 |      20210513 |
| 3325 |    00753643 | 컨버즈                                            |       109070 |      20210513 |
| 3326 |    00304401 | 텔라움                                            |       047730 |      20210506 |
| 3327 |    01437186 | ESR켄달스퀘어리츠                                 |       365550 |      20210602 |
| 3328 |    00521390 | 뉴파워프라즈마                                    |       144960 |      20210602 |
| 3329 |    00657002 | 에이디테크놀로지                                  |       200710 |      20210602 |
+------+-------------+---------------------------------------------------+--------------+---------------+
WorkingTime: 0.975637674331665 sec

Process finished with exit code 0
```
- 반복 실행 결과 해결방안 적용 후 기존 방법과 속도 차이가 많이 나지 않는다.
```python
------------------------------------------------------------
### 해경방안 적용 전: 0.6143901348114014 sec
### 해결방안 적용 후: 0.7070801258087158 sec
------------------------------------------------------------
------------------------------------------------------------
### 해경방안 적용 전: 0.797865629196167 sec
### 해결방안 적용 후: 0.8986263275146484 sec
------------------------------------------------------------
------------------------------------------------------------
### 해경방안 적용 전: 0.6781601905822754 sec
### 해결방안 적용 후: 0.6861639022827148 sec
------------------------------------------------------------
------------------------------------------------------------
### 해경방안 적용 전: 0.7978692054748535 sec
### 해결방안 적용 후: 0.894636869430542 sec
------------------------------------------------------------
------------------------------------------------------------
### 해경방안 적용 전: 0.8856019973754883 sec
### 해결방안 적용 후: 0.7280890941619873 sec
------------------------------------------------------------
------------------------------------------------------------
```

두 데이터가 일치하는지 확인하기
--------------------------------
```python
    df = pd.concat([my_code_data_frame, df1])
    df = df.reset_index(drop=True)  # 인덱스 초기화

    df_grp = df.groupby(df.columns.tolist())  # 전체 열 비교
    df_di = df_grp.groups  # 딕셔너리로 만들기

    idx = [x[0] for x in df_di.values() if len(x) == 1]  # 인덱스 검토
    print(df.reindex(idx))  # Same as df.reindex(idx) 
```
### 출력 = 
```python
Empty DataFrame
Columns: [corp_code, corp_name, stock_code, modify_date]
Index: []
```

### 만약 두 데이터 프레임의 결과가 다를 경우
```python
    df1 = pd.DataFrame(
        {
        'corp_code' : [1,2,3,4,5],
        'corp_name' : [1,2,3,4,5],
        'stock_code' : [1,2,3,4,5],
        'modify_date' : [1,2,3,4,5]
        })
 
    df = pd.concat([my_code_data_frame, df1])
    df = df.reset_index(drop=True)  # 인덱스 초기화

    df_grp = df.groupby(df.columns.tolist())  # 전체 열 비교
    df_di = df_grp.groups  # 딕셔너리로 만들기

    idx = [x[0] for x in df_di.values() if len(x) == 1]  # 인덱스 검토
    print(df.reindex(idx))  # Same as df.reindex(idx)
```
### 출력 = 
```python
     corp_code  corp_name stock_code modify_date
3330         1          1          1           1
3331         2          2          2           2
3332         3          3          3           3
3333         4          4          4           4
3334         5          5          5           5
...        ...        ...        ...         ...
3105  01535257  엔에이치스팩19호     380440    20210521
3297  01562589      LX홀딩스     383800    20210527
2959  01566248   에코프로에이치엔     383310    20210528
3102  01568413        F&F     383220    20210521
710   99999999   금감원(테스트)     999980    20200129

[3335 rows x 4 columns]
```
- ???
- 710   99999999   금감원(테스트)     999980    20200129 ????    
<img src="https://github.com/joohyoungkim19940805/imgRepository/blob/194d51778f4322e8582dbde8d5424e8a2d64559c/WTF.JPG?raw=true" width="50%" height="40%" title="px(픽셀) 크기 설정" alt="RubberDuck"></img>
- 금감원 수듄;;
    
결론
----------------------------------------------------
xml을 자주 쓸 예정일 것 같아서 만들어본 코드지만 고쳐야 할 부분이나 아직 부족한 부분들이 더러 보여서 아쉽다.      
그래도 xml을 자주 써보면서 개선될 부분들을 찾아내서 고쳐나가면 쓸만한 코드가 되지 않을까 싶다.      
