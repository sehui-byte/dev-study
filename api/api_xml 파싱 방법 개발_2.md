null이 존재하는 api 파싱
================================

```python
|  0 | 20200813000422 |        11012 |        2020 |    01089855 | BS       | 재무상태표     | ifrs-full_CurrentAssets                                                               | 유동자산                                            | -                                 | 제 6 기 반기말 |     80275548813 | 제 5 기말   | 67731073011     |     1 | 139194852967        | 제 5 기 반기  | 74495807995       | 139939545275        |
|  1 | 20200813000422 |        11012 |        2020 |    01089855 | BS       | 재무상태표     | ifrs-full_CashAndCashEquivalents                                                      | 현금및현금성자산                                    | -                                 | 제 6 기 반기말 |      8980886843 | 제 5 기말   | 11315722602     |     2 | 98736343036         | 제 5 기 반기  | 53063429002       | 99768660109         |
|  2 | 20200813000422 |        11012 |        2020 |    01089855 | BS       | 재무상태표     | dart_ShortTermDepositsNotClassifiedAsCashEquivalents                                  | 단기금융상품                                        | -                                 | 제 6 기 반기말 |     48403296268 | 제 5 기말   | 19940690226     |     3 | 40458509931         | 제 5 기 반기  | 21432378993       | 40170885166         |
|  3 | 20200813000422 |        11012 |        2020 |    01089855 | BS       | 재무상태표     | dart_ShortTermTradeReceivable                                                         | 매출채권                                            | -                                 | 제 6 기 반기말 |      4543518029 | 제 5 기말   | 5273485220      |     4 | 28543881075         | 제 5 기 반기  | 14526245489       | 28102897641         |
|  4 | 20200813000422 |        11012 |        2020 |    01089855 | BS       | 재무상태표     | ifrs-full_OtherCurrentFinancialAssets                                                 | 기타금융자산                                        | -                                 | 제 6 기 반기말 |      1034810934 | 제 5 기말   | 785618157       |     5 | 11914628856         | 제 5 기 반기  | 6906133504        | 12067987525         |
|  5 | 20200813000422 |        11012 |        2020 |    01089855 | BS       | 재무상태표     | ifrs-full_Inventories                                                                 | 재고자산                                            | -                                 | 제 6 기 반기말 |     15867510107 | 제 5 기말   | 21855046949     |     6 | 17093022160         | 제 5 기 반기  | 201075310         | 245630579           |
|  6 | 20200813000422 |        11012 |        2020 |    01089855 | BS       | 재무상태표     | dart_OtherCurrentAssets                                                               | 기타자산                                            | -                                 | 제 6 기 반기말 |      1445526632 | 제 5 기말   | 460509857       |     7 | 1287253238          | 제 5 기 반기  | 225824686         | 276130964           |
|  7 | 20200813000422 |        11012 |        2020 |    01089855 | BS       | 재무상태표     | -표준계정코드 미사용-                                                                 | 매각예정자산                                        | -                                 | 제 6 기 반기말 |                 | 제 5 기말   | 8100000000      |     8 | 773478305           | 제 5 기 반기  | 366278973         | 939032059           |
|  8 | 20200813000422 |        11012 |        2020 |    01089855 | BS       | 재무상태표     | ifrs-full_NoncurrentAssets                                                            | 비유동자산                                          | -                                 | 제 6 기 반기말 |     57775714785 | 제 5 기말   | 58178026782     |     9 | 74827226            | 제 5 기 반기  | 74257930          | 966422324           |
|  9 | 20200813000422 |        11012 |        2020 |    01089855 | BS       | 재무상태표     | ifrs-full_OtherNoncurrentFinancialAssets                                              | 기타금융자산                                        | -                                 | 제 6 기 반기말 |      2236462425 | 제 5 기말   | 2892779249      |    10 | 28419048857         | 제 5 기 반기  | 7173405171        | 12010096875         |
| 10 | 20200813000422 |        11012 |        2020 |    01089855 | BS       | 재무상태표     | dart_OtherNonCurrentAssets                                                            | 기타자산                                            | -                                 | 제 6 기 반기말 |        47350000 | 제 5 기말   | 47350000        |    11 | 6770978421          | 제 5 기 반기  | 1852343118        | 3149799417          |
...
...
...
| 95 | 20200813000422 |        11012 |        2020 |    01089855 | SCE      | 자본변동표     | ifrs-full_Equity                                                                      | 기말자본                                            | 별도재무제표 [member]             | 제 6 기 반기   |    104263791605 | -           | -               |    12 | -                   | -             | -                 | -                   |
| 96 | 20200813000422 |        11012 |        2020 |    01089855 | SCE      | 자본변동표     | ifrs-full_Equity                                                                      | 기말자본                                            | 자본 [member]|기타자본항목        | 제 6 기 반기   |      -323588455 | -           | -               |    12 | -                   | -             | -                 | -                   |
| 97 | 20200813000422 |        11012 |        2020 |    01089855 | SCE      | 자본변동표     | ifrs-full_Equity                                                                      | 기말자본                                            | 자본 [member]|이익잉여금 [member] | 제 6 기 반기   |     62722987838 | -           | -               |    12 | -                   | -             | -                 | -                   |
| 98 | 20200813000422 |        11012 |        2020 |    01089855 | SCE      | 자본변동표     | ifrs-full_Equity                                                                      | 기말자본                                            | 자본 [member]|자본금 [member]     | 제 6 기 반기   |     10181753100 | -           | -               |    12 | -                   | -             | -                 | -                   |
| 99 | 20200813000422 |        11012 |        2020 |    01089855 | SCE      | 자본변동표     | ifrs-full_Equity                                                                      | 기말자본                                            | 자본 [member]|자본잉여금 [member] | 제 6 기 반기   |     31682639122 | -           | -               |    12 | -                   | -             | -                 | -                   |
```

기존 코드의 문제점
---------------------
### xml형태로 제공되는 api에서 컬럼 자체가 없는 상태가 있었다. 
> ex) <frmtrm_add_amount/> 태그가 어떤 부모 태그에는 있고 어떤 부모 태그에는 없다.
### 기존 코드는 태그 구성이 모두 동일할 거라고 생각했다.

추가된 함수
---------------------
1. url 형태로 노드 정보 받아오기
```python
   def create_element_tree_url(self, url):
        #파이썬 버젼에 따라 분기한다. 파이썬 버젼에 따라 사용될 패키지가 다르다.
        if sys.version_info[0] == 3: 3.x.x버젼이면..
            from urllib.request import urlopen
        else:#아니면..
            from urllib import urlopen
        response = urlopen(url).read()
        tree = et.fromstring(response)
        return tree
```

2. 태그의 갯수가 불일치하여 추후에 컬럼 갯수가 안 맞는 이슈가 발생할 수 있었으니 이를 처리해야 한다.
```python
    def uncheck_validation(self, prev, lowest):
        itemCount = 0
        index = 0
        tagList = [] #태그 이름을 받아올 리스트 객체
        for i in range(0, len(prev), 1):
            for tagName in list(prev)[i]:
                if not tagName.tag in tagList : # 만약 tagList 안에 해당하는 태그 이름이 없다면
                    tagList.append(tagName.tag) # 태그 이름 리스트에 해당 태그 이름을 추가한다.
        itemCount = len(tagList) # 태그이름 리스트를 max값 기준으로 한다.
        self.element_list = prev
        self.stock_list = self.create_list(itemCount)
        self.column_name_list = tagList # 어차피 태그 이름을 컬럼 이름으로 바꿔야하니 숫자 max값 찾는 로직보다 효율적이라고 생각했다.
        return itemCount
```
수정된 코드
----------------------------

1.가장 최하위 태그를 찾는 함수

### 수정 전
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

### 수정 후
```python
    def find_lowest_tag(self, root):
        first = list(root)
        prev = first
        lowest = []

        # api의 상태값인 해당 태그를 제외시킨다. Element 'status' at 0x0B075938><Element 'message' at 0x0B075960> 
        for i in range(0,len(first),1):
            if not list(list(first[i])) == []:
                first = root.findall(first[i].tag)
                break
        """
        전 = [<Element 'status' at 0x0B8D3A28>, <Element 'message' at 0x0B8D3A50>, <Element 'list' at 0x0B8D3A78>, <Element 'list' at 0x0B8D3D98>...
        후 = <Element 'list' at 0x0B8D3A78>, <Element 'list' at 0x0B8D3D98>, <Element 'list' at 0x0B8E80C8>, <Element 'list' at 0x0B8E83C0>...
        """
        
        # 빈 리스트를 찾을 때, 인터넷에는 if not List : 라고 쓰면 된다고 하는데 이때 if not List == False, True: 로 바꿔서 쓰면
        # 무조건 True로 if문이 도는 현상이 있었다. 사실 []는 True, False가 아니니까 무조건 도는 게 맞기에... 착각했다.
        # 따라서 빈 리스트 값을 찾을 때는 if not List보단 if List == []를 써서 명시적으로 보이게끔 하는 게 나아보인다.
        while not first == []: 
            prev = lowest
            lowest = first
            first = list(first[0])
            if first == []:
                break
        return {'prev' : prev ,'lowest' : lowest}
```

2. 데이터프레임으로 바꿔주는 함수

### 수정 전
``` python
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

### 수정 후

```python
    def create_stock_dataFrame(self, prev, lowest, check): #check 매개변수를 추가했다. 노드 구성이 올바를 때 True, 그렇지 않을 때 False로 분기처리하도록 하였다.
        break_point = False
        if check :
            itemCount = self.check_validation(prev, lowest) #노드의 구성끼리 일치할 때 태그 갯수 가져오기
        else :
            itemCount = self.uncheck_validation(prev, lowest) #노드 구성끼리 불일치할 때 태그 max 갯수 가져오기

        dataFrame = pd.DataFrame(columns=self.column_name_list)

        if check: # 노드끼리 구성이 일치할 때
            for row in self.element_list:
                for item in row:
                        if len(item.text) < 2 and item.text == ' ':
                            break_point = True
                            break
                        elif item.text == '금감원(테스트)': #금감원 테스트를 지우도록 바꿨다.
                            break_point = True
                            break
                        else:
                            break_point=False
                if not break_point:
                    for i in range(0, itemCount, 1):
                        self.stock_list[i].append(row[i].text)
        else: # 노드끼리 구성이 불일치할 때
            column = list(dataFrame) # 데이터 프레임에서 컬럼 이름을 꺼내온다. 
            for data_list in prev: 
                for data_text in data_list: #데이터 프레임의 컬럼과 일치하게 들어가도록 한다.
                    #column 변수 안에서 data의 태그이름이 몇번째 인덱스인지 검색 후 해당 인덱스에 data의 value 값 집어넣기
                    self.stock_list[column.index(data_text.tag)].append(data_text.text) 
                    
            for i in range(0,len(self.stock_list),1): #데이터의 길이가 맞지 않으면 에러가 난다. 
                                                      #예를 들어 전체 데이터 행 갯수 max가 100인데 특정 A컬럼의 행 갯수가 34개 일 경우 인덱스 길이가 불일치하는 에러가 난다.
                                                      #이를 NoneData로 처리해준다.
                if len(self.stock_list[i]) < len(prev): # 전체 데이터 갯수인 len(prev)보다 해당 컬럼의 행 갯수가 더 적을 때
                    for j in range(0, len(prev) - len(self.stock_list[i]), 1):
                        self.stock_list[i].append('-') # NoneData를 임시로 '-' 값으로 넣어준다.
                        
        for i in range(0,itemCount,1):
            dataFrame.loc[:,self.column_name_list[i]] = self.stock_list[i]

        return dataFrame
```

3.객체 참조 오류
### 수정 전
```python
    def create_list(self, itemCount):##상위 함수는 = create_stock_dataFrame
        stock_list = []
        item = [] # 이미 이 순간 주소값이 값이 고정 됐다.
        for i in range(0,itemCount,1):
            stock_list.append(item)
        return stock_list
        
        """
        ex) 해당 코드의 문제점
        stock_list[0].append('1') 
        print(stock_list)
        
        출력 = ['1', '1', '1', '1']
        
        예상한 결과 = ['1', [], [], []]
        """
```

### 수정 후
```python 
    def create_list(self, itemCount):##상위 함수는 = create_stock_dataFrame
        stock_list = []
        for i in range(0,itemCount,1):
            item = [] #item 변수를 for문 안에 둠으로써 for문이 한 번 돌 때 주소 값이 초기화되도록 한다.
            stock_list.append(item)
        return stock_list
        
        """
        ex) 수정 후 결과
        stock_list[0].append('1') 
        print(stock_list)
        
        출력 = ['1', [], [], []]
        
        예상한 결과 = ['1', [], [], []].
        """
```python

